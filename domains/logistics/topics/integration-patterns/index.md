# Integration Patterns in Logistics

## Overview

Integration patterns define how bounded contexts communicate with each other and with external systems. Domain-Driven Design identifies several strategic integration patterns -- shared kernel, published language, open host service, customer-supplier, conformist, and anti-corruption layer -- each appropriate for different relationship types. In logistics, integration patterns govern how Fleet Management, Route Optimization, Freight Brokerage, Customs & Clearance, Yard Management, and Last-Mile Delivery contexts exchange data with each other and with external TMS platforms, ELD providers, load boards, and government systems.

## Relevance to Logistics

Logistics is one of the most integration-intensive domains in enterprise software. A typical logistics operation integrates with ELD hardware vendors for telematics, load boards for freight matching, carrier APIs for tender and tracking, customs authorities for border clearance, mapping services for route planning, and ERP systems for financial settlement. The choice of integration pattern at each boundary determines system resilience, coupling level, and the cost of change when external systems evolve.

## Internal Integration Patterns (Between Bounded Contexts)

### Published Language

Bounded contexts communicate through a shared event schema that defines the structure and semantics of domain events. In logistics:

- Event schemas define the format for VehicleDispatched, LoadTendered, CustomsCleared, and other cross-context events
- The published language is versioned and maintained as a shared artifact
- Consumers and producers agree on the language without sharing internal domain models

### Open Host Service

A bounded context exposes a well-defined API for other contexts to query. In logistics:

- Fleet Management provides an Open Host Service for capacity queries: available vehicles by lane, equipment type, and date
- Yard Management provides an Open Host Service for dock appointment availability by facility and date
- These APIs use request-response patterns for synchronous queries that need immediate answers

### Customer-Supplier

One context (supplier) provides data that another context (customer) depends on, with the customer having influence over the supplier's API. In logistics:

- Freight Brokerage (customer) depends on Fleet Management (supplier) for capacity data
- Route Optimization (customer) depends on Fleet Management (supplier) for vehicle availability

### Partnership

Two contexts evolve together with mutual coordination, neither dominating the other. In logistics:

- Fleet Management and Route Optimization maintain a partnership: vehicle changes affect route plans and route assignments affect vehicle schedules
- Freight Brokerage and Customs & Clearance maintain a partnership for international shipments

## External Integration Patterns (With External Systems)

### Anti-Corruption Layer

ACL adapters translate external system data into internal domain models. In logistics:

- ELD provider adapters translate vendor-specific telematics data into internal Vehicle and Driver models
- Load board adapters translate DAT, Truckstop, and Uber Freight data into internal LoadPosting models
- Customs authority adapters translate CBP ACE, CBSA CARM, and EU CDS schemas into internal CustomsDeclaration models

### Conformist

The internal system accepts the external system's model without translation, typically when the external system is authoritative and non-negotiable. In logistics:

- Government customs filing formats (ACE, CARM) are accepted as-is at the filing layer
- FMCSA safety rating data is consumed in its native format

### EDI Integration

Electronic Data Interchange remains the standard for carrier-broker and shipper-carrier communication:

- EDI 204 (Motor Carrier Load Tender): Shipper/broker sends load details to carrier
- EDI 210 (Motor Carrier Freight Details and Invoice): Carrier sends invoice to shipper/broker
- EDI 214 (Transportation Carrier Shipment Status Message): Carrier sends tracking updates
- EDI 990 (Response to a Load Tender): Carrier accepts or rejects a tender
- Transport protocols: AS2 (encrypted point-to-point), SFTP (batch file transfer), VAN (value-added network)

### API-Based Integration

Modern carrier and TMS integrations increasingly use REST or GraphQL APIs:

- Carrier tracking APIs for real-time shipment visibility
- Mapping and routing APIs (Google Maps Platform, HERE, Trimble) for distance and time calculations
- Weather APIs for route condition assessment
- Fuel price APIs for surcharge calculations

## Integration Infrastructure

- **Message broker**: Apache Kafka, RabbitMQ, or cloud-native messaging (Amazon SNS/SQS, Azure Service Bus) for event-driven integration
- **API gateway**: Centralized entry point for synchronous API calls between contexts
- **EDI translator**: Software that converts between EDI formats and internal data structures
- **Integration monitoring**: Tracking message delivery, API latency, and integration health

## Relationships to Other Topics

- [Anti-Corruption Layer](../anti-corruption-layer/) -- ACL is one of the key integration patterns
- [Context Map](../context-map/) -- Integration patterns are documented in the context map
- [Event-Driven Architecture](../event-driven-architecture/) -- EDA provides the asynchronous integration backbone
- [Domain Events](../domain-events/) -- Events are the payload of asynchronous integration
- [Logistics Standards](../logistics-standards/) -- EDI and GS1 standards define the published language for external integration
- [Bounded Contexts](../bounded-contexts/) -- Integration patterns operate at bounded context boundaries

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 14: Maintaining Model Integrity. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 13: Integrating Bounded Contexts. Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 9: Communication Patterns. O'Reilly, 2021.
- Hohpe, Gregor & Woolf, Bobby. "Enterprise Integration Patterns." Addison-Wesley, 2003.
- ANSI ASC X12. "Electronic Data Interchange Standards." Accredited Standards Committee X12.
