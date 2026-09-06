---
name: accrue-savings-refund-payment
description: Refund a captured Accrue payment safely, including partial refunds, disbursement splits and the one operation in this API where an idempotency key is required.
generated: '2026-09-06'
method: generated
source: openapi/accrue-savings-merchant-api-openapi.yaml
api: Accrue Merchant API
base_url: https://merchant-api.accruesavings.com
operations:
  - getPayment
  - refund
  - listCounterparties
---

# Refund an Accrue payment

## Preconditions — check these first

`POST /api/v1/payments/{paymentId}/refund` (`refund`) is accepted **only** when the Payment is in
`Processing` or `Sent` status **and** has at least one successful capture. Read the current state
with `GET /api/v1/payments/{paymentId}` (`getPayment`) before you call.

## Idempotency is required here

The request body attribute `data.attributes.idempotencyKey` is **required** on refund. It is a
string of 1-255 characters; a UUIDv4 is recommended. If a refund with the same key already exists,
the existing refund is returned rather than a second one being created. Keys remain effective for
48 hours.

This is not a header. Accrue has no `Idempotency-Key` header anywhere in the API — the key travels
in the JSON:API body. A replayed key that conflicts returns `IdempotencyConflict` (409).

Derive the key from something stable on your side (the order id plus a refund sequence number, for
example) so a retry after a timeout reuses it.

## Partial refunds

Send `data.attributes.amount` in cents. The refund amount cannot exceed the captured amount, and the
sum of all non-failed refunds cannot exceed the captured amount either.

## Disbursements

For pay-by-wallet refunds you may optionally send `data.attributes.disbursement` — an array of
`{counterpartyId, amount}` naming which counterparties to debit and for how much. The disbursement
amounts must sum exactly to the refund amount. Omit it and Accrue derives proportional disbursements
automatically. Use `listCounterparties` to resolve ids.

Note that the `remit` boolean on a disbursement entry is deprecated: it always returns `true`,
all disbursements are remitted directly, and it will be removed in a future version. Do not build
on it.

## Reading the result — the trap

**Refunding never changes the Payment's status.** You cannot use the Payment status to tell whether
it has been refunded. The call returns a separate `Refund` object with its own independent status;
it can also be pulled back through `getPayment` with `include`.

Track it asynchronously through the `refundCreated` and `refundUpdated` webhook topics.

## Failure modes

400 `InvalidRefund`, 400 `InvalidAmount`, 400 `IllegalOperation` (wrong payment status),
409 `IdempotencyConflict`, 400 `InsufficientBalance`.
