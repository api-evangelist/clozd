---
name: Build a competitive win-loss picture from the Clozd API
description: >-
  Assemble a per-competitor view — how often you meet them, how you fare, and what buyers said — from the
  v3.0 competitors and deals operations.
api: openapi/clozd-data-api-v3-openapi.yml
operations:
  - get-programs-op
  - get-competitors-op
  - get-deal-op
generated: '2026-08-04'
method: generated
source: openapi/clozd-data-api-v3-openapi.yml + conventions/clozd-conventions.yml
---

# Build a competitive win-loss picture

The REST API gives you the raw material; it does **not** compute win rates or sentiment. If you want the
computed answer, the MCP tools `get_competitors` and `get_competitor_sentiment_drivers` already do it
server-side — see `skills/clozd-query-insights-over-mcp.md`.

## Steps

1. **Resolve the program.** `get-programs-op` — `GET /programs`.

2. **List competitors with their deals.** `get-competitors-op` —
   `GET /programs/{program_id}/competitors?include=deals&include=responses&limit=1000&offset=0`.
   - `include` on this operation accepts only `deals` and `responses`, max 2.
   - **Hard constraint from the spec:** you cannot include `responses` without also including `deals`.
     Sending `include=responses` alone returns `400 API009`.
   - The result is `ListOfCompetitorsWithDeals` — `clozd_competitor_name`, `clozd_competitor_id`,
     `clozd_competitor_domain`, `clozd_update_date`, plus the nested deals.

3. **Compute the tallies yourself.** For each competitor, group the nested deals by `clozd_outcome` /
   `clozd_outcome_type` to get encounters, wins and losses. Segment by `clozd_industry`,
   `clozd_region`, `clozd_headcount`, `clozd_revenue` or `clozd_products` as needed.

4. **Attribute the reasons.** Responses carry `clozd_primary_competitor` and `clozd_drivers`. Group drivers
   by competitor to see what buyers cited. For verbatim evidence, call `get-deal-op` on the specific deal
   with `include=feedback&include=transcripts`.

## Rules that matter

- `clozd_primary_competitor` is the *primary* competitor on a response — a deal can involve more than one
  competitor, so competitor-level driver counts derived this way undercount multi-vendor evaluations.
- Drivers are AI-derived labels on the response, not a normalized taxonomy in REST. The driver *category*
  taxonomy (Product, Pricing, Support …) exists only on the MCP surface.
- Only published feedback is visible, so early-stage or unpublished interviews are absent from every count.
- Competitor records are program-scoped. A competitor seen in two programs appears twice with different
  `clozd_competitor_id` values — reconcile on `clozd_competitor_domain`, not on name.
- Use `filter[competitor_updated_since]` for repeat runs instead of a full re-walk.
