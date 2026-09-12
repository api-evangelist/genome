---
name: genome-reconcile-transactions
description: Pull historical Genome transactions and chargebacks for reconciliation through the Query on Demand API, and resolve any transaction whose outcome is unknown.
api: Genome Query on Demand API
operations:
  - processTransaction
generated: '2026-09-12'
method: generated
source: https://developers.genome.eu/merchants/query-on-demand-api/ + https://developers.genome.eu/merchants/host-to-host-api/ + https://developers.genome.eu/list-of-response-codes/
---

# Reconcile Genome transactions

Two surfaces do reconciliation work: the Query on Demand API for bulk history, and the `CHECK`
transaction type on the Host-to-Host API (operationId `processTransaction`) for resolving one
transaction whose outcome you do not know.

## Bulk history — Query on Demand

`POST https://api.genome.eu/api/pf/qod`, JSON or form-encoded.

Required: `api_version: 1`, `merchant_account`, `merchant_password`, `time_from`, `time_to` (unix
timestamps), and `qod_type` — either `transactions` or `chargebacks`.

Optional: `order` (`asc` default, or `desc`), `page` (1-12), `limit` (1-1000, default 1000).

## Paginate carefully

There is no total-count field and no next-page link. The only way to know whether another page exists
is to request it and see whether you got fewer than `limit` rows. Walk pages until a short page comes
back, and keep `order` stable across the walk so rows do not shift under you.

`page` maxes out at 12, so a window can surface at most 12,000 records. If a time window returns 12
full pages, narrow `time_from`/`time_to` and walk it again rather than assuming you have everything.

Code `5002` means `time_from` and `time_to` are in the wrong order. Code `5000` is a general QoD error.

## What comes back

Each row carries `transaction_type` (SALE, AUTH, AUTH3D, SALE3D, SETTLE, REFUND, VOID), `status`
(SUCCESS, DECLINED, ERROR, MALFORMED, FRAUDED, CHARGEDBACK, REFUNDED, VOIDED, PARTIAL-REFUNDED,
WRONGREF), `mode` (CC, TOKEN, REF, CASCADE), `reference`, `base_reference`, `amount` and `currency`.

`base_reference` is the join: a SETTLE, REFUND or VOID row points back at the AUTH or SALE it derives
from. Build your ledger by grouping on it.

Note that the webhook feed identifies transactions by a `transaction_id` (uint64) while this surface
uses `reference` (20-character string), and Genome publishes no mapping between the two. Pick one as
your primary key and store the other alongside it from the moment you first see both.

## Resolving one unknown transaction — CHECK

Whenever a write returned one of the unknown-outcome codes — `1, 2, 3, 10, 11, 12, 1003, 3109, 3117,
3118, 3124, 3125, 3133, 6000, 7000, 7101, 7102, 7103` — do not retry the payment. POST to
`https://api.genome.eu/api/pf/host-to-host` with `transaction_type: CHECK` and the transaction's
identifier, and act on the status it returns. Code `3014` means no transaction was found for your
CHECK request, which is itself the answer: nothing was created.

For SEPA payouts the equivalent status surface is `POST https://api.genome.eu/api/mp/transaction`.
Genome notes that `https://api.genome.eu/api/mp/transactions` reports amounts **including** the SEPA
payout fee, while `https://api.genome.eu/api/mp/transactions/ra` reports the same transactions
**excluding** it — use the second one when reconciling against what the counterparty received.

## Reconciliation error band

Codes `4000`-`4007` are the reconciliation band: data not ready (`4001`), total amount mismatch
(`4002`), total count mismatch (`4003`), balance not available (`4005`), exceeded amount (`4007`).
`4001` is the one to retry later; the mismatches are real discrepancies to investigate.

## Remember

Every call returns HTTP 200. Read `code` from the body — `0` is the only success.
