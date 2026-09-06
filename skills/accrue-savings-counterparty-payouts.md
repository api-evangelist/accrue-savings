---
name: accrue-savings-counterparty-payouts
description: Pay a counterparty out over bank rails with Accrue, track the payout to settlement or return, and reconcile incoming funding without polling.
generated: '2026-09-06'
method: generated
source: openapi/accrue-savings-merchant-api-openapi.yaml
api: Accrue Merchant API
base_url: https://merchant-api.accruesavings.com
operations:
  - createCounterparty
  - listCounterparties
  - getCounterparty
  - updateCounterparty
  - deleteCounterparty
  - createCounterpartyPayout
  - listCounterpartyPayouts
  - createCounterpartyTransfer
  - listCounterpartyTransfers
  - getCounterpartyTransfer
---

# Move money to a counterparty

## 1. Create the counterparty

`POST /api/v1/counterparties` (`createCounterparty`). Read it back with
`GET /api/v1/counterparties/:counterpartyId` (`getCounterparty`) — since 2026-08 the resource
exposes both its Accrue bank account details and its settlement account details.

Failure modes: 409 `CounterpartyAlreadyExists`, 400 `ValidationFailed`.

## 2. Pay out

`POST /api/v1/counterparties/:counterpartyId/payouts` (`createCounterpartyPayout`).

**Send an `idempotencyKey`.** This is one of only four operations in the whole API that accepts one,
and it is the one where a duplicate costs real money. Keys are effective for 48 hours; a conflict
returns `IdempotencyConflict` (409).

The deprecated `remit` flag on disbursement entries always returns `true` and will be removed —
ignore it.

## 3. Follow the payout to a terminal state

A payout is not done when the call returns 201. Subscribe to all four lifecycle topics:

| Topic | Meaning |
|---|---|
| `counterpartyPayoutCreated` | submitted; `status` is `Approved`, `NeedsApproval` or `Processing` depending on your approval configuration |
| `counterpartyPayoutSent` | funds have left the bank but are **not** reconciled — a payout can sit here for days depending on the rail, and can still be returned |
| `counterpartyPayoutCompleted` | terminal success: reconciled to a posted bank transaction, settled at the receiving bank |
| `counterpartyPayoutReturned` | terminal failure — covers returned, reversed, cancelled, denied **and** failed. Read `status` to see which; do not infer it from the topic name |

**There is no reversal operation for a payout.** Once it is sent, a return is a bank-side outcome
you observe, not an action you take. This is the least reversible thing in the API — get the
idempotency key right rather than planning to undo.

`GET /api/v1/counterparties/:counterpartyId/payouts` (`listCounterpartyPayouts`) is the catch-up
read if you miss an event.

## 4. Reconcile incoming funding

Subscribe to `counterpartyIncomingPayment` instead of polling the counterparty balance. It fires
after the payment is recorded, so the balance already reflects it when the event arrives. Payload:
`counterpartyId`, `merchantId`, `amount` (cents), `currency` (ISO 4217), `direction`
(`credit`/`debit`), `method` (the rail, e.g. `ach` or `wire`), `asOfDate` (bank settlement date,
YYYY-MM-DD) and `externalIncomingPaymentId` for reconciliation against your bank records.

If you subscribed with the `*` wildcard you are already receiving this topic — your handler must
tolerate unknown topics.

## 5. Internal transfers

`POST /api/v1/counterparty-transfers` (`createCounterpartyTransfer`) moves funds between
counterparties and also accepts an `idempotencyKey`. Cross-tenant attempts return
403 `CrossTenantTransfer`.

## 6. Deleting

`DELETE /api/v1/counterparties/:counterpartyId` (`deleteCounterparty`) is a **soft** delete, and it
refuses while the counterparty still holds a balance — 409 `CounterpartyHasBalance`. Pay the balance
out first.

## Sandbox

`POST /api/v1/simulation/counterparties/{counterpartyId}/deposits`
(`simulateCounterpartyDeposit`) fabricates an incoming deposit so you can exercise the
reconciliation path end to end without a real bank transfer.
