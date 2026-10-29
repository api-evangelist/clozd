# Clozd

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

Clozd is a Lehi, Utah based decision-intelligence and win-loss analysis platform that collects structured buyer feedback — through human-led live interviews, AI-assisted Flex interviews, and autonomous Flow interviews — and turns it into decision drivers, competitor sentiment, win rates, and verbatim buyer quotes for sales, marketing, product, and customer-success teams. The platform integrates with Salesforce, HubSpot, Microsoft Dynamics, Gong, Slack, Calendly, Outlook, and Gmail, and exposes two public programmatic surfaces: the versioned Clozd Data API (v1/v2/v3) documented with OpenAPI 3.0.3 and Swagger UI at app.clozd.com for importing deals and participants and exporting programs, deals, touchpoints, responses, transcripts, and competitors; and a hosted, OAuth 2.0 protected Model Context Protocol server at mcp.clozd.com that gives Claude, ChatGPT, Cursor, Copilot, Windsurf, Antigravity, and Gemini CLI direct tool access to the same win-loss data.

**Areas:** Win-Loss Analysis · Customer Feedback · Decision Intelligence · Sales Intelligence · Market Research · Competitive Intelligence · Voice of Customer · Revenue Intelligence · Software-as-a-Service · MCP · Agent-Native

## Clozd on APIs.io

