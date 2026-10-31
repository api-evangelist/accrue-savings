# Accrue Savings

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Accrue Savings (legally Accrue Money, Inc., now trading as Accrue at byaccrue.com) is a New York fintech that powers customer loyalty at the payment layer with a branded stored-value wallet merchants embed in their own checkout. Its platform lets brands hold customer funds in a wallet, accept payments over wallet and bank rails, run a rules-driven reward engine, return refunds as closed-loop credit, and let shoppers pre-fund or crowdfund a future purchase. The Accrue Merchant API is a public, JSON:API-shaped OpenAPI 3.1 contract covering wallets, payment intents, payments, users, KYC and identity verification, counterparties and payouts, linked accounts, rewards, gifts, sweepstakes, external transactions and webhooks, with a sandbox environment, mobile SDKs for iOS/Android/React Native/Expo, and embeddable web wallet widgets. Banking services are provided by Cross River Bank, Member FDIC.

**Areas:** Company · Payments · Loyalty · Wallets · Stored Value · Rewards · Banking · Fintech · Webhook · Loyalty & Incentives

## Accrue Savings on APIs.io

Everything profiled for Accrue Savings is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/accrue-savings/](https://apis.io/providers/accrue-savings/)** — or go straight to one of the APIs or properties below.

### APIs (18)

- [Accrue Savings Banking API](https://apis.io/apis/accrue-savings/accrue-savings-banking-api/) — Banking APIs provide KYC (Know Your Customer) verification functionality to enable users to comply with financial regulations when using banking features.…
- [Accrue Savings Counterparties API](https://apis.io/apis/accrue-savings/accrue-savings-counterparties-api/) — Counterparties define bank accounts where funds are settled for captured payments. Settlement details live on `externalBankAccount`. There can be multiple…
- [Accrue Savings Counterparty Transfers API](https://apis.io/apis/accrue-savings/accrue-savings-counterpartytransfers-api/) — Counterparty Transfers represent movements of funds between two counterparties owned by the same partner. Use these endpoints to initiate, list, and…
- [Accrue Savings External Transactions API](https://apis.io/apis/accrue-savings/accrue-savings-externaltransactions-api/) — External transactions are records of transfers that happened outside the Accrue system. E.g. purchases in an online shop using a non-Accrue payment method.
- [Accrue Savings Gifts API](https://apis.io/apis/accrue-savings/accrue-savings-gifts-api/) — Gifts expose remaining spendable balance for a scanned lookUpId. Point-of-sale remaining lookup is GET /api/v1/gifts/{lookUpId}. Spend still uses payment…
- [Accrue Savings Identity Verification API](https://apis.io/apis/accrue-savings/accrue-savings-identity-verification-api/) — Identity Verification provides knowledge-based authentication for sensitive account changes. Use these endpoints to challenge a user with profile and wallet…
- [Accrue Savings Payments API](https://apis.io/apis/accrue-savings/accrue-savings-payments-api/) — Payments are the realization of payment intents, representing the actual transfer or authorization of funds. This resource encapsulates the details of…
- [Accrue Savings Rewards API](https://apis.io/apis/accrue-savings/accrue-savings-rewards-api/) — Partner-issued rewards let trusted integrations credit customer wallets or create pre-issued rewards for recipients identified by phone number. When a…
- [Accrue Savings Simulations API](https://apis.io/apis/accrue-savings/accrue-savings-simulations-api/) — Simulations are used to simulate payments authorizations and captures for Card Rails (Virtual Debit Cards).
- [Accrue Savings Sweepstakes API](https://apis.io/apis/accrue-savings/accrue-savings-sweepstakes-api/) — Sweepstakes campaigns give users the chance to win prizes by participating in merchant-sponsored promotions. Each campaign is linked to a specific merchant…
- [Accrue Savings Users API](https://apis.io/apis/accrue-savings/accrue-savings-users-api/) — Users represent the end users and are the parent container of Wallets. A user is automatically created when the end-user signs into the Accrue product…
- [Accrue Savings Wallets API](https://apis.io/apis/accrue-savings/accrue-savings-wallets-api/) — Wallets are where users save money and collect rewards for future payments to merchants. Each wallet is linked to a specific merchant and tracks the balance…
- [Accrue Savings Webhook Topics API](https://apis.io/apis/accrue-savings/accrue-savings-webhook-topics-api/) — Webhook Topics
- [Accrue Savings Webhooks API](https://apis.io/apis/accrue-savings/accrue-savings-webhooks-api/) — Webhook management APIs
- [Accrue Savings Widgets API](https://apis.io/apis/accrue-savings/accrue-savings-widgets-api/) — <p>Widgets are components that are embedded into applications to enrich the user experience. Some of those require additional data loaded through API…
- [Accrue Savings Linked Accounts API](https://apis.io/apis/accrue-savings/accrue-savings-linked-accounts-api/) — Linked Accounts represent payment methods that users have connected to their Accrue account. These accounts can be used for funding payments or topping up…
- [Accrue Savings Payment Intents API](https://apis.io/apis/accrue-savings/accrue-savings-payment-intents-api/) — Payment Intents represent a commitment to pay a specified amount, allowing for a structured process to handle payments from initiation to completion.
- [Accrue Savings Webhook Events API](https://apis.io/apis/accrue-savings/accrue-savings-webhook-events-api/) — Webhook Event management APIs

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### AsyncAPI (1)

Event-driven and webhook contracts.

- [Accrue Savings Merchant Api Webhooks](https://apis.io/asyncapis/accrue-savings/accrue-savings-merchant-api-webhooks/)

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [Accrue Merchant API — candidate MCP tool surface](https://apis.io/servers/accrue-savings/accrue-merchant-api-candidate-mcp-tool-surface/)

#### Overlays (1)

OpenAPI overlays, including conversational phrasing for each operation.

- [Accrue Savings Merchant Api Overlay](https://apis.io/overlays/accrue-savings/accrue-savings-merchant-api-overlay/)

#### Security (3)

Authentication, trust centre, and vulnerability disclosure.

- [Accrue Savings Authentication](https://apis.io/security/accrue-savings/accrue-savings-authentication/)
- [Accrue Savings Domain Security](https://apis.io/security/accrue-savings/accrue-savings-domain-security/)
- [Accrue Savings Trust Center](https://apis.io/security/accrue-savings/accrue-savings-trust-center/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Accrue Savings Plans Pricing](https://apis.io/plans/accrue-savings/accrue-savings-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Accrue Savings Rate Limits](https://apis.io/rate-limits/accrue-savings/accrue-savings-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.byaccrue.com/)
- [Documentation](https://docs.byaccrue.com/)
- [Getting started](https://docs.byaccrue.com/getting-started-api)
- [API reference](https://docs.byaccrue.com/api/)
- [Support](https://www.byaccrue.com/contact)
- [Changelog](https://docs.byaccrue.com/changelog)
- [Trust centre](https://trust.byaccrue.com/)
- [Terms of service](https://files.byaccrue.com/048c1405-0e96-4033-af8f-ad353e48646f.pdf)
- [Privacy policy](https://files.byaccrue.com/61d09791-95f0-4030-8490-d7e7d061592c.pdf)
- [GitHub](https://github.com/accrue-savings)
- [Status](https://status.accruesavings.com/)

## API Evangelist rating

**Kin Score 53.6** — developing · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 73.2 |
| Contract quality | 53.2 |
| Contract governance | 18.2 |
| Operational transparency | 55.3 |
| Developer ergonomics | 70.8 |
| Access clarity | 28.9 |

**Agent readiness 39.2** — agent-ready (gated down from agent-native)

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | — |
| Reversibility Documented | documented (partial credit) |
| MCP Server | — |
| Auth Clarity | bearer (partial credit) |
| Idempotency | documented (partial credit) |
| Error Semantics | verified |
| OpenAPI Examples | partial |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | yes |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | yes |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | — |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |
| Pagination Ratio | — |
| Operation Distinctiveness | distinct |

**Regulatory alignment 30.4** — Payments

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/accrue-savings/](https://apis.io/providers/accrue-savings/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Accrue Savings: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/accrue-savings/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
