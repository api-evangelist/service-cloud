# Salesforce Service Cloud APIs

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

Salesforce Service Cloud is the customer-service and contact-centre product on the Salesforce Platform, and its API surface is the Salesforce Platform API applied to service objects — Case, CaseComment, Knowledge articles, LiveChatTranscript, MessagingSession, entitlements and omni-channel routing — reached through the REST, SOAP, Bulk and Composite APIs. Alongside that record layer Salesforce ships purpose-built runtimes with their own contracts: an OpenAPI 3.0 document for the Einstein Bots Runtime API, a gRPC/protobuf contract for the Pub/Sub event API, a CometD Streaming API, and generally available hosted MCP servers that expose the sObject layer to AI agents over OAuth. There is no single global API host — every call is made against the customer's own org at the instance_url returned with the OAuth token, on API version 67.0 (Summer '26). Authentication is OAuth 2.0 only; Salesforce issues no API keys.

**Areas:** Cloud · CRM · Customer Service · Enterprise · Salesforce · Support

## Salesforce Service Cloud APIs on APIs.io

Everything profiled for Salesforce Service Cloud APIs is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/service-cloud/](https://apis.io/providers/service-cloud/)** — or go straight to one of the APIs or properties below.

### APIs (19)

- [Service Cloud REST API](https://apis.io/apis/service-cloud/service-cloud-rest-api/) — Core REST API for Service Cloud operations: sObject CRUD over Case, CaseComment, EmailMessage, Knowledge articles and every other standard and custom…
- [Service Cloud Case Management API](https://apis.io/apis/service-cloud/service-cloud-case-management-api/) — The Case object and its children — CaseComment, CaseHistory, EmailMessage — worked through the sObjects REST resources. Assignment-rule behaviour on create…
- [Service Cloud Knowledge API](https://apis.io/apis/service-cloud/service-cloud-knowledge-api/) — Knowledge article management — article versions, categories, publishing states and the knowledgeManagement REST resources used to surface self-service content.
- [Service Cloud Omni-Channel API](https://apis.io/apis/service-cloud/service-cloud-omni-channel-api/) — Omni-channel presence, work assignment and routing, exposed through the connect/omnichannel REST resources and the ServiceChannel / PendingServiceRouting…
- [Service Cloud Live Agent API](https://apis.io/apis/service-cloud/service-cloud-live-agent-api/) — Real-time chat with customers over the Live Agent REST surface. Superseded for new builds by Messaging for In-App and Web, which authenticates with the…
- [Service Cloud Pub/Sub API](https://apis.io/apis/service-cloud/service-cloud-pubsub-api/) — The gRPC event API. Publish and subscribe to platform events and Change Data Capture events (including /data/CaseChangeEvent) with Avro-encoded payloads and…
- [Service Cloud Streaming API](https://apis.io/apis/service-cloud/service-cloud-streaming-api/) — CometD/Bayeux long-polling surface for PushTopic, generic streaming and platform event subscriptions. The legacy transport; new integrations should use the…
- [Service Cloud CTI API](https://apis.io/apis/service-cloud/service-cloud-cti-api/) — Open CTI — the JavaScript API that connects a telephony system's softphone to the Service Cloud console, plus the Voice/contact-centre surfaces built on it.
- [Salesforce Hosted MCP Servers](https://apis.io/apis/service-cloud/salesforce-hosted-mcp-servers/) — Generally available remote Model Context Protocol servers that expose the Salesforce sObject layer to AI agents over OAuth. Four least-privilege SObject…
- [Salesforce Service Cloud APIs Bot API](https://apis.io/apis/service-cloud/service-cloud-bot-api/) — The bot API from Salesforce Service Cloud APIs — 3 operation(s) for bot.
- [Salesforce Service Cloud APIs Health API](https://apis.io/apis/service-cloud/service-cloud-health-api/) — The health API from Salesforce Service Cloud APIs — 1 operation(s) for health.
- [Salesforce Service Cloud APIs Versions API](https://apis.io/apis/service-cloud/service-cloud-versions-api/) — The versions API from Salesforce Service Cloud APIs — 1 operation(s) for versions.
- [Salesforce Service Cloud SOAP API](https://apis.io/apis/service-cloud/salesforce-service-cloud-soap-api/) — SOAP-based API for enterprise integration and complex service cloud operations.
- [Knowledge API](https://apis.io/apis/service-cloud/knowledge-api/) — API for managing knowledge base articles and content.
- [Einstein Bot API](https://apis.io/apis/service-cloud/einstein-bot-api/) — API for building and managing AI-powered chatbots for customer service.
- [Omni-Channel API](https://apis.io/apis/service-cloud/omni-channel-api/) — API for managing omni-channel routing, agent presence, and work distribution across multiple service channels including chat, messaging, email, and voice.
- [Service Cloud Voice Telephony Integration API](https://apis.io/apis/service-cloud/service-cloud-voice-telephony-integration-api/) — REST API for integrating telephony systems with Service Cloud Voice, enabling programmatic management of voice calls, call recording, and real-time…
- [Service Cloud Voice for Partner Telephony API](https://apis.io/apis/service-cloud/service-cloud-voice-for-partner-telephony-api/) — Developer API for connecting third-party telephony systems to Service Cloud Voice, including the Connector API for passing information between contact…
- [Agentforce Service Agent API](https://apis.io/apis/service-cloud/agentforce-service-agent-api/) — REST API for building and deploying AI-powered service agents using Agentforce, enabling headless agent interactions, session management, and seamless…

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### AsyncAPI (1)

Event-driven and webhook contracts.

- [Service Cloud Event Surface](https://apis.io/asyncapis/service-cloud/service-cloud-event-surface/)

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [Salesforce Hosted MCP Servers + Salesforce DX MCP Server](https://apis.io/servers/service-cloud/salesforce-hosted-mcp-servers-salesforce-dx-mcp-server/)

#### Overlays (4)

OpenAPI overlays, including conversational phrasing for each operation.

- [Service Cloud Bot Api Phrasing Overlay](https://apis.io/overlays/service-cloud/service-cloud-bot-api-phrasing-overlay/)
- [Service Cloud Einstein Bots Overlay](https://apis.io/overlays/service-cloud/service-cloud-einstein-bots-overlay/)
- [Service Cloud Health Api Phrasing Overlay](https://apis.io/overlays/service-cloud/service-cloud-health-api-phrasing-overlay/)
- [Service Cloud Versions Api Phrasing Overlay](https://apis.io/overlays/service-cloud/service-cloud-versions-api-phrasing-overlay/)

#### Security (4)

Authentication, trust centre, and vulnerability disclosure.

- [Service Cloud Authentication](https://apis.io/security/service-cloud/service-cloud-authentication/)
- [Service Cloud Domain Security](https://apis.io/security/service-cloud/service-cloud-domain-security/)
- [Service Cloud Trust Center](https://apis.io/security/service-cloud/service-cloud-trust-center/)
- [Service Cloud Vulnerability Disclosure](https://apis.io/security/service-cloud/service-cloud-vulnerability-disclosure/)

#### OAuth scopes (1)

The permissions the API asks for.

- [Service Cloud Scopes](https://apis.io/scopes/service-cloud/service-cloud-scopes/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Service Cloud Plans Pricing](https://apis.io/plans/service-cloud/service-cloud-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Service Cloud Rate Limits](https://apis.io/rate-limits/service-cloud/service-cloud-rate-limits/)

#### FinOps (1)

What the API costs to run, by operation.

- [Service Cloud Finops](https://apis.io/finops/service-cloud/service-cloud-finops/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.salesforce.com/service/)
- [Documentation](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/)
- [Getting started](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/intro_what_is_rest_api.htm)
- [API reference](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/resources_list.htm)
- [Pricing](https://www.salesforce.com/service/pricing/)
- [Sign up](https://www.salesforce.com/products/free-trial/developer/)
- [Support](https://help.salesforce.com/)
- [Blog](https://www.salesforce.com/blog/)
- [Security](https://www.salesforce.com/company/disclosure/)
- [Compliance](https://compliance.salesforce.com/)
- [Terms of service](https://www.salesforce.com/company/legal/agreements/)
- [Privacy policy](https://www.salesforce.com/company/privacy/)
- [GitHub](https://github.com/forcedotcom)
- [Status](https://status.salesforce.com/)

## API Evangelist rating

**Kin Score 70.4** — exemplar · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 58.3 |
| Contract quality | 50.8 |
| Contract governance | 18.2 |
| Operational transparency | 84.2 |
| Developer ergonomics | 82.7 |
| Access clarity | 92.1 |

**Agent readiness 46.2** — agent-ready (gated down from agent-native)

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | — |
| Reversibility Documented | — |
| MCP Server | documented (partial credit) |
| Auth Clarity | served (partial credit) |
| Idempotency | documented (partial credit) |
| Error Semantics | derived (partial credit) |
| OpenAPI Examples | partial |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | yes |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | served (partial credit) |
| Protected Resource Metadata | verified |
| Dynamic Client Registration | — |
| Agentic Commerce | — |
| Pagination Ratio | na |
| Operation Distinctiveness | distinct |

**Regulatory alignment 45.1** — Horizontal (data, software, accessibility, platform)

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/service-cloud/](https://apis.io/providers/service-cloud/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Salesforce Service Cloud APIs: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/service-cloud/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
