# Anti-Corruption Layer in Energy

## Overview

An anti-corruption layer (ACL) is a translation boundary that prevents external system concepts, data formats, and assumptions from leaking into a bounded context's internal domain model. The ACL acts as an adapter that converts between the external system's language and the internal ubiquitous language. In the energy domain, ACLs protect the domain model from the complexity of legacy SCADA systems, ISO/RTO market interfaces, external metering platforms, and regulatory reporting systems.

## Relevance to Energy

Energy systems integrate with numerous external systems that have their own data models, protocols, and assumptions. ISO/RTO market platforms use specific bid formats and settlement schemas. SCADA systems communicate via proprietary protocols with their own data representations. Legacy billing platforms carry decades of accumulated data model decisions. Without ACLs, these external models would distort the internal domain model, creating a "big ball of mud" where no one can reason clearly about the domain.

## Key ACL Boundaries in Energy

### ISO/RTO Market Interface ACL

The Trading & Market Context maintains an ACL to translate between ISO/RTO market platforms and the internal trading model.

- Translates ISO-specific bid formats (e.g., PJM, ERCOT, CAISO formats) into internal MarketBid aggregates
- Maps external settlement line items to internal Settlement value objects
- Converts ISO-specific node identifiers to internal grid node references
- Normalizes different ISO pricing structures (LMP, zonal pricing) into a consistent internal price model
- Handles versioning differences across ISO platform updates without impacting the domain model

### SCADA System ACL

The Transmission & Distribution Context maintains an ACL to translate between SCADA telemetry and the internal grid model.

- Converts SCADA point data (using proprietary tag naming conventions) into domain-meaningful measurements like LineFlow, BusVoltage, and SubstationStatus
- Maps SCADA alarm codes to internal Fault and Outage domain events
- Translates SCADA control commands into internal dispatch and switching operations
- Buffers high-frequency SCADA data into the temporal granularity appropriate for the domain model

### Legacy Billing Platform ACL

The Metering & Billing Context maintains an ACL when integrating with legacy customer information and billing systems.

- Translates legacy account structures into internal CustomerAccount aggregates
- Maps legacy rate codes to internal Tariff value objects
- Converts legacy billing period definitions to internal BillingCycle representations
- Normalizes legacy address formats to internal validated Address value objects

### Regulatory Reporting ACL

The Regulatory & Grid Context maintains an ACL to translate between internal domain models and external regulatory reporting formats.

- Converts internal compliance data into NERC compliance filing formats
- Translates internal generation and transmission data into FERC Form 714 and Form 1 formats
- Maps internal demand response event records to regulatory reporting schemas
- Adapts internal environmental monitoring data to EPA reporting formats

### Weather and Forecasting Service ACL

The Renewable Integration Context maintains an ACL to translate between external weather data providers and the internal forecasting model.

- Converts vendor-specific weather forecast formats into internal WeatherForecast value objects
- Normalizes different spatial resolutions and temporal granularities across weather providers
- Maps external irradiance and wind speed data to internal generation forecast inputs
- Handles provider-specific error codes and data quality indicators

## ACL Design Principles

- **Isolate translation logic**: All format conversion, protocol adaptation, and data mapping lives in the ACL, not in the domain model
- **Fail fast on invalid external data**: The ACL validates incoming data and rejects or quarantines data that cannot be meaningfully translated
- **Version external interfaces independently**: When an ISO changes its bid format, only the ACL changes; the domain model remains stable
- **Test ACLs with contract tests**: Verify that the ACL correctly translates known external inputs to expected internal representations

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) -- ACLs protect bounded context boundaries
- [Context Map](../context-map/) -- ACLs appear as integration patterns on the context map
- [Integration Patterns](../integration-patterns/) -- ACLs are one of several integration patterns
- [Strategic Design](../strategic-design/) -- ACLs are a strategic design mechanism
- [Energy Standards](../energy-standards/) -- ACLs often translate between standard formats and internal models

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 14: Maintaining Model Integrity. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 3: Context Maps. Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 9: Integration Patterns. O'Reilly, 2021.
- Hohpe, Gregor & Woolf, Bobby. "Enterprise Integration Patterns." Addison-Wesley, 2003.
