---
name: ieso-mim-bid-submission
description: >-
  Submit, query and cancel bids and offers in Ontario's wholesale electricity market through the IESO
  Market Information Management (MIM) SOAP web services — correct operation selection, credential and
  network prerequisites, and the two-path submission result model that must be checked on success as
  well as on fault.
api: ieso:ieso-mim-web-services
generated: '2026-07-27'
method: generated
source: >-
  wsdl/ieso-mim-web-services.wsdl and xsd/ieso-mim-web-services.xsd in this repo (harvested verbatim
  from ieso.ca), plus MWT-User-Guide.pdf
auth: UserID/password in the form user@participantName, plus source-IP allow-listing and a client truststore
operations:
  - RealtimeEnergyBidUploadOperation
  - RealtimeEnergyBidQueryOperation
  - RealtimeEnergyBidCancelOperation
  - OperatingReserveBidUploadOperation
  - OperatingReserveBidQueryOperation
  - OperatingReserveBidCancelOperation
  - ScheduleBidUploadOperation
  - ScheduleBidQueryOperation
  - ScheduleBidCancelOperation
  - BilateralBidUploadOperation
  - BilateralBidQueryOperation
  - BilateralBidCancelOperation
  - DailyDispatchBidUploadOperation
  - DailyDispatchBidQueryOperation
  - ForebayBidUploadOperation
  - ForebayBidQueryOperation
  - ForebayOperation
  - MarketStatusOperation
  - MarketMessageOperation
  - ResourceOperation
  - ActAsMarketParticipantOperation
  - HealthCheckOperation
---

# Submitting to the IESO market via MIM web services

> **This is a safety-critical, participant-only interface.** These operations place real bids and
> offers into Ontario's wholesale electricity market and shape how the grid is dispatched. Nothing
> here should be executed by an autonomous agent without an explicit human authorization step. Read
> `mcp/ieso-mcp.yml` for the per-operation consequence classification.

## Prerequisites you cannot work around

1. **A registered market participant organization.** Start at
   <https://www.ieso.ca/Sector-Participants/Connection-Process/Overview>. There is no self-serve
   signup for any IESO API.
2. **An IESO UserID and password**, formatted `user@participantName`.
3. **Source IP allow-listing.** The endpoint is not reachable from the open internet — probed
   2026-07-27, `https://webservices.ieso.ca/emim` returned curl exit `000` (no connection). Your
   egress addresses must be registered with the IESO first.
4. **A client truststore** configured for the IESO certificate chain.

Confirm all four with `HealthCheckOperation` before doing anything else. It returns
`HealthCheckInfoType { Version, Status }`.

## Endpoints

| Environment | Endpoint |
|---|---|
| Production | `https://webservices.ieso.ca/emim` |
| Sandbox | `https://webservices-sandbox.ieso.ca/emim` |
| Market Renewal trials | `https://webservices-sandboxmrp.ieso.ca/emim` |

The service is `emim-web-service`, target namespace `http://webservices.ieso.ca/emim/`. In the IESO
MIM Web Services Toolkit (MWT), the endpoint is the `ws.client.end.point` property. Always exercise
sandbox first, and use sandbox-specific credentials — IESO warns explicitly that reusing production
credentials against the sandbox causes connection failures.

## Pick the right operation

Five bid domains, each with the same `Upload` / `Query` / `Cancel` shape. Operation names in the WSDL
are `<Domain><Action>Operation`:

| Domain | Upload | Query | Cancel |
|---|---|---|---|
| Real-time energy | `RealtimeEnergyBidUploadOperation` | `RealtimeEnergyBidQueryOperation` | `RealtimeEnergyBidCancelOperation` |
| Operating reserve | `OperatingReserveBidUploadOperation` | `OperatingReserveBidQueryOperation` | `OperatingReserveBidCancelOperation` |
| Schedule | `ScheduleBidUploadOperation` | `ScheduleBidQueryOperation` | `ScheduleBidCancelOperation` |
| Bilateral contract | `BilateralBidUploadOperation` | `BilateralBidQueryOperation` | `BilateralBidCancelOperation` |
| Daily dispatch data | `DailyDispatchBidUploadOperation` | `DailyDispatchBidQueryOperation` | — |
| Forebay | `ForebayBidUploadOperation` | `ForebayBidQueryOperation` | — |

