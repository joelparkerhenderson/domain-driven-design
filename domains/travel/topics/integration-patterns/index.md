# Integration Patterns in Travel

## Overview

Integration patterns define how bounded contexts communicate with each other and with external systems. Domain-Driven Design identifies several strategic integration patterns -- Shared Kernel, Customer-Supplier, Conformist, Anti-Corruption Layer, Open Host Service, and Published Language -- each suited to different relationship dynamics. In travel, these patterns govern how internal contexts interact and how the system connects to GDS platforms, airline APIs, payment processors, and industry settlement systems.

## Relevance to Travel

Travel systems are inherently integration-heavy. A single booking may involve querying three GDS platforms, confirming with an airline, processing payment through a gateway, reporting to IATA BSP, and synchronizing with a corporate travel management system. Each integration has different power dynamics, data ownership patterns, and change cadences. Integration patterns provide a vocabulary for describing and managing these relationships.

## Strategic Integration Patterns

### Anti-Corruption Layer (ACL)

Used when integrating with external systems whose models differ significantly from the internal domain model.

- **GDS integrations**: Amadeus, Sabre, and Travelport each have proprietary data formats. ACLs translate their responses into the internal ubiquitous language.
- **Payment processors**: Stripe, Adyen, and Worldpay APIs are normalized through ACLs into the internal Payment model.
- **ATPCO fare data**: Complex tariff filing formats are translated into internal FareRule and FareComponent value objects.
- **Legacy airline systems**: Direct connections to airline departure control and inventory systems use ACLs to isolate legacy formats.

### Published Language

A well-documented, shared language used for communication between systems.

- **IATA NDC**: The New Distribution Capability standard defines a published language for airline offer and order management via XML/JSON APIs.
- **AIRIMP messaging**: The standard for inter-airline reservation messaging defines the published language for PNR exchange.
- **IATA BSP**: Settlement reports follow a published format for financial reconciliation between agents and airlines.
- **PNR format**: The Passenger Name Record structure is a de facto published language across the travel industry.

### Open Host Service

A bounded context exposes a well-defined API for other contexts to consume.

- **Pricing & Fare Context**: Exposes a fare calculation service consumed by Search and Reservation contexts.
- **Passenger Management Context**: Exposes a profile lookup service consumed by Reservation and Itinerary contexts.
- **Search & Availability Context**: Exposes a search API consumed by front-end applications and partner channels.

### Customer-Supplier

An upstream context (supplier) provides data or services to a downstream context (customer), with the customer's needs influencing the supplier's development.

- **Search (supplier) to Reservation (customer)**: Search results are structured to meet the Reservation Context's booking requirements.
- **Reservation (supplier) to Itinerary (customer)**: Confirmed segments are provided in a format suitable for itinerary assembly.
- **Reservation (supplier) to Payment (customer)**: Booking events trigger payment operations with necessary financial data.

### Conformist

A downstream context accepts the upstream context's model without translation, typically when the downstream has no leverage to influence the upstream.

- **Airline schedule data**: The Itinerary Context may conform to airline schedule formats (SSIM) for timetable data rather than building a translation layer.
- **Airport reference data**: Airport codes, names, time zones, and terminal information are consumed as-is from IATA reference databases.

### Shared Kernel

A small, shared subset of the domain model used by two or more contexts, maintained jointly.

- **Passenger identity**: Basic passenger name and identifier shared between Passenger Management and Reservation contexts. Changes to this shared kernel require coordination between both teams.
- **This pattern is used sparingly** in travel because the risks of tight coupling outweigh the convenience of sharing.

### Partnership

Two contexts that evolve together, with teams coordinating their development and releases.

- **Reservation and Pricing**: These contexts have a partnership relationship -- changes to fare structures in Pricing require corresponding changes in how Reservation stores and validates pricing data. Teams coordinate releases.

## External System Integration

### GDS Integration Architecture

- Multiple GDS platforms are accessed through provider-specific ACLs
- A GDS gateway layer routes requests to the appropriate provider based on content type, geography, or airline preference
- Response normalization ensures the domain model is independent of any single GDS
- Failover and fallback strategies handle GDS outages

### Payment Processor Integration

- Payment processors are accessed through a payment gateway ACL
- Tokenization occurs at the processor boundary -- no raw card data enters the domain
- Multiple processors may be supported for redundancy and geographic coverage
- 3D Secure and Strong Customer Authentication (SCA) flows are managed at this boundary

### Airline Direct Connect (NDC)

- Airlines increasingly offer direct APIs bypassing GDS, using IATA NDC standard
- NDC connections are integrated through airline-specific ACLs
- NDC offers richer content (ancillaries, branded fares, rich media) than traditional GDS channels
- The system must support both GDS and NDC channels simultaneously

## Relationships to Other Topics

- [Anti-Corruption Layer](../anti-corruption-layer/) -- The most commonly used integration pattern in travel
- [Context Map](../context-map/) -- Integration patterns are annotated on the context map
- [Bounded Contexts](../bounded-contexts/) -- Integration patterns define how contexts relate
- [Event-Driven Architecture](../event-driven-architecture/) -- Events are a key integration mechanism
- [Travel Standards](../travel-standards/) -- Standards define the published languages used in integrations

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 14: Maintaining Model Integrity. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 13: Integrating Bounded Contexts. Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 7: Context Mapping Patterns. O'Reilly, 2021.
- Hohpe, Gregor & Woolf, Bobby. "Enterprise Integration Patterns." Addison-Wesley, 2003.