Everything profiled for Clozd is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/clozd/](https://apis.io/providers/clozd/)** — or go straight to one of the APIs or properties below.

### APIs (8)

- [Clozd MCP Server](https://apis.io/apis/clozd/clozd-mcp-server/) — Hosted remote Model Context Protocol server exposing 19 documented read tools over Clozd win-loss data — programs, deals, responses, response summaries,…
- [Clozd /programs API](https://apis.io/apis/clozd/clozd-programs-api/) — The /programs API from Clozd — 1 operation(s) for /programs.
- [Clozd /programs/:program Id/competitors API](https://apis.io/apis/clozd/clozd-programs-program-id-competitors-api/) — The /programs/:program_id/competitors API from Clozd — 1 operation(s) for /programs/:program_id/competitors.
- [Clozd /programs/:program Id/deals API](https://apis.io/apis/clozd/clozd-programs-program-id-deals-api/) — The /programs/:program_id/deals API from Clozd — 1 operation(s) for /programs/:program_id/deals.
- [Clozd /programs/:program Id/deals/:deal ID API](https://apis.io/apis/clozd/clozd-programs-program-id-deals-deal-id-api/) — The /programs/:program_id/deals/:deal_id API from Clozd — 1 operation(s) for /programs/:program_id/deals/:deal_id.
- [Clozd /programs/:program Id/deals/import API](https://apis.io/apis/clozd/clozd-programs-program-id-deals-import-api/) — The /programs/:program_id/deals/import API from Clozd — 1 operation(s) for /programs/:program_id/deals/import.
- [Clozd /programs/:program Id/touchpoints API](https://apis.io/apis/clozd/clozd-programs-program-id-touchpoints-api/) — The /programs/:program_id/touchpoints API from Clozd — 1 operation(s) for /programs/:program_id/touchpoints.
- [Clozd /programs/:program Id/touchpoints/:touchpoint ID API](https://apis.io/apis/clozd/clozd-programs-program-id-touchpoints-touchpoint-id-api/) — The /programs/:program_id/touchpoints/:touchpoint_id API from Clozd — 1 operation(s) for /programs/:program_id/touchpoints/:touchpoint_id.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [clozd](https://apis.io/servers/clozd/clozd/)

#### Agentic access (1)

Which operations an agent can call, which act, and which need a human in the loop.

- [Clozd Agentic Access](https://apis.io/agentic-access/clozd/clozd-agentic-access/)

#### Collections (7)

Postman and Open Collection request collections.

- [Clozd Data /programs /programs API](https://apis.io/collections/clozd/open-clozd-programs-api/)
- [Clozd Data /programs/:program Id/competitors /programs/:program Id/competitors API](https://apis.io/collections/clozd/open-clozd-programs-program-id-competitors-api/)
- [Clozd /programs/:program Id/deals /programs/:program Id/deals API](https://apis.io/collections/clozd/open-clozd-programs-program-id-deals-api/)
- [Clozd /programs/:program Id/deals/:deal ID /programs/:program Id/deals/:deal ID API](https://apis.io/collections/clozd/open-clozd-programs-program-id-deals-deal-id-api/)
- [Clozd Data /programs/:program Id/deals/import /programs/:program Id/deals/import API](https://apis.io/collections/clozd/open-clozd-programs-program-id-deals-import-api/)
- [Clozd Data /programs/:program Id/touchpoints /programs/:program Id/touchpoints API](https://apis.io/collections/clozd/open-clozd-programs-program-id-touchpoints-api/)
- [Clozd Data /programs/:program Id/touchpoints/:touchpoint ID /programs/:program Id/touchpoints/:touchpoint ID API](https://apis.io/collections/clozd/open-clozd-programs-program-id-touchpoints-touchpoint-id-api/)

#### Overlays (3)

OpenAPI overlays, including conversational phrasing for each operation.

- [Clozd Data Api V1 Overlay](https://apis.io/overlays/clozd/clozd-data-api-v1-overlay/)
- [Clozd Data Api V2 Overlay](https://apis.io/overlays/clozd/clozd-data-api-v2-overlay/)
- [Clozd Data Api V3 Overlay](https://apis.io/overlays/clozd/clozd-data-api-v3-overlay/)

#### Security (4)

Authentication, trust centre, and vulnerability disclosure.

- [Clozd Authentication](https://apis.io/security/clozd/clozd-authentication/)
- [Clozd Domain Security](https://apis.io/security/clozd/clozd-domain-security/)
- [Clozd Trust Center](https://apis.io/security/clozd/clozd-trust-center/)
- [Clozd Vulnerability Disclosure](https://apis.io/security/clozd/clozd-vulnerability-disclosure/)

#### OAuth scopes (1)

The permissions the API asks for.

- [Clozd Scopes](https://apis.io/scopes/clozd/clozd-scopes/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Clozd Plans Pricing](https://apis.io/plans/clozd/clozd-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Clozd Rate Limits](https://apis.io/rate-limits/clozd/clozd-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.clozd.com/)
- [Documentation](https://help.clozd.com/hc/en-us/articles/9948957669659-API-Imports-Exports)
- [Getting started](https://help.clozd.com/hc/en-us/articles/9948957669659-API-Imports-Exports)
- [API reference](https://app.clozd.com/public-api/docs/?urls.primaryName=v3.0)
- [Sign up](https://www.clozd.com/talk-with-us)
- [Support](https://help.clozd.com/hc/en-us)
- [Blog](https://www.clozd.com/blog)
- [Compliance](https://www.clozd.com/privacy/gdpr)
- [Trust centre](https://trust.clozd.com)
- [Terms of service](https://www.clozd.com/privacy/terms-of-use)
- [Privacy policy](https://www.clozd.com/privacy/policy)
- [GitHub](https://github.com/Clozd)

## API Evangelist rating

**Kin Score 52.6** — developing · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 73.3 |
| Contract quality | 54.2 |
| Contract governance | 18.2 |
| Operational transparency | 13.2 |
| Developer ergonomics | 58.9 |
| Access clarity | 50.0 |

**Agent readiness 60.4** — agent-native

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | derived (partial credit) |
| Reversibility Documented | — |
| MCP Server | verified |
| Auth Clarity | served (partial credit) |
| Idempotency | documented (partial credit) |
| Error Semantics | verified |
| OpenAPI Examples | verified |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | — |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | served (partial credit) |
| Protected Resource Metadata | verified |
| Dynamic Client Registration | yes |
| Agentic Commerce | — |
| Pagination Ratio | na |
| Operation Distinctiveness | distinct |

**Regulatory alignment 45.1** — Horizontal (data, software, accessibility, platform)

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/clozd/](https://apis.io/providers/clozd/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Clozd: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/clozd/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
