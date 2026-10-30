# Genome

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

Genome, operated by UAB "Maneuver LT" of Vilnius, is an Electronic Money Institution licensed and supervised by the Bank of Lithuania. It runs multi-currency personal and business accounts, issues Visa cards, and sells a merchant payments stack that is documented as six public API surfaces: the Host-to-Host API for direct card processing (AUTH, AUTH3D, SALE, SALE3D, SETTLE, REFUND, VOID and CHECK), the Payout API for sending funds to cardholders, the SEPA Payout API, the Query on Demand reporting API, a Verification of Payee API returning the EPC scheme's match outcomes, and a Berlin Group NextGenPSD2 dedicated interface offered free of charge to registered AISPs and PISPs. Batch transfers are submitted as ISO 20022 pain.001.001.03. Outcomes are carried in a proprietary 137-code response envelope rather than HTTP status codes, and Genome publishes no OpenAPI, AsyncAPI, MCP server or /.well-known/ document on any of its hosts.

**Areas:** Finance · Payments · Banking · Open Banking · PSD2 · SEPA · Card Processing · Payouts · Electronic Money Institution · Europe

## Genome on APIs.io

Everything profiled for Genome is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/genome/](https://apis.io/providers/genome/)** — or go straight to one of the APIs or properties below.

### APIs (8)

- [Genome](https://apis.io/apis/genome/genome/) — The Genome merchant payments programme as a whole — hosted payment pages, host-to-host card processing, payouts, reporting and the PSD2 dedicated interface,…
- [Genome Host-to-Host API](https://apis.io/apis/genome/genome-host-to-host-api/) — Direct card processing. One endpoint dispatching eight transaction types — AUTH, AUTH3D, SALE, SALE3D, SETTLE, REFUND, VOID and CHECK — accepting…
- [Genome Payouts API](https://apis.io/apis/genome/genome-payouts-api/) — Send funds to cardholders using full card data or a tokenized card. The synchronous response is pending; the final status arrives on the merchant callback.…
- [Genome SEPA Payout API](https://apis.io/apis/genome/genome-sepa-payout-api/) — SEPA credit transfer payouts by IBAN, with a companion transaction-status surface. Amounts can be read including or excluding the SEPA payout fee. No…
- [Genome Query on Demand API](https://apis.io/apis/genome/genome-query-on-demand-api/) — Historical transaction and chargeback reporting over a required unix time window, paged by page and limit with a 1,000-record page ceiling. No OpenAPI is…
- [Genome Verification of Payee API](https://apis.io/apis/genome/genome-verification-of-payee-api/) — Check a SEPA beneficiary's name or identifier against the account before sending a payment. Returns the EPC VOP scheme outcomes — match, close match, no…
- [Genome PSD2 API](https://apis.io/apis/genome/genome-psd2-api/) — Berlin Group NextGenPSD2 dedicated interface. Account Information Services, Payment Initiation Services for SEPA credit transfers, and Confirmation of…
- [Genome Hosted Payment Page](https://apis.io/apis/genome/genome-hosted-payment-page/) — Redirect checkout hosted by Genome, opened with an HS256 JWT signed with the SHA-256 digest of the payment page secret. Keeps card data out of merchant…

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### AsyncAPI (1)

Event-driven and webhook contracts.

- [Genome Webhooks](https://apis.io/asyncapis/genome/genome-webhooks/)

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [genome-mcp.yml](https://apis.io/servers/genome/genome-mcp-yml/)

#### Agentic access (1)

Which operations an agent can call, which act, and which need a human in the loop.

- [Genome Agentic Access](https://apis.io/agentic-access/genome/genome-agentic-access/)

#### JSON Schema (4)

The data models the API reads and writes.

- [HostToHostRequest](https://apis.io/schemas/genome/genome-host-to-host-request/)
- [PayoutRequest](https://apis.io/schemas/genome/genome-payout-request/)
- [PayoutResponse](https://apis.io/schemas/genome/genome-payout-response/)
- [TransactionResponse](https://apis.io/schemas/genome/genome-transaction-response/)

#### JSON-LD (1)

Linked-data contexts for the provider's vocabulary.

- [Genome Context](https://apis.io/jsonld/genome/genome-context/)

#### Collections (3)

Postman and Open Collection request collections.

- [Genome Payments API](https://apis.io/collections/genome/open-genome/)
- [Genome Payments Host-to-Host API](https://apis.io/collections/genome/open-genome-host-to-host-api/)
- [Genome Payments Host-to-Host Payouts API](https://apis.io/collections/genome/open-genome-payouts-api/)

#### Overlays (2)

OpenAPI overlays, including conversational phrasing for each operation.

- [Genome Host To Host Api Overlay](https://apis.io/overlays/genome/genome-host-to-host-api-overlay/)
- [Genome Payouts Api Overlay](https://apis.io/overlays/genome/genome-payouts-api-overlay/)

#### Governance rules (1)

Spectral rulesets for the provider's contracts.

- [Genome API Rules](https://apis.io/rules/genome/genome-rules/)

#### Security (3)

Authentication, trust centre, and vulnerability disclosure.

- [Genome Authentication](https://apis.io/security/genome/genome-authentication/)
- [Genome Domain Security](https://apis.io/security/genome/genome-domain-security/)
- [Genome Trust Center](https://apis.io/security/genome/genome-trust-center/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Genome Plans Pricing](https://apis.io/plans/genome/genome-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Genome Rate Limits](https://apis.io/rate-limits/genome/genome-rate-limits/)

#### FinOps (1)

What the API costs to run, by operation.

- [Genome Finops](https://apis.io/finops/genome/genome-finops/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://genome.eu/)
- [Documentation](https://developers.genome.eu/)
- [Getting started](https://developers.genome.eu/merchants/hosted-payment-page/)
- [API reference](https://gateway.genome.eu/help/cc)
- [Pricing](https://genome.eu/low-risk-merchant-account-pricing/)
- [Sign up](https://my.genome.eu/signup)
- [Support](https://support.genome.eu/hc/en-us/)
- [Blog](https://blog.genome.eu)
- [Security](https://blog.genome.eu/category/security/)
- [Terms of service](https://genome.eu/merchant-terms-of-use/)
- [Privacy policy](https://genome.eu/privacy-notice/)
- [GitHub](https://github.com/genome-eu)
- [Status](https://status.genome.eu)

## API Evangelist rating

**Kin Score 63.9** — strong · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 66.1 |
| Contract quality | 68.0 |
| Contract governance | 35.6 |
| Operational transparency | 44.7 |
| Developer ergonomics | 73.2 |
| Access clarity | 78.9 |

**Agent readiness 31.1** — agent-ready

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | derived (partial credit) |
| Reversibility Documented | — |
| MCP Server | — |
| Auth Clarity | bearer (partial credit) |
| Idempotency | documented (partial credit) |
| Error Semantics | documented (partial credit) |
| OpenAPI Examples | — |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | yes |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | — |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |
| Pagination Ratio | na |
| Operation Distinctiveness | na |

**Regulatory alignment 23.6** — Banking & Open Finance

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/genome/](https://apis.io/providers/genome/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Genome: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/genome/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
