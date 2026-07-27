# IESO (ieso)

The Independent Electricity System Operator (IESO) is the Crown corporation that operates Ontario's bulk electricity system and administers the province's wholesale electricity market, balancing supply and demand in real time, running the day-ahead and real-time energy and operating reserve markets, procuring capacity and transmission rights, planning the provincial grid, and delivering the Save on Energy conservation programs. It sits at the top of Canada's largest provincial electricity value chain — upstream of the licensed local distribution companies that bill retail customers — which is why its API posture splits cleanly in two. Its market and system data surface is genuinely open: a public report repository at reports-public.ieso.ca serves 139 report directories of demand, price, generation-by-fuel, intertie flow, outage, adequacy and settlement data as CSV, XML and XLSX over anonymous HTTPS with no account, key or registration, backed by published XSD schemas and a public interface specification. Its programmatic APIs are the opposite: the Axway SecureTransport REST API at reports.ieso.ca/api/v1.4, the SOAP Market Information Management web services, and the Appian-based Online IESO registration, retrofit, dispatch and outage web services are all reserved for registered Ontario market participants with IESO-issued machine accounts. IESO publishes no consumer energy-data API: Ontario's Green Button mandate (O. Reg. 633/21) binds licensed electricity and gas distributors, not the system operator, so there is no retail usage or billing surface here at all.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/ieso/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/ieso/refs/heads/main/apis.yml)

## Tags

- Energy
- Canada
- Electricity
- Energy Markets
- Grid
- System Operator
- Market Data
- Open Data
- Ontario
- Demand Response
- Renewables

## Timestamps

- **Created:** 2026-07-27
- **Modified:** 2026-07-27

## APIs

### IESO Public Reports Repository

The IESO's open market and system data surface. A flat HTTPS file repository publishing 139 report directories — hourly Ontario and market demand, HOEP and nodal/zonal prices, day-ahead and pre-dispatch LMPs, generator output and capability, generation by fuel type, intertie schedules and flows, transmission and generation outages, operating reserve, adequacy, transmission rights auction results and settlement uplift — as CSV, XML and XLSX. Report URLs follow a documented, stable pattern and are readable anonymously with no account, API key, registration or licence acceptance.

