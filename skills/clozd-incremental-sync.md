---
name: Incrementally sync Clozd feedback without re-reading everything
description: >-
  Keep a downstream store in sync with a Clozd program using the filter[*_updated_since] parameters, which
  are Clozd's only change-detection mechanism — there are no webhooks and no event stream.
api: openapi/clozd-data-api-v3-openapi.yml
operations:
  - get-programs-op
  - get-deals-op
  - get-touchpoints-op
  - get-competitors-op
generated: '2026-08-04'
method: generated
source: openapi/clozd-data-api-v3-openapi.yml + conventions/clozd-conventions.yml
---

# Incrementally sync Clozd feedback

Clozd publishes **no webhooks, no AsyncAPI and no event stream**. The documented way to stay current is to
poll with an `updated_since` filter. Clozd states this explicitly: specifying the filter "will enable
periodic query of incremental changes since the last pull (instead of always having to pull all
competitors)".

## Steps

1. **Store a high-water mark per collection.** Keep the last successful sync timestamp separately for
   deals, touchpoints and competitors. Use ISO 8601 date-time; the parameter is capped at 25 characters.

2. **Poll deals.** `get-deals-op` —
   `GET /programs/{program_id}/deals?filter[feedback_updated_since]=2026-08-01T00:00:00Z&include=feedback&limit=1000&offset=0`
   - `filter` is a `deepObject`, exploded — send `filter[feedback_updated_since]=...`.
   - Deals also accept `filter[feedback_published_since]` (first publication, not edits) and
     `filter[clozd_insight_gems]`.
   - Follow `links.next` to the end of the page set before advancing the high-water mark.

3. **Poll touchpoints.** `get-touchpoints-op` with the same
   `filter[feedback_updated_since]` / `filter[feedback_published_since]` pair.

4. **Poll competitors.** `get-competitors-op` with
   `filter[competitor_updated_since]` — note the different field name.

5. **Advance the watermark only on a clean full walk.** Set it to the timestamp you *started* the poll, not
   the max `clozd_update_date` you saw, so records written mid-walk are not skipped. Overlap the window by
   a few minutes and rely on upsert-by-id downstream.

## Rules that matter

- `feedback_updated_since` tracks **feedback** changes, not deal-field changes. A CRM-side edit that Clozd
  re-imports may not move it. Re-baseline periodically (a full walk on a slow cadence) if downstream deal
  attributes must be exact.
- Reconcile on `clozd_deal_id` / `clozd_touchpoint_id` (Clozd UUIDs) downstream, and keep
  `clozd_external_id` as the CRM join key.
- There are no rate limits published and no `429` in the contract — pick a conservative poll interval
  (hourly or slower) rather than discovering a limit empirically.
- Offset caps at 100000. If a window returns more than that, narrow the window rather than paging past it.
- Errors: `400 API009` usually means a malformed `filter[...]` value — check the ISO 8601 formatting and
  the 25-character cap.