Supporting operations: `ForebayOperation` (forebay reference data), `ResourceOperation` (your
registered resources), `MarketStatusOperation` (market and window state), `MarketMessageOperation`
(IESO market messages), `ActAsMarketParticipantOperation` (act on behalf of another participant —
treat as privileged), `HealthCheckOperation`.

## Build the submission

Every `Upload` request is a `<Domain>BidSubmitType` carrying an `UploadDateType` selector plus one
hourly element per delivery hour — `RealtimeEnergySubmitHourlyType`,
`OperatingReserveSubmitHourlyType`, `ScheduleSubmitHourlyType`, `BilateralSubmitHourlyType`. Offer
curves are built from `PriceQuantityType` pairs.

Physical parameters ride alongside the price/quantity curve and are their own types in
`xsd/ieso-mim-web-services.xsd`: `DailyEnergyRampRateType`, `HourlyEnergyRampRateType`,
`RampUpEnergyToMlpType`, `RampUpToMlpColdType` / `RampUpToMlpWarmType` / `RampUpToMlpHotType`,
`MinGenBlockDownTimeType`, `ForbiddenRegionType`, `LeadTimeType`, `StartUpOfferType`.

`Query` requests use `QueryDateType`, and `QueryStandingType` distinguishes standing submissions from
day-specific ones.

## Check the result on BOTH paths — this is the part people get wrong

Every `Upload` and `Cancel` operation returns `BidProcessingStatusType` on **success**, and the same
type inside a **fault**. A 200 with no fault does not mean a clean submission.

```
BidProcessingStatusType
  SubmissionTime  xs:dateTime
  TransactionId   string (min length 6)
  Message         1..unbounded
    Severity      INFO | WARN | ERROR
    Code          string
    Description   string
    Hour          optional — present when the message applies to one delivery hour
```

Algorithm:

1. Log `TransactionId` — it is your only correlation handle with IESO support.
2. Walk `Message[]`. If any `Severity` is `ERROR`, the submission has a problem even if no fault was
   raised.
3. Use `Message.Hour` to identify exactly which delivery hours failed.

Two faults are declared on every write operation:

- **`SubmissionFullyRejectedFault`** — nothing was accepted. Correct and resubmit the whole thing.
- **`SubmissionPartiallyRejectedFault`** — the accepted portion **stands**. Do not blindly resubmit
  the whole set; read `Message[].Hour` and resubmit only the failed hours, or you will double up.

Query operations declare a single **`QueryFault`**, which is a plain string — it does not decompose
into a code and description.

IESO does not publish the `Message.Code` registry, so treat codes as opaque and drive on `Severity`
and `Description`.

## There is no idempotency key

No IESO interface documents an idempotency header, request-deduplication token or replay-safety
contract. Retry semantics are yours to manage:

- Never blind-retry an `Upload` after a timeout. Run the matching `Query` operation first to
  establish what actually landed.
- Use the `Cancel` operations, not a compensating upload, to withdraw a submission.
- `TransactionId` is returned, not supplied — you cannot use it to deduplicate on the way in.

See `conventions/ieso-conventions.yml`.

## Watch the release train

MIM is a technical interface, so it changes only through IESO's scheduled releases. Category 3
releases — twice yearly — are defined as those that "require market participants to make changes to
their own technical interfaces", and land in sandbox roughly six weeks before production. Baseline
56.0 (production 2026-09-09) includes MM4.1 Dispatch Data Submission changes affecting this surface:
"Resources may now submit reserve loading point values equal to zero for synchronized ten-minute
operating reserve offers."

Track `changelog/ieso-changelog.yml` and
<https://www.ieso.ca/sector-participants/change-management/it-release-schedule>. Comment window
contact: `pending.changes@ieso.ca`.

## Reference material in this repo

- `wsdl/ieso-mim-web-services.wsdl` — all 23 operations and their fault bindings.
- `xsd/ieso-mim-web-services.xsd` — 62 complex types, 267 elements.
- `errors/ieso-error-codes.yml` — fault-to-operation map and the message schema.
- `data-model/ieso-data-model.yml` — the type families, grouped.
- `sandbox/ieso-sandbox.yml` — every sandbox host and the release-aligned schedule.

Sample request/response payloads are published by IESO at
<https://www.ieso.ca/-/media/Files/IESO/technical-interfaces/mp-submissions/MIM-sample-request-response.zip>,
and the first-party client toolkit at
<https://www.ieso.ca/-/media/Files/IESO/technical-interfaces/mp-submissions/MIM%20Web%20Services%20Toolkit%20-%20MWT.zip>.