- **Human URL:** [https://www.ieso.ca/en/Power-Data/Data-Directory](https://www.ieso.ca/en/Power-Data/Data-Directory)
- **Base URL:** `https://reports-public.ieso.ca/public/`

#### Tags

- Market Data
- Open Data
- Electricity
- Reports
- Ontario

#### Properties

- [Documentation](https://www.ieso.ca/en/Power-Data/Data-Directory)
- [Documentation](https://www.ieso.ca/en/Power-Data)
- [Specification](https://www.ieso.ca/-/media/Files/IESO/technical-interfaces/xml-automated-docs/IMO_SPEC_0100.pdf)
- [Specification](https://www.ieso.ca/-/media/Files/IESO/technical-interfaces/xml-automated-docs/STD-2.pdf)
- [XSD](xsd/ieso-imo-document-r1.xsd)
- [XSD](xsd/ieso-document-r1.xsd)
- [XSD](xsd/ieso-intertie-schedule-flow-r1.xsd)
- [XSD](xsd/ieso-gen-output-capability-r1.xsd)
- [XSD](xsd/ieso-disp-uncons-hoep-r1.xsd)
- [XSD](xsd/ieso-disp-advisory-r1.xsd)
- [XSD](xsd/ieso-dacp-predisp-energy-r1.xsd)
- [XSD](xsd/ieso-dacp-predisp-prices-r1.xsd)
- [XSD](xsd/ieso-dacp-adequacy-r1.xsd)
- [XSD](xsd/ieso-dacp-reliability-commit-r1.xsd)
- [XSD](xsd/ieso-dacp-commit-r1.xsd)
- [XSD](xsd/ieso-dacp-check-source-ade-r1.xsd)
- [XSD](xsd/ieso-financial-binding-status-r1.xsd)

### IESO Reports Site REST API

A RESTful file-access API provided by Axway SecureTransport, used to automate retrieval of reports from the IESO Reports Site. Accepts and returns both JSON and XML, and exposes file and directory listing, file property inspection and file download. Every request must carry `?idp_id=ieso` and HTTP Basic credentials. Documented only for the confidential participant report repository — probed anonymously and returned HTTP 401.

- **Human URL:** [https://www.ieso.ca/sector-participants/technical-interfaces](https://www.ieso.ca/sector-participants/technical-interfaces)
- **Base URL:** `https://reports.ieso.ca/api/v1.4`

#### Tags

- Reports
- File Transfer
- Market Participants
- REST

#### Properties

- [Documentation](https://www.ieso.ca/-/media/Files/IESO/technical-interfaces/api-reports-guide/IESO_Reports_API_Guide.pdf)
- [Specification](https://www.ieso.ca/-/media/Files/IESO/technical-interfaces/xml-automated-docs/IMO_SPEC_0100.pdf)
- [Documentation](https://www.ieso.ca/sector-participants/technical-interfaces)

### IESO Market Information Management (MIM) Web Services

SOAP web services that let registered Ontario market participants submit and retrieve market information programmatically. The published WSDL declares 23 operations covering real-time energy bids, operating reserve bids, schedule bids, bilateral contract bids, daily dispatch data, forebay data, resource and market status queries, market messages and a health check. Access requires an IESO UserID and password plus source IP whitelisting and a client truststore.

- **Human URL:** [https://www.ieso.ca/sector-participants/technical-interfaces](https://www.ieso.ca/sector-participants/technical-interfaces)
- **Base URL:** `https://webservices.ieso.ca/emim`

#### Tags

- SOAP
- Bids and Offers
- Market Participants
- Energy Markets

#### Properties

- [WSDL](wsdl/ieso-mim-web-services.wsdl)
- [XSD](xsd/ieso-mim-web-services.xsd)
- [Documentation](https://www.ieso.ca/-/media/Files/IESO/technical-interfaces/mp-submissions/MWT-User-Guide.pdf)
- [Examples](https://www.ieso.ca/-/media/Files/IESO/technical-interfaces/mp-submissions/MIM-sample-request-response.zip)
- [SDK](https://www.ieso.ca/-/media/Files/IESO/technical-interfaces/mp-submissions/MIM%20Web%20Services%20Toolkit%20-%20MWT.zip)

### IESO Registration System Facilities API

A REST API on the Appian-based Online IESO platform for retrieving and maintaining facility registration data. Authentication is by Appian API key, supplied as HTTP Basic, an `Appian-API-Key` header, or an `Authorization: Bearer` header, over mandatory HTTPS. There is no self-serve key issuance — the organization's registered IESO Rights Administrator must request an API machine account.

- **Human URL:** [https://www.ieso.ca/sector-participants/technical-interfaces](https://www.ieso.ca/sector-participants/technical-interfaces)
- **Base URL:** `https://online.ieso.ca/suite/webapi/reg-v1-facilities`

#### Tags

- Registration
- Facilities
- Market Participants
- REST

#### Properties

- [Documentation](https://www.ieso.ca/-/media/Files/IESO/technical-interfaces/registration-system/FacilityAPISpecification-SPEC-249.pdf)
- [Documentation](https://www.ieso.ca/sector-participants/technical-interfaces)

### IESO Retrofit System API

A web service API for the Retrofit program under Save on Energy, IESO's conservation and demand management portfolio, on the Online IESO Appian platform using API-key authentication. Access requires an IESO-issued API machine account.

- **Human URL:** [https://www.ieso.ca/sector-participants/technical-interfaces](https://www.ieso.ca/sector-participants/technical-interfaces)
- **Base URL:** `https://online.ieso.ca/suite/webapi`

#### Tags

- Energy Efficiency
- Demand Response
- Retrofit
- REST

#### Properties

- [Documentation](https://www.ieso.ca/-/media/Files/IESO/technical-interfaces/retrofit-system/RetrofitAPI-SPEC-188.pdf)
- [Documentation](https://www.ieso.ca/sector-participants/technical-interfaces)

### IESO Retrofit Service Provider API

A companion web service API for service providers participating in the Save on Energy Retrofit program, covering the service-provider side of project submission and management. Participant-only; requires an IESO-issued API account.

- **Human URL:** [https://www.ieso.ca/sector-participants/technical-interfaces](https://www.ieso.ca/sector-participants/technical-interfaces)

#### Tags

- Energy Efficiency
- Service Providers
- Retrofit

#### Properties

- [Documentation](https://www.ieso.ca/-/media/Files/IESO/technical-interfaces/retrofit-system/RetrofitServiceProviderAPI-SPEC-232.pdf)
- [Documentation](https://www.ieso.ca/sector-participants/technical-interfaces)

### IESO Dispatch Service Web Service

A web service through which registered market participants retrieve dispatch instructions issued by the IESO and send back acknowledgements and responses. The design specification is published openly, but no base URL is disclosed and access is restricted to registered market participants operating dispatchable facilities.

- **Human URL:** [https://www.ieso.ca/sector-participants/technical-interfaces](https://www.ieso.ca/sector-participants/technical-interfaces)

#### Tags

- Dispatch
- Real Time
- Market Participants

#### Properties

- [Documentation](https://www.ieso.ca/-/media/Files/IESO/technical-interfaces/dispatch-service-system/SPEC-154-Dispatch-Service-Web-Service-Design-Specifications.pdf)
- [Documentation](https://www.ieso.ca/sector-participants/technical-interfaces)

### IESO Dispatch Notification Service Web Service

A push-notification web service that delivers automated notifications of dispatch instructions to registered market participants rather than requiring them to poll.

- **Human URL:** [https://www.ieso.ca/sector-participants/technical-interfaces](https://www.ieso.ca/sector-participants/technical-interfaces)

#### Tags

- Dispatch
- Notifications
- Push
- Market Participants

#### Properties

- [Documentation](https://www.ieso.ca/-/media/Files/IESO/technical-interfaces/dispatch-service-system/SPEC-155-Dispatch-Notification-Service-Web-Service-Design-Specifications.pdf)
- [Documentation](https://www.ieso.ca/sector-participants/technical-interfaces)

### IESO Outage Coordination and Scheduling System (OCSS) Web Service

A web service for submitting and managing transmission and generation outage requests. The design specification is public; the service is restricted to registered market participants. The resulting outage data is republished anonymously through the public report repository — the write side is gated, the read side is open.

- **Human URL:** [https://www.ieso.ca/sector-participants/technical-interfaces](https://www.ieso.ca/sector-participants/technical-interfaces)

#### Tags

- Outages
- Scheduling
- Transmission
- Market Participants

#### Properties

- [Documentation](https://www.ieso.ca/-/media/Files/IESO/technical-interfaces/outage-management/WebServiceDesignSpec_SPEC-113.pdf)
- [Documentation](https://www.ieso.ca/sector-participants/technical-interfaces)

## Common Properties

- [Website](https://www.ieso.ca/)
- [Documentation](https://www.ieso.ca/sector-participants/technical-interfaces)
- [Documentation](https://www.ieso.ca/en/Power-Data/Data-Directory)
- [Terms of Service](https://www.ieso.ca/Terms-of-Use)
- [Blog](https://www.ieso.ca/en/Corporate-IESO/Blog)
- [LinkedIn](https://www.linkedin.com/company/ieso)
- [Email](mailto:customer.relations@ieso.ca)

## Mandate Posture

- **Mandate regime:** `other` — a market-transparency publication obligation under Ontario's electricity Market Rules, **not** a consumer data right.
- **Mandate status:** `live-implemented` — verified by anonymous HTTP 200 against `https://reports-public.ieso.ca/public/` and a live 2026 demand CSV on 2026-07-27, and described in IESO's own published specification IMO_SPEC_0100.
- **Green Button Ontario (O. Reg. 633/21):** does **not** apply. The obligation binds "energy providers", defined as most electricity and natural gas distributors. IESO is the system operator, holds no retail customer accounts, and publishes no ESPI surface.
- **Consumer data API:** none, and none expected at this tier.
- **Market data open:** yes — 139 report directories, anonymous, no registration, no open-data licence applied.
- **Access gate:** nothing at all for market data; market-participant registration plus an IESO-issued machine account for every programmatic API.
- **Data standard:** proprietary IESO XML report schemas (STD-2 / IMO_SPEC_0100, `IMODocument_r1.xsd`) plus ANSI X12 EDI 867 for meter data. No Green Button/ESPI, CIM, or CDR reference found. No OpenAPI or AsyncAPI published anywhere.

See [review.yml](review.yml) for the full probe log, HTTP statuses and artifact provenance.

## Maintainers

**FN:** Kin Lane
**Email:** kin@apievangelist.com
