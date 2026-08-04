---
name: Export published win-loss feedback and transcripts
description: >-
  Pull published win-loss feedback out of a Clozd program into a BI tool, warehouse or reporting pipeline —
  deals, AI summaries, decision drivers, survey questions and full interview transcripts.
api: openapi/clozd-data-api-v3-openapi.yml
operations:
  - get-programs-op
  - get-deals-op
  - get-deal-op
generated: '2026-08-04'
method: generated
source: openapi/clozd-data-api-v3-openapi.yml + conventions/clozd-conventions.yml
---

# Export published win-loss feedback and transcripts

Use this to move Clozd feedback into a warehouse, BI tool, or an analysis you are running yourself. If you
just need answers in natural language, use the MCP server instead (see
`skills/clozd-query-insights-over-mcp.md`) — it is cheaper and pre-aggregated.

## Steps

1. **Resolve the program.** `get-programs-op` — `GET /programs`. Keep `clozd_program_id`.

2. **Page the deals with feedback attached.** `get-deals-op` —
   `GET /programs/{program_id}/deals?include=feedback&include=surveyQuestions&limit=1000&offset=0`.
   - `include` is an exploded array — repeat the parameter, do not comma-join.
   - On the **collection** operation the legal `include` values are `customFields`, `products`, `feedback`,
     `tags`, `surveyQuestions` (max 5). `participants` and `transcripts` are **not** available here.
   - The response is a `PagedListResponse`: `{success, message, links{self,prev,next,first,last}, count,
     total, data[]}`. Follow `links.next` verbatim — it is an absolute URL — until it is null.
   - `limit` maxes at 1000 and `offset` at 100000, so a single program walk tops out at 100k deals. If
     `total` exceeds that, partition the walk with `filter[feedback_updated_since]` windows.

3. **Fetch transcripts per deal.** For each deal that has published feedback, call `get-deal-op` —
   `GET /programs/{program_id}/deals/{deal_id}?include=feedback&include=transcripts&include=participants`.
   This returns `DealFull` with `ResponseWithTranscript` objects. Transcripts are **only** available on the
   single-deal operation, so this is one request per deal — do it selectively, not for the whole program.

4. **Map the shape.** Per response: `clozd_response_id`, `clozd_response_participant`, `clozd_channel`,
   `clozd_decision`, `clozd_primary_competitor`, `clozd_publish_date`, `clozd_summary` (the AI summary),
   `clozd_drivers` (the decision drivers), `clozd_update_date`, `clozd_survey_questions[]`
   (`clozd_question` / `clozd_answer`).

## Rules that matter

- Only **published** feedback is returned. Unpublished responses are invisible to the API.
- `clozd_summary` and `clozd_drivers` are AI-generated; treat them as derived analysis, not verbatim buyer
  words. Use transcripts when you need the actual quote.
- Feedback content is buyer interview data. Respect the customer's DPA — Clozd states it does not process
  sensitive personal data as defined by GDPR, and prohibits clients from sending it.
- Error handling: `401 API003/AUTH005` (token), `403 AUTH006` (program not entitled), `400 API009`
  (malformed include/filter). Note `get-deal-op` declares **no 404**, so a deleted deal id has no documented
  response shape — treat any non-200 defensively.
- Never re-walk the whole program on a schedule. Use the incremental sync skill.
