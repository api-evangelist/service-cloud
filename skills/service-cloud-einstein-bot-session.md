---
name: service-cloud-einstein-bot-session
description: Run a complete Einstein Bots conversation against Salesforce Service Cloud — open a session, exchange messages, handle escalation, and close it cleanly.
api: Einstein Bots Runtime API v5.3.0
spec: openapi/service-cloud-einstein-bots-openapi.yml
operations:
  - checkHealthStatus
  - getAPIVersions
  - startSession
  - continueSession
  - endSession
generated: '2026-08-27'
method: generated
source: openapi/service-cloud-einstein-bots-openapi.yml + conventions/service-cloud-conventions.yml + errors/service-cloud-problem-types.yml
---

# Run an Einstein Bots session

Every `operationId` below is grepped from `openapi/service-cloud-einstein-bots-openapi.yml`. There are only five; do not reach for anything else.

## 0. Pick a host and a version

Servers are regional, on `*.prod.chatbots.sfdc.sh`: `runtime-api-na-west`, `runtime-api-na-east`, `runtime-api-eu-west`, `runtime-api-eu-east`, `runtime-api-ap-west`, `runtime-api-ap-east`. Choose the one matching the org's region — there is no global host.

Before pinning `v5.3.0` into paths, call **`getAPIVersions`** (`GET /versions`). It answers anonymously and returns each version's status. As of 2026-08-27: `4.0.0` is `DEPRECATED`; `5.0.0`–`5.3.0` are `ACTIVE`. If the version you are about to use is not `ACTIVE`, stop and move up.

**`checkHealthStatus`** (`GET /status`) also answers anonymously and returns `{"status":"UP"|"DOWN"}`. Use it as a pre-flight, not as a liveness poll.

## 1. Authenticate

Mint a Salesforce OAuth access token with the **`chatbot_api`** scope, using the **JWT Bearer** flow (`jwtBearer` scheme) for server-to-server work. Token endpoint: `https://login.salesforce.com/services/oauth2/token` (sandbox: `https://test.salesforce.com/...`). Send it as `Authorization: Bearer <token>`.

The spec also declares `chatbotAuth` with authorization-code and implicit flows. Prefer authorization code with PKCE (`S256`) over implicit.

## 2. Start the session

**`startSession`** — `POST /v5.3.0/bots/{bot-id}/sessions`

Body is an `InitMessageEnvelope`:
- `externalSessionKey` — YOUR correlation id for this conversation. Generate a UUID and keep it; it is how you tie the bot session back to your own record.
- `forceConfig` — the org endpoint configuration.
- `message` — a `TextInitMessage` carrying the opening utterance.
- `variables` — optional context you pre-load into the bot (customer id, case number, entitlement).
- `responseOptions` — controls what comes back.

The response is a `ResponseEnvelope`: read `sessionId` off it and hold it. Also read `botVersion` — log it, because bot behaviour changes between versions and you will want it when a conversation goes wrong.

**This is not idempotent.** There is no `Idempotency-Key` on this API and `externalSessionKey` does not deduplicate. A retried `startSession` opens a second session. If the call times out, call `getAPIVersions` to confirm the service is reachable, then decide deliberately — do not blind-retry.

## 3. Exchange messages

**`continueSession`** — `POST /v5.3.0/sessions/{session-id}/messages`

Body is a `ChatMessageEnvelope`, which is a `oneOf` over:
- `TextMessage` — free text from the user
- `ChoiceMessage` — the user picked one of the bot's offered choices
- `RedirectMessage` — jump the dialog
- `SetVariablesMessage` — push context mid-conversation
- `TransferSucceededRequestMessage` / `TransferFailedRequestMessage` — tell the bot how your handoff to a human went

Two ordering fields matter and are easy to get wrong:
- `sequenceId` must increase monotonically within the session.
- `inReplyToMessageId` threads your message to the bot message it answers.

The response is a `ChatMessageResponseEnvelope`. Iterate its message list; each entry is one of:
- `TextResponseMessage` — say this to the user
- `ChoicesResponseMessage` — render these choices, then reply with a `ChoiceMessage`
- `EscalateResponseMessage` — **the bot is asking for a human.** Hand off, then report the outcome back with `TransferSucceededRequestMessage` or `TransferFailedRequestMessage`
- `SessionEndedResponseMessage` — the bot closed the session; do not call `endSession`

Also read `intents` (`NormalizedIntent` + `IntentSource`) and `entities` (`NormalizedEntity` + `EntitySource` + `EntityType`) — that is the bot telling you what it understood, and it is the cheapest signal you have for detecting a conversation going off the rails.

## 4. Close the session

**`endSession`** — `DELETE /v5.3.0/sessions/{session-id}`

Always close. An abandoned session holds org resources. Send an `EndSessionMessage` with an `EndSessionReason`.

**Not reversible.** There is no resume. Ending loses conversation state; the only path back is a fresh `startSession`, which starts the dialog over.

## 5. Handle errors

Every error body is the same shape and `requestId` is **required** on it — capture it, it is the trace handle for Salesforce support.

```
{ "status": int, "path": str, "requestId": uuid, "error": str, "message": str, "timestamp": int }
```

| Status | Name | Do |
|---|---|---|
| 400 | BadRequestError | Fix the body. Do not retry as-is. |
| 401 | UnauthorizedError | Re-mint the token, retry once. |
| 403 | ForbiddenError | Permission problem on the bot. Do not retry. |
| 404 | NotFoundError | Bad `bot-id`, or the session already ended. |
| 422 | RequestProcessingException | Semantically invalid. Read `message`. |
| 423 | NotAvailableError | Locked. Back off and retry. |
| 429 | TooManyRequestsError | Back off exponentially. There is no `Retry-After`. |
| 503 | ServiceUnavailable | A bot dialog's Apex or Flow timed out. Retry with backoff; if it persists the fault is in the org's automation. |

## Anti-patterns

- Do not poll `/status` in a loop as a heartbeat.
- Do not retry `startSession` on timeout without checking — you will strand a session.
- Do not skip `endSession` on the escalation path. Escalating and abandoning are different things.
- Do not hard-code `v5.3.0` without calling `getAPIVersions` first.
