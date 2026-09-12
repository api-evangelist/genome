---
name: genome-card-authorize-and-capture
description: Authorize a card payment with Genome, then capture it when goods ship, or release the hold if the order is cancelled. Covers the AUTH -> SETTLE / VOID flow on the Host-to-Host API.
api: Genome Host-to-Host API
operations:
  - processTransaction
generated: '2026-09-12'
method: generated
source: openapi/genome-host-to-host-api-openapi.yml + https://developers.genome.eu/merchants/host-to-host-api/ + https://developers.genome.eu/list-of-response-codes/
---

# Authorize and capture a card payment with Genome

Genome exposes one endpoint, `POST https://api.genome.eu/api/pf/host-to-host` (operationId
`processTransaction`), and selects behaviour with the `transaction_type` field. This skill covers the
two-step flow: hold the money at order time, take it when you ship.

Use SALE instead if you capture immediately — but note that after a successful SALE you can only
REFUND, never VOID.

## Before you start

- You need `merchant_account` and `merchant_password` from Genome. They go in the request **body**,
  not a header.
- Decide the `transaction_unique_id` before the first attempt and persist it. It is 11-45 characters
  on this flow and Genome rejects a reused value with code `3001`.
- While testing, set `currency` to `XTS`. That is the whole test mode — the host is production.
  Test cards: `4111111111111111` (Visa 2D), `5555555555554444` (Mastercard 2D),
  `4012000300001003` (Visa 3DS), `5191330000004415` (Mastercard 3DS), any 3-digit CVV, any expiry
  month with year 2020 or later.

## Step 1 — Authorize

POST to `/api/pf/host-to-host` with `transaction_type: AUTH` (or `AUTH3D` for 3-D Secure) plus
`api_version: 1`, `amount`, `currency`, `transaction_unique_id`, the card fields
(`card_number`, `card_exp_month`, `card_exp_year`, `cvv`, `card_holder`), the customer fields
(`first_name`, `last_name`, `user_email`, `user_ip`, `country`) and `callback_url`.

Content type is `application/json` or `application/x-www-form-urlencoded` — both are accepted.

**Read the body, not the status.** The HTTP status is 200 whether the call succeeded, was rejected or
was declined. Success is `code: 0`. Keep the `reference` from the response; every later step addresses
the transaction by it. On `AUTH3D` you also get a `redirect_url` — send the cardholder there.

The hold lasts about 7 days (Genome states this depends on the acquirer), after which Genome voids it
automatically.

## Step 2 — Decide from the response code

Look up `code` in `errors/genome-error-codes.yml`.

- `0` — authorized. Go to step 3.
- `1, 2, 3, 10, 11, 12, 1003, 3109, 3117, 3118, 3124, 3125, 3133, 6000, 7000, 7101, 7102, 7103` —
  **the outcome is UNKNOWN.** Do not retry the payment. Send a `CHECK` transaction (same endpoint,
  `transaction_type: CHECK`, addressing the transaction) and act on what it reports. Retrying instead
  of checking is how a cardholder gets charged twice.
- Anything in `3100`-`3299` — declined. See `errors/genome-decline-codes.yml` for what to do with each
  one, and which ones must never be shown to the buyer (`3102`, `3119`, `3120`, `3127`, `3128`, `3200`,
  `3201`, `3220`, `3221` tell a card-tester which control they tripped).
- Any other non-zero code — a request, account or processing error. Fix the request; do not retry
  blindly.

## Step 3 — Capture when you ship

POST again with `transaction_type: SETTLE` and the `reference` from step 1 as `base_reference`.
Funds move from the cardholder to the merchant account.

Error `3007` means you are outside the permitted interval between AUTH and SETTLE. `3010` means it was
already settled. `3016` means the settle amount is wrong.

## Step 3 (alternative) — Release the hold

If the order is cancelled before capture, POST with `transaction_type: VOID` and the `reference`.
A voided transaction never reaches the acquirer and never shows on the cardholder's statement.

`3004` means the reference cannot be voided; `3008` means it was already voided. If the transaction
was already settled, VOID no longer applies — use REFUND instead.

## Step 4 — Handle the callback

Genome POSTs the final outcome to your `callback_url` as
`application/x-www-form-urlencoded` with `token`, `reference`, `transaction_unique_id`, `status`
(`SUCCESS` / `DECLINED` / `ERROR` / `FRAUDED`), `code`, `message` and `checkSum`.

Verify `checkSum` before trusting it: drop the `checkSum` field, sort the remaining fields by key,
join them as `key=value` with `|`, append your Genome-issued private signature as a final segment, and
SHA-256 the result. Respond with HTTP 200 (body text `OK` is optional). If Genome does not get a 200
it re-sends, so make your handler idempotent on `reference`.

## What this API will not do for you

- There are no rate-limit headers and no published limits. Set your own ceiling.
- `transaction_unique_id` prevents duplicates; it does not replay a lost response. `CHECK` is the
  recovery path, not a retry.
- There is no reversal for a payout (see `genome-card-payout`), and a settled SALE cannot be voided.
