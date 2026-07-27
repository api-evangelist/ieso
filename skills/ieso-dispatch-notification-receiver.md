---
name: ieso-dispatch-notification-receiver
description: >-
  Host the participant-side Dispatch Notification web service that the IESO calls to push real-time
  dispatch instructions for Ontario grid resources — the payload contract, the confirmation
  requirement, and why the response path still runs through the Dispatch Service Web Service.
api: ieso:ieso-dispatch-notification-service
generated: '2026-07-27'
method: generated
source: >-
  SPEC-155 "Dispatch Notification Service Web Service Design Specifications", Issue 4.0, 2025-02-26,
  and asyncapi/ieso-dispatch-notification-webhooks.yml in this repo
auth: username/password supplied by the participant TO the IESO, plus participant-side IP allow-listing
operations:
  - newDispatches
---

# Receiving IESO dispatch instructions

This is the one IESO interface where the direction reverses: **you host the service and the IESO
calls you.** SPEC-155 specifies a web service the market participant stands up so dispatch
instructions arrive in real time rather than being polled for.

> Safety-critical. A dispatch instruction tells a generator or dispatchable load what to do on
> Ontario's grid. Build for delivery reliability first.

## Onboarding

You give the IESO three things:

1. **IP allow-listing requirements** — which IESO source addresses you will accept.
2. **A username and password** the IESO will use when calling your service.
3. **Your web service endpoint** for both the **Dev** and **Production** environments.

All times in the specification are Eastern Standard Time.

## It does not replace SPEC-154

SPEC-155 paragraph 6 is explicit: the Dispatch Notification does not replace the Dispatch Service
System. You still send your **response** to each dispatch instruction back through the Dispatch
Service Web Service (SPEC-154). Push is the delivery channel only; acknowledgement stays
request/response on the other interface.

## The one operation: `newDispatches`

**Request** — `DispatchInstructions` (0..*) containing `DispatchInstruction` (0..*). Every field is
optional (0..1), so code defensively; nothing is guaranteed present.

Identity and routing:

| Field | Type | Meaning |
|---|---|---|
| `MESSAGE_ID` | String | Unique identifier assigned to the dispatch instruction |
| `PARTICIPANT_NAME` | String | Market Participant Short Name |
| `RESOURCE_ID` | String | Name of the resource being dispatched |
| `DATE_SENT` | DateTime | When the IESO issued it |
| `LAST_UPDATED` | DateTime | When it was last altered |

Classification:

| Field | Type | Values |
|---|---|---|
| `DISPATCH_TYPE` | DispatchType | `ENG`, `ORA`, `ORD` (not being used), `RESV`, `RGR`, `RGS`, `START`, `EXTEND`, `DECOM` |
| `STATE` | DispatchState | `New`, `Timed Out`, `Accepted`, `Rejected` |
| `ACTIVE` | Boolean | Whether this is the last confirmed dispatch for the resource per dispatch type |

Timing:

| Field | Type | Notes |
|---|---|---|
| `DELIVERY_DATE` | DateTime | `YYYY-MM-DD` |
| `DELIVERY_HOUR` | Integer | 1–24 inclusive |
| `DELIVERY_INTERVAL` | Integer | 1–12 inclusive, the five-minute interval |
| `DELIVERY_START_TIME` / `DELIVERY_STOP_TIME` | DateTime | `YYYY-MM-DDTHH:MM:SS`, contract activation |
| `EXPIRES_AT` | DateTime | End of the active window |
| `EFFECTIVE_TIME` | DateTime | Time associated with `START`, `EXTEND`, `DECOM` |
| `MLP_TIME` | DateTime | First time the resource is scheduled at or above Minimum Load Point |
| `SYNC_TIME` / `ALT_SYNC_TIME` | DateTime | Scheduled and alternate synchronization |

Instruction content:

| Field | Type | Notes |
|---|---|---|
| `AMOUNT` | Double | Value assigned to the dispatch; **meaning depends on `DISPATCH_TYPE`** |
| `LIMIT_TYPE` | LimitType | `FIX` manually set, `MAX` limited to maximum output, `MIN` limited to minimum output, `OTD` manual on-demand one-time dispatch |
| `VG_OI` | String | Variable Generation Obligation Indicator — `Mandatory`, `Release`, or null for non-Variable Generators |
| `RESERVE_CLASS` | String | On `RESV` dispatches — `10S` ten-minute spinning, `10N` ten-minute non-spinning, `30R` thirty-minute reserve |
| `REGULATION_RANGE` | Double | For regulation dispatches |
| `RESPONDER` | String | Section 2 states it is not used and will be left blank |

Always branch on `DISPATCH_TYPE` before interpreting `AMOUNT`.

## Respond `Confirmed: true` — nothing else counts

**Response:** a single required `Confirmed` Boolean.

The two business rules from SPEC-155 section 2.1 are absolute:

1. *"IESO will consider any non 'confirmed' responses as delivery failures."*
2. *"IESO will not process any SOAP faults returned from the web service."*

So: confirm receipt as soon as you have durably persisted the instruction. Do **not** confirm only
after downstream processing succeeds, and do **not** raise a SOAP fault to signal a problem — it will
be discarded and the dispatch counted as undelivered. Handle your own failures out of band, then
respond through SPEC-154.

## Size your HTTP server for large payloads

SPEC-155 section 4, verbatim:

> Dispatch instructions can become large in size, if the Dispatch Notification Service is not
> configured properly error may occur when a set of new dispatches is received. It is recommended to
> properly set the HTTPS request limit on the server which is hosting Dispatch Notification Service
> to avoid potential errors.

A single call can carry an unbounded `DispatchInstruction` list. Raise your maximum request body size
and request timeout before go-live, and test with a large batch in the Dev environment.

## Idempotency is on you

There is no idempotency contract. `MESSAGE_ID` plus `LAST_UPDATED` is your natural dedupe key —
persist on `MESSAGE_ID`, and treat a repeat with a newer `LAST_UPDATED` as an update rather than a
duplicate. `ACTIVE` tells you whether an instruction is the current one for that resource and
dispatch type.

## Reference material in this repo

- `asyncapi/ieso-dispatch-notification-webhooks.yml` — the full field, type and enum contract.
- `conventions/ieso-conventions.yml` — cross-cutting IESO semantics.
- SPEC-155: <https://www.ieso.ca/-/media/Files/IESO/technical-interfaces/dispatch-service-system/SPEC-155-Dispatch-Notification-Service-Web-Service-Design-Specifications.pdf>
- SPEC-154 (the response path): <https://www.ieso.ca/-/media/Files/IESO/technical-interfaces/dispatch-service-system/SPEC-154-Dispatch-Service-Web-Service-Design-Specifications.pdf>
