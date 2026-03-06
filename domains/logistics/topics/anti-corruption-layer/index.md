# Anti-Corruption Layer in Logistics

## Overview

An anti-corruption layer (ACL) is a translation boundary that protects a bounded context's internal domain model from being polluted by external system concepts, data structures, and terminology. In logistics, ACLs are critical because the domain integrates with numerous external systems -- ELD providers, customs authorities, load board APIs, TMS platforms, carrier APIs, and government filing systems -- each with its own data models and conventions.

## Relevance to Logistics

Logistics systems are integration-heavy. A fleet management system must consume telematics data from multiple ELD hardware vendors. A freight brokerage system must interface with dozens of load board APIs. A customs clearance system must file declarations with government agencies (CBP, CBSA, EU Customs) that each have their own XML schemas and filing protocols. Without anti-corruption layers, external data structures leak into the domain model, creating fragile coupling where a change in an external API breaks internal business logic.

## ACL Implementations in Logistics

### ELD Provider Integration

External ELD devices (KeepTruckin/Motive, Samsara, Omnitracs) each use proprietary data formats for driver logs, vehicle events, and location telemetry. The ACL translates these vendor-specific formats into the Fleet Management Context's internal Driver, Vehicle, and HOSRecord models. When an ELD provider changes their API, only the ACL adapter changes -- not the domain model.

### Customs Authority Filing

Government customs systems (CBP ACE in the US, CBSA CARM in Canada, EU CDS) each define their own XML schemas, filing procedures, and response codes. The ACL translates the internal CustomsDeclaration aggregate into the authority-specific filing format and translates response codes back into domain-meaningful ClearanceStatus values.

### Load Board APIs

External load boards (DAT, Truckstop.com, Uber Freight) use different data structures for loads, lanes, and rates. The ACL translates external load postings into the Freight Brokerage Context's LoadPosting model and translates outbound capacity postings into each load board's required format.

### TMS Platform Integration

When integrating with external Transportation Management Systems (Oracle TMS, Blue Yonder, SAP TM), the ACL translates shipment data, route plans, and carrier assignments between the external TMS model and the internal logistics domain model.

### Carrier API Integration

Each carrier (Schneider, J.B. Hunt, Werner) has its own API for tender acceptance, load tracking, and invoice submission. The ACL normalizes these into the internal Carrier and Shipment models.

## ACL Design Principles

- The ACL is a distinct layer at the boundary of a bounded context, not scattered throughout the domain model
- Translation logic converts external data types into internal value objects and entities
- The domain model never references external API data structures directly
- Each external system integration has its own ACL adapter implementing a common internal interface
- ACL adapters handle error translation, mapping external error codes to domain-meaningful exceptions
- ACL logic is tested independently of both the domain model and the external system

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) -- ACLs protect bounded context boundaries
- [Context Map](../context-map/) -- ACLs appear as integration patterns in the context map
- [Integration Patterns](../integration-patterns/) -- ACL is one of several integration patterns used between contexts
- [Value Objects](../value-objects/) -- ACLs translate external data into internal value objects
- [Logistics Standards](../logistics-standards/) -- EDI and customs filing formats are translated through ACLs
- [Regulatory Compliance](../regulatory-compliance/) -- Government system integrations require ACL protection

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 14: Maintaining Model Integrity. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 13: Integrating Bounded Contexts. Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 9: Communication Patterns. O'Reilly, 2021.
- Hohpe, Gregor & Woolf, Bobby. "Enterprise Integration Patterns." Addison-Wesley, 2003.
