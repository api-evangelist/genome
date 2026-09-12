---
name: genome-card-payout
description: Send funds to a cardholder through the Genome Payout API, using full card data or a stored token, and resolve the final status. This is an irreversible money movement — read the guardrails first.
api: Genome Payouts API
operations:
  - initPayout
generated: '2026-09-12'
method: generated
source: openapi/genome-payouts-api-openapi.yml + https://developers.genome.eu/merchants/payout-api/ + https://developers.genome.eu/list-of-response-codes/
---

# Send a card payout with Genome

`POST https://api.genome.eu/api/pf/payout` (operationId `initPayout`) pushes funds to a cardholder.

## Read this before calling it

**There is no reversal.** Genome publishes no cancel, void, reverse or recall operation for a payout.
Once the request is accepted the money is on its way. If you are running this from an agent, require
human approval before the call — not after.

**The synchronous response is not the outcome.** A successful submission returns `status: pending`.
The real result arrives later on your callback URL.

## Prepare the request

Required fields: `api_version: 1`, `method: init`, `merchant_account`, `merchant_password`,
`transaction_unique_id`, `amount`, `currency`, `callback_url` (must be https), and either full card
data (`card[card_number]`, `card[card_exp_month]`, `card[card_exp_year]`, `card[card_cvv]`,
`card[card_holder]`) or a stored `card[card_token]` — not both.

Content type is `application/json` or `application/x-www-form-urlencoded`.

Generate and persist `transaction_unique_id` before the first attempt. Genome enforces uniqueness and
returns code `3001` if you reuse it — which means a blind retry after a timeout will be rejected, not
deduplicated.

Test with `currency: XTS` on the same production host; there is no sandbox endpoint.

## Send it and read the envelope

The HTTP status is 200 regardless of outcome. Read `code` from the body:

- `0` — accepted, `status` will be `pending`. Record `session_id` and wait for the callback.
- `3300`, `3310`, `3311`, `3330` — the payout band: general payout error, payout validation error,
  invalid mid reference, unregistered card. The money did not move. Fix and resubmit with a **new**
  `transaction_unique_id`.
- `1, 2, 3, 10, 11, 12, 1003, 3109, 3117, 3118, 3124, 3125, 3133, 6000, 7000, 7101, 7102, 7103` —
  **UNKNOWN.** Do not resubmit. Check the transaction status before doing anything else; a duplicate
  payout cannot be taken back.
- `2008` — your source IP is not allowlisted for this surface. That is configuration, not a transient
  failure; contact Genome rather than retrying.

Full registry: `errors/genome-error-codes.yml`.

## Resolve the final status

Genome POSTs the outcome to `callback_url` with `token`, `reference`, `transaction_unique_id`,
`status` (`SUCCESS` / `DECLINED` / `ERROR` / `FRAUDED`), `code`, `message` and `checkSum`.

Verify `checkSum` the same way as any Genome callback: remove `checkSum`, sort the remaining fields by
key, join as `key=value` with `|`, append your private signature, SHA-256. Reply HTTP 200 (body `OK`
optional) or Genome will re-send.

If you cannot expose a callback endpoint, poll instead — but note that the polling surface for payouts
is documented on the SEPA Payout page (`POST https://api.genome.eu/api/mp/transaction`), not the card
payout page.

## SEPA instead of card

To pay out to a bank account rather than a card, use the SEPA Payout API
(`POST https://api.genome.eu/api/mp/payout`, `type: sepa`) with `receiver_iban`, `receiver_name` and
`transfer_description`. Before you send it, consider running a Verification of Payee check at
`POST https://api.genome.eu/api/mp/payee/verification` — on a `close match` result Genome returns
`payee_suggested_name`, which is the name you should use. Nothing links the verification to the
payout automatically; you have to carry that association yourself.
