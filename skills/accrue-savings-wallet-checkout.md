---
name: accrue-savings-wallet-checkout
description: Take a payment from an Accrue stored-value wallet at checkout, from payment intent through authorization, capture and completion.
generated: '2026-09-06'
method: generated
source: openapi/accrue-savings-merchant-api-openapi.yaml
api: Accrue Merchant API
base_url: https://merchant-api.accruesavings.com
operations:
  - createPaymentIntent
  - authorizePayment
  - getPayment
  - capturePayment
  - getVirtualDebitCard
  - completePayment
  - cancelPayment
---

# Charge an Accrue wallet at checkout

All requests carry `Authorization: Bearer <Client Secret>` and `Client-ID: <your client id>`, and
use `Content-Type: application/vnd.api+json`. Amounts are integers in the smallest currency unit
(cents for USD). Never run this against production while testing — point at
`https://merchant-api-sandbox.accruesavings.com` instead.

## 1. Create the payment intent

`POST /api/v1/payment-intents` (`createPaymentIntent`)

Send `amount` and **exactly one** of `walletId` or `lookUpId`. `lookUpId` is a scanned string: it is
resolved as a wallet barcode first, then as a gift identifier. Never send a gift `data.id` as
`walletId` — the contract warns about this explicitly.

For a gift `lookUpId`, first call `getGiftByLookUpId` (`GET /api/v1/gifts/{lookUpId}`) to read the
remaining balance. Gift look-ups skip KYC.

Read `balance.available` from the response: it is the remaining spendable amount.

Returns 201. Failure modes: 400 `InvalidAmountOrWalletIdentifier`, 403 `WalletAccessDenied`,
404 `WalletIdentifierNotFound`, 400 `WalletIdentifierExpired`, 400 `GiftIdentifierExpired`,
400 `GiftIdentifierMaxUsesExceeded`.

## 2. Authorize

`POST /api/v1/payment-intents/{paymentIntentId}/authorize` (`authorizePayment`)

Creates a `Payment` and, for wallet intents, reserves wallet funds. If the intent was already
authorized, the existing payment is returned rather than re-authorizing — this call is safe to
repeat.

When a card processor is configured, processor authorization runs **inline** before the response.
On decline the payment is marked `Failed` and the endpoint returns **402**. Do not treat 402 as a
transport error and retry blindly.

Optional `data.attributes.channel` and `data.attributes.risk.deviceSessionId` are forwarded to
wallet authorization.

## 3a. Wallet rails — capture

`POST /api/v1/payments/{paymentId}/capture` (`capturePayment`). Returns 200, or 402 if the capture
fails.

If you need more than you authorized, use
`PATCH /api/v1/payments/{paymentId}/increase-authorization` (`increaseAuthorization`) before
capturing.

## 3b. Card rails — virtual debit card

If your integration uses card rails, load the card with
`GET /api/v1/payments/{paymentId}/card` (`getVirtualDebitCard`) from the Payment's
`links.virtualDebitCard`, then run a card-present authorization against it. For a gift intent this
is what actually spends the gift reserve — authorize alone does not debit it.

Once you have captured all the funds from the card, call
`POST /api/v1/payments/{paymentId}/complete` (`completePayment`). This marks the payment complete
and releases the remaining reserved funds back to the user. **Card rails only.**

## 4. If it goes wrong

`POST /api/v1/payments/{paymentId}/cancel` (`cancelPayment`) works **only** while the Payment is in
`Created` or `Waiting` status, and always cancels the full amount — there is no partial cancel. On
card rails, only call cancel if the manual capture at your processor failed.

Once money has moved, cancel is no longer the tool: use the refund skill.

## Conventions that apply to every step

- **Idempotency:** none of these operations accepts an idempotency key. `authorizePayment` is
  naturally safe to repeat (it returns the existing payment). `capturePayment` and `cancelPayment`
  are not — record the `paymentId` and re-read state with `getPayment` before retrying.
- **Errors:** `{id, status, code, title, detail, meta}`. `code` may be empty on framework-level
  errors; fall back to `title`. Log the `id` — it is unique to that occurrence.
- **Rate limit:** 10,000 requests/minute per IP; 429 on exhaustion with no Retry-After header.
- **Retries:** 408, 429 and 5xx only, with exponential backoff and jitter. 402 is a decision, not a
  transient failure.
- **Async truth:** track final state through the `paymentCreated`, `paymentUpdated` and
  `paymentCaptured` webhook topics rather than polling.
