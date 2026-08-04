---
name: Import and read Clozd touchpoints (non-deal feedback)
description: >-
  Use the v3.0 touchpoint operations for customer-experience, implementation, retention and research
  programs — the non-deal analogue of the deal import/export flow.
api: openapi/clozd-data-api-v3-openapi.yml
operations:
  - get-programs-op
  - post-touchpoints-op
  - get-touchpoints-op
  - get-touchpoint-op
generated: '2026-08-04'
method: generated
source: openapi/clozd-data-api-v3-openapi.yml + conventions/clozd-conventions.yml
---

# Import and read Clozd touchpoints

A **touchpoint** is a customer moment that is not a won/lost sales opportunity — an implementation, a
renewal, a churn event, a CX or market-research contact. Touchpoints exist only in **v3.0**; v1 and v2 have
no equivalent. This is the surface behind Clozd's customer-experience, implementation-feedback, retention
and research programs.

## Steps

1. **Resolve the program.** `get-programs-op` — `GET /programs`. Check `clozd_program_type` — a touchpoint
   flow belongs to a non win-loss program type.

2. **Import touchpoints.** `post-touchpoints-op` —
   `POST /programs/{program_id}/touchpoints`. Body objects follow the `Touchpoint` schema:
   `clozd_external_id` (your id — the upsert key), `clozd_touchpoint_name`, `clozd_organization_name`,
   `clozd_organization_domain`, `clozd_created_date`, `clozd_owner_name`, `clozd_owner_email`, plus optional
   `clozd_industry`, `clozd_region`, `clozd_headcount`, `clozd_revenue`, `clozd_currency`. Participants
   nest inside the touchpoint object.

3. **Page touchpoints.** `get-touchpoints-op` —
   `GET /programs/{program_id}/touchpoints?include=feedback&include=surveyQuestions&limit=1000&offset=0`.
   Legal collection includes: `customFields`, `feedback`, `tags`, `surveyQuestions` (max 5).

4. **Fetch one with transcripts.** `get-touchpoint-op` —
   `GET /programs/{program_id}/touchpoints/{touchpoint_id}?include=feedback&include=transcripts&include=participants`.
   Returns `TouchpointFull` with `TouchpointResponseWithTranscript`.

## Rules that matter

- Same natural-key idempotency as deals: re-importing the same `clozd_external_id` **updates**; a bad
  attribute rolls the whole request back.
- `get-touchpoint-op` is the **only** operation in the whole v3 spec that declares a `404` (`AUTH007`) —
  handle a missing touchpoint id explicitly here.
- `participants` and `transcripts` are single-record includes only, exactly as with deals.
- No MCP tool addresses touchpoints. Everything touchpoint-shaped has to go through REST — an agent
  connected only over MCP cannot see this data.
- Incremental sync uses `filter[feedback_updated_since]` and `filter[feedback_published_since]`.
