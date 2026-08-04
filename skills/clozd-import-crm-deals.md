---
name: Import CRM deals and participants into Clozd
description: >-
  Push won/lost opportunities and their buyer-side participants from a CRM (HubSpot, Microsoft Dynamics, or
  any non-Salesforce system) into a Clozd win-loss program, idempotently, using the v3.0 Data API.
api: openapi/clozd-data-api-v3-openapi.yml
operations:
  - get-programs-op
  - post-deals-op
generated: '2026-08-04'
method: generated
source: openapi/clozd-data-api-v3-openapi.yml + conventions/clozd-conventions.yml
---

# Import CRM deals and participants into Clozd

Use this when a customer's CRM is not Salesforce (Clozd's Salesforce integration handles that natively) and
deal data must be pushed to Clozd programmatically.

## Before you start

- API access is **not self-serve**. The organization's *API Imports* setting must be enabled by a Clozd
  Program Manager or by emailing support@clozd.com. Until then this flow returns `403` / `AUTH006`.
- Get an organization access token from the Clozd app: initials menu → **Settings** → **API Token** →
  **Create Access Token**. The token is shown **once**.
- Send it on every request as the header `x-api-token: <token>`.
- Any custom fields you intend to send must already exist in the Clozd app, and the key name must match the
  Clozd field name **exactly**.

## Steps

1. **Resolve the program.** Call `get-programs-op` — `GET https://app.clozd.com/public-api/v3/programs`.
   Read `data[].clozd_program_id` for the win-loss program you are loading. Every other call is scoped by
   this id. Page with `limit` (max 1000) and `offset` if the org has many programs.

2. **Shape each deal.** Build objects against the `Deal` schema. Set:
   - `clozd_external_id` — **your** CRM opportunity id. This is the upsert key; get it right.
   - `clozd_deal_name`, `clozd_outcome`, `clozd_outcome_type`, `clozd_amount`, `clozd_currency`,
     `clozd_closed_date`, `clozd_organization_name`, `clozd_organization_domain`
   - optional segmentation: `clozd_industry`, `clozd_region`, `clozd_headcount`, `clozd_revenue`,
     `clozd_lead_source`, `clozd_sales_rep_name`, `clozd_sales_rep_email`, `clozd_products`
   - participants are nested **inside** the deal object, not sent separately.

3. **Import.** Call `post-deals-op` —
   `POST https://app.clozd.com/public-api/v3/programs/{program_id}/deals` with the deal array as the body.
   One or many deals, zero or many participants per deal.

4. **Read the result.** A `200` returns
   `{"success":true,"message":"Successfully imported deal data.","data":{"result":"Created Deals: N Updated Deals: M"}}`.
   Parse `data.result` to confirm the created/updated split matches what you expected.

## Rules that matter

- **Idempotency is by natural key.** Re-importing a deal whose `clozd_external_id` already exists in the
  program **updates** it — it does not duplicate. Safe to replay. There is no `Idempotency-Key` header.
- **The import is atomic per request.** "If an attribute is required or a wrong value type is provided the
  POST request will be rolled back and rejected." A single bad row kills the whole batch, so validate types
  client-side and prefer moderate batch sizes so one bad record does not block a large load.
- **Errors are not RFC 9457.** Branch on `errorCode`, not on `message`:
  - `API003` / `AUTH005` (401) — token missing or rejected. Re-issue the token.
  - `AUTH006` (403) — API imports not enabled, or the program does not belong to this token's organization.
  - `API009` (400) — malformed payload or wrong value type. Fix and resubmit the whole batch.
- **No rate limits are published.** Back off on your own schedule; there is no `429` in the contract and no
  rate-limit response header to read.
- Older versions (`/public-api/v1`, `/public-api/v2`) are still live and also accept imports. Use **v3**.
