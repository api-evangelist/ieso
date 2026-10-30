# IESO

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

The Independent Electricity System Operator (IESO) is the Crown corporation that operates Ontario's bulk electricity system and administers the province's wholesale electricity market, balancing supply and demand in real time, running the day-ahead and real-time energy and operating reserve markets, procuring capacity and transmission rights, planning the provincial grid, and delivering the Save on Energy conservation programs. It sits at the top of Canada's largest provincial electricity value chain — upstream of the licensed local distribution companies that bill retail customers — which is why its API posture splits cleanly in two. Its market and system data surface is genuinely open: a public report repository at reports-public.ieso.ca serves 139 report directories of demand, price, generation-by-fuel, intertie flow, outage, adequacy and settlement data as CSV, XML and XLSX over anonymous HTTPS with no account, key or registration, backed by published XSD schemas and a public interface specification. Its programmatic APIs are the opposite: the Axway SecureTransport REST API at reports.ieso.ca/api/v1.4, the SOAP Market Information Management web services, and the Appian-based Online IESO registration, retrofit, dispatch and outage web services are all reserved for registered Ontario market participants with IESO-issued machine accounts. IESO publishes no consumer energy-data API: Ontario's Green Button mandate (O. Reg. 633/21) binds licensed electricity and gas distributors, not the system operator, so there is no retail usage or billing surface here at all.

**Areas:** Energy · Canada · Electricity · Energy Markets · Grid · System Operator · Market Data · Open Data · Ontario · Demand Response · Renewables

## IESO on APIs.io

Everything profiled for IESO is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/ieso/](https://apis.io/providers/ieso/)** — or go straight to one of the APIs or properties below.

### APIs (9)

- [IESO Public Reports Repository](https://apis.io/apis/ieso/ieso-public-reports-repository/) — The IESO's open market and system data surface. A flat HTTPS file repository publishing 139 report directories — hourly Ontario and market demand, HOEP and…
- [IESO Reports Site REST API](https://apis.io/apis/ieso/ieso-reports-site-rest-api/) — A RESTful file-access API provided by Axway SecureTransport, used to automate retrieval of reports from the IESO Reports Site. Accepts and returns both JSON…
- [IESO Market Information Management (MIM) Web Services](https://apis.io/apis/ieso/ieso-mim-web-services/) — SOAP web services that let registered Ontario market participants submit and retrieve market information programmatically. The published WSDL (service name…
- [IESO Registration System Facilities API](https://apis.io/apis/ieso/ieso-registration-facilities-api/) — A REST API on the Appian-based Online IESO platform for retrieving and maintaining facility registration data. SPEC-249 documents the base path…
- [IESO Retrofit System API](https://apis.io/apis/ieso/ieso-retrofit-api/) — A web service API for the Retrofit program under Save on Energy, IESO's conservation and demand management portfolio, allowing participants to integrate…
- [IESO Retrofit Service Provider API](https://apis.io/apis/ieso/ieso-retrofit-service-provider-api/) — A companion web service API documented in SPEC-232 for service providers participating in the Save on Energy Retrofit program, covering the service-provider…
- [IESO Dispatch Service Web Service](https://apis.io/apis/ieso/ieso-dispatch-service-web-service/) — A web service, specified in SPEC-154, through which registered market participants retrieve dispatch instructions issued by the IESO and send back…
- [IESO Dispatch Notification Service Web Service](https://apis.io/apis/ieso/ieso-dispatch-notification-service/) — A push-notification web service, specified in SPEC-155, that delivers automated notifications of dispatch instructions to registered market participants…
- [IESO Outage Coordination and Scheduling System (OCSS) Web Service](https://apis.io/apis/ieso/ieso-ocss-web-service/) — A web service, specified in SPEC-113, for submitting and managing transmission and generation outage requests in IESO's Outage Coordination and Scheduling…

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### AsyncAPI (1)

Event-driven and webhook contracts.

- [Ieso Dispatch Notification Webhooks](https://apis.io/asyncapis/ieso/ieso-dispatch-notification-webhooks/)

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [ieso](https://apis.io/servers/ieso/ieso/)

#### Security (3)

Authentication, trust centre, and vulnerability disclosure.

- [Ieso Authentication](https://apis.io/security/ieso/ieso-authentication/)
- [Ieso Domain Security](https://apis.io/security/ieso/ieso-domain-security/)
- [Ieso Vulnerability Disclosure](https://apis.io/security/ieso/ieso-vulnerability-disclosure/)

#### OAuth scopes (1)

The permissions the API asks for.

- [Ieso Scopes](https://apis.io/scopes/ieso/ieso-scopes/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.ieso.ca/)
- [Documentation](https://www.ieso.ca/sector-participants/technical-interfaces)
- [API reference](https://www.ieso.ca/-/media/Files/IESO/technical-interfaces/xml-automated-docs/IMO_SPEC_0100.pdf)
- [Sign up](https://www.ieso.ca/Sector-Participants/Connection-Process/Overview)
- [Support](https://www.ieso.ca/Corporate-IESO/Contact)
- [Blog](https://www.ieso.ca/en/Corporate-IESO/Blog)
- [Security](https://www.ieso.ca/Sector-Participants/Cyber-Security/Cyber-Security-Incident-Reporting)
- [Terms of service](https://www.ieso.ca/Terms-of-Use)
- [Privacy policy](https://www.ieso.ca/privacy)

## API Evangelist rating

**Kin Score 46.8** — developing · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 62.5 |
| Contract quality | 45.3 |
| Contract governance | 4.5 |
| Operational transparency | 47.4 |
| Developer ergonomics | 58.9 |
| Access clarity | 34.2 |

**Agent readiness 35.3** — agent-ready

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | — |
| Reversibility Documented | — |
| MCP Server | — |
| Auth Clarity | served (partial credit) |
| Idempotency | — |
| Error Semantics | derived (partial credit) |
| OpenAPI Examples | documented (partial credit) |
| Rate Limit Signal | — |
| Event Surface Described | yes |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | served (partial credit) |
| Protected Resource Metadata | — |
| Dynamic Client Registration | yes |
| Agentic Commerce | — |
| Pagination Ratio | na |
| Operation Distinctiveness | na |

**Regulatory alignment 39.3** — Energy & Utilities

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/ieso/](https://apis.io/providers/ieso/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for IESO: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/ieso/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
