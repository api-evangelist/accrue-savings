---
name: accrue-savings-onboard-user-kyc
description: Onboard an end customer onto Accrue — create the user, open a wallet, run KYC to approval, and handle document verification and step-up identity challenges.
generated: '2026-09-06'
method: generated
source: openapi/accrue-savings-merchant-api-openapi.yaml
api: Accrue Merchant API
base_url: https://merchant-api.accruesavings.com
operations:
  - createUser
  - getUser
  - createWallet
  - acceptKycDisclosureDocuments
  - createKycApplication
  - getKycStatus
  - getDocumentVerificationLink
  - completeDocumentVerification
  - createIdentityVerificationChallenge
  - submitIdentityVerificationChallenge
  - applyVerifiedProfileUpdate
---

# Onboard a user and get them through KYC

A wallet cannot be funded until the user behind it has passed KYC. In sandbox this is also true —
complete KYC first, then fund.

## 1. Create the user

`POST /api/v1/users` (`createUser`). The user carries an `attachedProfile` with `referenceId`,
`email` and `phoneNumber`. Your own `referenceId` is what lets you address the user later as
`:userReference`.

Failure modes: 409 `UserAlreadyExists`, 400 `PhoneNumberBanned`, 400 `InvalidUser`,
500 `IdentityProviderFailure`.

## 2. Create the wallet

`POST /api/v1/wallets` (`createWallet`). Failure modes: 400 `WalletAlreadyExists`,
400 `MultipleActiveWallets`, 400 `MerchantIsDisabled`.

## 3. Disclosures, then the KYC application

`POST /api/v1/users/:userId/disclosures/kyc` (`acceptKycDisclosureDocuments`) records the user's
acceptance of the required disclosures.

`POST /api/v1/users/:userId/kyc/application` (`createKycApplication`) opens the application. This
fires the `kycCreated` webhook.

## 4. Follow the outcome — via webhooks, not polling

Seven topics report every state the application can reach:

| Topic | Meaning |
|---|---|
| `kycCreated` | application opened |
| `kycPending` | running through automated checks |
| `kycAwaitingDocuments` | the user must upload identity documents |
| `kycManualReview` | automation could not decide; a human is reviewing |
| `kycApproved` | passed — banking features unlock |
| `kycDeclined` | failed — banking features stay locked |

`GET /api/v1/users/:userId/kyc/status` (`getKycStatus`) is the point-in-time read if you need one.

## 5. If documents are required

`POST /api/v1/users/:userId/kyc/document-verification-link` (`getDocumentVerificationLink`) returns
a link to send the user to. When they finish, call
`POST /api/v1/users/:userId/kyc/document-verification/complete` (`completeDocumentVerification`).

## 6. Step-up verification for profile changes

Changing a verified profile requires a challenge, not just a PATCH:

1. `POST /api/v1/users/:userIdentifier/identity-verification/challenges`
   (`createIdentityVerificationChallenge`)
2. `POST /api/v1/users/:userIdentifier/identity-verification/challenges/:challengeId/submissions`
   (`submitIdentityVerificationChallenge`) — the result carries `passed`, a `verificationToken`,
   an `expiresAt` (present only when `passed` is true) and, on failure, a `failureReason` of
   `IncorrectAnswer`, `ChallengeExpired` or `MaxAttemptsExceeded`.
3. `POST /api/v1/users/:userIdentifier/identity-verification/profile-updates`
   (`applyVerifiedProfileUpdate`) with the token.

Failure modes: 403 `InsufficientVerificationData`, 400 `CooldownActive`, 400 `ChallengeExpired`,
400 `MaxAttemptsExceeded`, 404 `ChallengeNotFound`.

The plain `PATCH /api/v1/users/:userReference/attached-profile` (`updateUser`) exists for
non-verified fields.

## Sandbox values

Test phone numbers follow `xxx-555-xxxx` (the middle segment must be `555`) and the SMS code is
always `0001`. For sandbox KYC, use an age over 18, a random US address and SSN `123456789`.
Published at https://docs.byaccrue.com/docs/integration/sandbox/instructions.
