---
name: Query Clozd win-loss insights over the MCP server
description: >-
  Connect an AI client to the hosted Clozd MCP server and use its 19 read tools to answer win-loss,
  decision-driver and competitive questions without touching the REST API.
api: mcp/clozd-mcp.yml
operations:
  - get_programs
  - get_win_rates
  - get_decision_drivers
  - get_driver_quotes
  - get_competitor_sentiment_drivers
generated: '2026-08-04'
method: generated
source: >-
  mcp/clozd-mcp.yml + https://help.clozd.com/hc/en-us/articles/49656607624987-Connecting-to-Clozd-via-MCP
---

# Query Clozd win-loss insights over MCP

The MCP server is the right surface for *questions*. The REST Data API is the right surface for *bulk
movement of records*. They are not the same projection of the data — see
`mcp/clozd-tool-crosswalk.yml`.

## Connect

- Server URL: `https://mcp.clozd.com/mcp` (streamable HTTP). That is the only value a client needs.
- Auth is OAuth 2.0 authorization code + PKCE (`S256`) against `https://oauth.clozd.com`, which brokers to
  the organization's own identity provider (Okta, Google, Microsoft/Entra, SAML) — or email + password if
  the org has no SSO. **No API keys go into any config file.**
- The org must have MCP access enabled; otherwise ask a Clozd account admin or support@clozd.com.
- Supported clients: Claude (web/Desktop, custom connector), ChatGPT Apps, Cursor, Microsoft Copilot Studio,
  Windsurf, Antigravity, Gemini CLI, and any MCP client that accepts a URL.
- Generic JSON config:
  `{"mcpServers":{"clozd":{"type":"http","url":"https://mcp.clozd.com/mcp"}}}`
  (Antigravity requires the key `serverUrl` instead of `url`).

## Steps

1. **Always call `get_programs` first.** Every other tool needs a program id, and the tool description says
   so explicitly.
2. **Ask the shaped question, not the raw one.**
   - "Why do we win/lose?" → `get_decision_drivers` (ranked with sentiment), or
     `get_decision_driver_categories` / `get_decision_driver_category_counts` for the thematic roll-up.
   - "How are we performing?" → `get_win_rates`, optionally broken down by industry, product, sales rep or
     deal size.
   - "Against whom?" → `get_competitors`, then `get_competitor_sentiment_drivers` filtered to positive or
     negative sentiment.
   - "Show me proof" → `get_driver_quotes` for verbatim buyer quotes tied to a driver.
   - "What should we act on?" → `get_awe_deals` (at-risk / win-back / expansion).
3. **Prefer summaries over transcripts.** `get_response_summaries` is explicitly documented as faster than
   `get_transcripts`; only pull transcripts when the full conversation is genuinely needed.
4. **Cross-check interview data against calls.** The `get_gong_*` family reads drivers detected in Gong call
   recordings; `get_gong_driver_overlap` reconciles them against Clozd interview drivers and is the tool to
   reach for when someone asks "does what buyers told us match what the reps heard?"

## Rules that matter

- **Everything is read-only.** There is no import, update or delete tool. Writes must go through
  `post-deals-op` / `post-touchpoints-op` on the REST API.
- **Touchpoints are invisible over MCP.** CX, implementation and retention feedback stored as touchpoints
  has no tool. Use REST.
- **Scope is coarse.** The OAuth `api` scope covers the whole server; what a token can actually see is
  decided by the user's platform role and the programs an admin scoped to them — not by the scope string.
  Do not assume a token is narrower than the user's own access.
- **Anonymous introspection is blocked.** `tools/list` without a bearer token returns `401` with
  `errorCode: MCP001` and a `WWW-Authenticate` header pointing at
  `https://mcp.clozd.com/.well-known/oauth-protected-resource/mcp`. Input schemas can only be read after
  authenticating.
- Gong tools return nothing unless the customer has the Gong integration enabled.
- The data is buyer interview content about named accounts and named competitors. Treat outputs as
  confidential customer research, not as publishable material.
