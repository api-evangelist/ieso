---
name: ieso-public-market-data
description: >-
  Retrieve Ontario electricity market and system data from the IESO public report repository —
  demand, prices, generation by fuel, intertie flows, outages and adequacy — anonymously, with the
  correct URL construction, latest-version link handling and polling cadence.
api: ieso:ieso-public-reports-repository
generated: '2026-07-27'
method: generated
source: >-
  IMO_SPEC_0100 (Outbound Automated Document API, Issue 8.0/9.0, 2025-03-05), the live directory
  listing of https://reports-public.ieso.ca/public/ enumerated 2026-07-27, and
  conventions/ieso-conventions.yml in this repo
auth: none
report_directories:
  - Demand
  - DemandZonal
  - GenOutputbyFuelHourly
  - GenOutputCapability
  - DispUnconsHOEP
  - RealtimeMktPrice
  - PredispMktPrice
  - DAHourlyOntarioZonalPrice
  - IntertieScheduleFlow
  - Adequacy3
  - TxOutagesTodayAll
  - VGForecastSummary
  - GlobalAdjustment
---

# Reading Ontario grid data from IESO

The IESO public report repository is the only IESO interface you can use without credentials. It is
not a REST API — there are no query parameters, no filtering, no pagination and no content
negotiation. It is a flat HTTPS file repository with a strict, documented naming convention. Get the
naming convention right and everything else follows.

## 1. Find the report directory

Every report has its own directory whose name IS the Document Identifier:

```
https://reports-public.ieso.ca/public/<Document Identifier>/
```

139 directories exist. List them by fetching `https://reports-public.ieso.ca/public/` and reading the
directory index. The `report_directories` list in this skill's frontmatter names the thirteen that
cover the common questions; `data-model/ieso-data-model.yml` in this repo groups all 139 into
families (Day-Ahead, Pre-Dispatch, Real-Time, Demand, Generation, Prices, Dispatch, Intertie,
Adequacy, Outages, Settlement, Transmission Rights, Auctions, Surveillance, Weekly).

## 2. Fetch the Global Link, not a computed filename

Files are named with five underscore-separated elements:

```
<Document Security>_<Document Identifier>_<Document Date/Time>_<Document Version>.<File Type>
PUB_Adequacy_20081107_v28.xml
```

**Do not compute that filename.** IESO publishes symbolic links for exactly this reason:

- **Global Link** — the filename with the date/time AND version removed. Always resolves to the
  latest version of the most recent reporting period. This is the URL you want.
  `https://reports-public.ieso.ca/public/Demand/PUB_Demand.csv`
- **Local Link** — the filename with only the version removed. Resolves to the latest version within
  one specific reporting period. Use this when you need a particular day.
  `https://reports-public.ieso.ca/public/Adequacy3/PUB_Adequacy3_20260727.xml`

## 3. Never infer meaning from the version number

IMO_SPEC_0100 paragraph 40, verbatim:

> Version numbers must not be relied upon as representing anything other than the next version for a
> specific report (e.g. v23 for a report produced each hour would normally represent hour 23 but
> because of a problem with issuing a previous report (i.e. a report is missed), v23 is for hour 24).

Read the hour from the document content, never from `_v<n>`.

## 4. Poll at the documented cadence

| Reporting period | Poll every |
|---|---|
| 5 minutes | 15 to 30 seconds |
| Hourly | 4 to 10 minutes |
| Daily | 30 to 60 minutes |

Two optimisations IESO documents:

- Parent report folders are listed together and carry a last-modified timestamp. Scan that list first
  and only traverse a folder whose timestamp moved.
- But a moved timestamp is not proof of new data — the parent folder last-modified also changes when
  a report is **moved or removed**, and purging runs roughly daily.

If you are going to poll heavily, IESO asks you to send your source IP addresses to
`customer.relations@ieso.ca` so their staff can troubleshoot.

## 5. Parse against the published schema

XML reports validate against a published, public XSD. Both live envelope generations are harvested in
this repo:

- `xsd/ieso-document-r1.xsd` — namespace `http://www.ieso.ca/schema`, root `Document`, sequence of
  `DocHeader` + `DocBody` pairs.
- `xsd/ieso-imo-document-r1.xsd` — legacy namespace `http://www.theIMO.com/schema`, root
  `IMODocument`, identical structure.

`DocHeader` carries `DocTitle`, `DocRevision` (integer, minimum 1), `DocConfidentiality` and
`CreatedAt` (`xs:dateTime`). `DocConfidentiality/DocConfClass` is the enum `PUB | CNF | HCNF | INT` —
anything you can fetch anonymously is `PUB` by definition.

Report-specific schemas specialise `DocBody`/`IMODocBody`. Eleven are harvested under `xsd/`. The
recurring payload type across the day-ahead and commitment reports is `HourlyValue`.

Many reports are also published as CSV and XLSX. The CSV carries a small header preamble (for
example `\\Hourly Demand Report`, `\\Created at 2026-07-27 07:30:10`) before the column row — skip
lines beginning with `\\`.

## 6. Expect no error envelope

You get bare HTTP status codes. `200` means the file exists. `404` means it does not.

One trap: `www.ieso.ca` answers unknown paths with HTTP **200** and an HTML page titled `Error-404`,
and `reports.ieso.ca` answers unknown paths with HTTP **200** and a SAML login page. A 200 from those
hosts is not proof the resource exists — check the body. The `reports-public.ieso.ca` host behaves
correctly and returns a real 404.

## 7. Know what is not here

- No consumer or retail energy data. Ontario's Green Button regulation (O. Reg. 633/21) binds
  licensed distributors, not the system operator. IESO holds no retail customer accounts.
- No REST API over this repository. `https://reports-public.ieso.ca/api/v1.4/files` returns 404 — the
  SecureTransport API serves the *confidential* repository only.
- No OpenAPI, AsyncAPI, GraphQL, MCP server or Postman collection.

## 8. Test against the sandbox

`https://reports-public-sandbox.ieso.ca/public/` is the anonymous sandbox twin with the same shape.
Use it to exercise your parser ahead of a Category 3 release — sandbox goes live roughly six weeks
before each production build (see `lifecycle/ieso-lifecycle.yml`).

## Terms

Use is governed by the IESO Terms of Use — <https://www.ieso.ca/Terms-of-Use>. There is no
click-through, no key and no registration, but the terms still apply.
