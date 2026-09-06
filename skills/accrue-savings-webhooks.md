---
name: accrue-savings-webhooks
description: Register, verify and replay Accrue webhooks — the twenty topics, the wildcard trap, and how to recover events you missed.
generated: '2026-09-06'
method: generated
source: openapi/accrue-savings-merchant-api-openapi.yaml
api: Accrue Merchant API
base_url: https://merchant-api.accruesavings.com
operations:
  - createWebhook
  - getWebhooks
  - getWebhook
  - updateWebhook
  - deleteWebhook
  - listWebhookEvents
  - getWebhookEvent
---

# Accrue webhooks

Accrue's event contract lives inside the OpenAPI 3.1 document's top-level `webhooks` object, with a
full request schema for each of the twenty topics. There is no AsyncAPI document.

## Register a subscription

`POST /api/v1/webhooks` (`createWebhook`) with your endpoint URL and the topics you want. List with
`GET /api/v1/webhooks` (`getWebhooks`), amend with `PATCH /api/v1/webhooks/{webhookId}`
(`updateWebhook`), remove with `DELETE` (`deleteWebhook`).

An unrecognised topic returns 400 `InvalidTopics`; an unknown subscription returns 404.

## The topics

Payments: `paymentIntentCreated`, `paymentIntentUpdated`, `paymentCreated`, `paymentUpdated`,
`paymentCaptured`, `refundCreated`, `refundUpdated`.

Identity: `kycCreated`, `kycPending`, `kycAwaitingDocuments`, `kycManualReview`, `kycApproved`,
`kycDeclined`.

Wallet money movement: `transactionCleared`, `transactionFailed`. The `transactionFailed` payload
carries a `fee` object and a `failureReason` from `TransactionFailureReason` — `HardDecline`
(do not retry this instrument), `SoftDecline` (a later attempt may succeed), `InsufficientFunds`,
`Expired`, `Canceled`, `Reversed`, `Unknown`. The hard/soft split is the retry decision; make it
from this field, not from the HTTP status.

Counterparties: `counterpartyIncomingPayment`, `counterpartyPayoutCreated`,
`counterpartyPayoutSent`, `counterpartyPayoutCompleted`, `counterpartyPayoutReturned`.

## The wildcard trap

Subscribing with `"*"` means every topic Accrue adds later starts arriving without you changing
anything. Accrue has shipped new topics this way twice in one week (August 2026). **Your handler
must ignore unknown topics rather than throwing** — a strict switch on topic name will start
erroring the day Accrue ships the next one.

## Verification

The getting-started guide instructs you to always verify webhook signatures with a constant-time
comparison. The signing header name, the algorithm and the secret-provisioning flow are **not
published** in the public documentation — you must get them from Accrue. Do not ship an endpoint
that trusts unverified bodies because the public docs did not tell you how to verify.

## Replay and catch-up

`GET /api/v1/webhook-events` (`listWebhookEvents`) and
`GET /api/v1/webhook-events/{webhookEventId}` (`getWebhookEvent`) let you read delivered events back
out of Accrue. Use them to reconcile after an outage on your side instead of asking Accrue to
resend. Page with `page[limit]` (max 50) and `page[offset]`.

## Payload shape

Every event body is a JSON:API document — a `WebhookEvent` resource in `data`, with the event's own
resource in `included`. Media type `application/vnd.api+json`.
