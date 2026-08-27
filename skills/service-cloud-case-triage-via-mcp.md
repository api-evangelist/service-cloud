---
name: service-cloud-case-triage-via-mcp
description: Read, triage and update Salesforce Service Cloud cases through the Salesforce hosted MCP servers, with the least-privilege server choice and the delete/undo rules that actually apply.
api: Salesforce Hosted MCP Servers (SObject family)
endpoint: https://api.salesforce.com/platform/mcp/v1/platform/sobject-all
tools:
  - getObjectSchema
  - soqlQuery
  - find
  - getUserInfo
  - listRecentSobjectRecords
  - getRelatedRecords
  - createSobjectRecord
  - updateSobjectRecord
  - updateRelatedRecord
  - deleteSobjectRecord
  - deleteRelatedRecord
generated: '2026-08-27'
method: generated
source: mcp/service-cloud-mcp.yml + mcp/service-cloud-tool-crosswalk.yml + conventions/service-cloud-conventions.yml
---

# Triage Service Cloud cases over MCP

Tool names below are quoted from Salesforce's own hosted-MCP reference pages. Nothing here is invented; where a schema is unknown it says so, because `tools/list` is auth-gated (`401 JWT Token is required`).

## 0. Pick the right server first — this is the whole safety story

Salesforce ships four SObject servers so you can withhold capability rather than trust an agent to restrain itself:

| Server | Endpoint | Use when |
|---|---|---|
| SObject Reads | `.../platform/mcp/v1/platform/sobject-reads` | Triage, reporting, answering questions. **Default to this.** |
| SObject Mutations | `.../platform/mcp/v1/platform/sobject-mutations` | Creating and updating cases. |
| SObject Deletes | `.../platform/mcp/v1/platform/sobject-deletes` | Deliberate cleanup only. |
| SObject All | `https://api.salesforce.com/platform/mcp/v1/platform/sobject-all` | Everything, including deletes. |

Sandbox rehearsal uses the `/sandbox/` path: `https://api.salesforce.com/platform/mcp/v1/sandbox/platform/sobject-all`. Run any destructive workflow there first.

Auth is OAuth 2.0 authorization code with PKCE (`S256`) against an admin-created External Client App, requesting the **`mcp_api`** scope. The protected-resource metadata is at `https://api.salesforce.com/.well-known/oauth-protected-resource/platform/mcp/v1/platform/sobject-all`.

## 1. Orient before you query

- **`getUserInfo`** — who am I, what profile and timezone. Do this first; every SOQL result and every SLA calculation depends on it.
- **`getObjectSchema`** on `Case` — in index mode first, then detail mode for the fields you actually need. Service Cloud orgs routinely carry hundreds of custom fields on `Case`; do not `SELECT` blind.

## 2. Find the work

- **`soqlQuery`** for a known filter:
  `SELECT Id, CaseNumber, Subject, Status, Priority, OwnerId, CreatedDate FROM Case WHERE IsClosed = false AND Priority = 'High' ORDER BY CreatedDate ASC`
- **`find`** for text across objects when you do not know which object holds the answer.
- **`listRecentSobjectRecords`** to pick up where a human left off.
- **`getRelatedRecords`** from a `Case` to reach `CaseComment`, `CaseHistory` and `EmailMessage` — the conversation history that tells you what has already been tried.

Pagination is a cursor: read `done`, and if it is `false`, follow `nextRecordsUrl` verbatim. Never build the next page URL yourself.

## 3. Act

- **`createSobjectRecord`** to open a `Case`. **Decide about assignment rules before you call.** The REST layer's `Sforce-Auto-Assign` header controls whether active assignment rules fire; with them on, your create routes the case to a queue and can notify real people.
- **`updateSobjectRecord`** to change `Status`, `Priority` or `OwnerId`.
- **`updateRelatedRecord`** to touch a child through the parent relationship.

**There is no idempotency key on this surface.** `Idempotency-Key` works on Salesforce UI API `/ui-api/records` (UUID v4, 30-day cache, 9 MB response cap), not on the sObject layer the MCP tools call. If you need a safely repeatable create, use an external ID upsert instead — `PATCH /services/data/v67.0/sobjects/Case/{ExternalIdField}/{value}` — which cannot duplicate on the same key.

## 4. Deleting — read this before you call it

- **`deleteSobjectRecord`** / **`deleteRelatedRecord`** are permanent from your seat.
- Salesforce documents the window verbatim: *"Deleted records go to the Recycle Bin and can be recovered in the Salesforce UI for up to 15 days — no undelete tool is available through MCP."*
- So: recoverable by a **human**, within **15 days**, in the **UI**. Not by you, not through MCP, not through REST.
- Treat every delete as irreversible. Confirm with the user, name the exact records, and prefer the reads or mutations server so the tool is not even present.

## 5. Watch the limits

The org has a rolling 24-hour API request allocation, not a per-second rate. There is no `RateLimit-*` or `Retry-After` header — the signal is `Sforce-Limit-Info`, returned on every REST response. Exhaustion surfaces as `REQUEST_LIMIT_EXCEEDED` (403), and it is a hard stop for the rest of the window, not a throttle you can back off through. A long SOQL over a large `Case` table can also trip the separate concurrent-long-running-request ceiling (5 in Developer Edition, 25 in production).

Platform REST errors arrive as an **array**: `[{"message":..., "errorCode":..., "fields":[...]}]`. Read `fields` — it names exactly what failed validation.
