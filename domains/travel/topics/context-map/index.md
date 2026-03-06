# Context Map in Travel

## Overview

A context map is a visual and conceptual representation of the relationships and integration patterns between bounded contexts in a system. It documents how contexts communicate, which context is upstream or downstream, and what translation mechanisms exist at the boundaries. In travel systems, the context map reveals the complex web of dependencies between search, reservation, pricing, passenger, itinerary, and payment contexts, as well as external GDS and airline systems.

## Relevance to Travel

Travel platforms integrate with numerous external systems (Amadeus, Sabre, Travelport, airline direct connects, hotel aggregators, payment processors, IATA BSP) while coordinating multiple internal contexts. The context map makes these relationships explicit, enabling teams to understand dependencies, plan integration strategies, and manage the risk of upstream changes breaking downstream consumers.

## Context Relationships

### Search & Availability Context

- **Upstream supplier to** Reservation Context: Search results feed into booking creation
- **Downstream consumer of** GDS systems (Amadeus, Sabre, Travelport) via Anti-Corruption Layers
- **Downstream consumer of** Pricing & Fare Context for display pricing
- **Relationship type**: Conformist to GDS response formats behind ACL; Customer-Supplier with Reservation

### Reservation Context

- **Downstream consumer of** Search & Availability Context for selected offers
- **Upstream supplier to** Itinerary Context for confirmed booking segments
- **Upstream supplier to** Payment & Billing Context for payment triggers
- **Downstream consumer of** Passenger Management Context for traveler details
- **Published Language** with GDS for PNR creation and modification via AIRIMP messaging
- **Relationship type**: Partnership with Pricing; Customer-Supplier with Payment

### Pricing & Fare Context

- **Upstream supplier to** Search & Availability Context for fare quotes
- **Upstream supplier to** Reservation Context for confirmed pricing
- **Downstream consumer of** external tariff data (ATPCO, airline-filed fares)
- **Relationship type**: Conformist to ATPCO fare data behind ACL; Open Host Service to internal contexts

### Passenger Management Context

- **Upstream supplier to** Reservation Context for passenger profiles
- **Upstream supplier to** Itinerary Context for traveler preferences
- **Shared Kernel** with Reservation Context for basic passenger identity
- **Relationship type**: Open Host Service exposing passenger profiles

### Itinerary Context

- **Downstream consumer of** Reservation Context for confirmed segments
- **Downstream consumer of** Pricing & Fare Context for display pricing
- **Downstream consumer of** Passenger Management Context for traveler details
- **Relationship type**: Conformist to Reservation model; Anti-Corruption Layer for GDS schedule data

### Payment & Billing Context

- **Downstream consumer of** Reservation Context for payment triggers
- **Downstream consumer of** Pricing & Fare Context for amounts and taxes
- **Upstream supplier to** external payment processors (acquirers, card networks)
- **Published Language** with IATA BSP for settlement
- **Relationship type**: Customer-Supplier with Reservation; Conformist to payment processor APIs behind ACL

### External Systems

- **GDS platforms** (Amadeus, Sabre, Travelport): Upstream suppliers accessed through Anti-Corruption Layers
- **IATA BSP**: Settlement partner using Published Language
- **ATPCO**: Fare data supplier, consumed via ACL in Pricing & Fare Context
- **Payment processors**: Downstream services accessed through ACL in Payment & Billing Context

## Integration Patterns on the Map

- **Anti-Corruption Layer**: Used at every GDS boundary and payment processor boundary
- **Published Language**: IATA NDC for airline distribution, AIRIMP for reservation messaging, BSP for settlement
- **Shared Kernel**: Limited to Passenger identity shared between Passenger Management and Reservation
- **Open Host Service**: Pricing & Fare Context exposes a fare calculation service consumed by Search and Reservation
- **Customer-Supplier**: Reservation Context is the primary customer of Search; Payment is the primary customer of Reservation

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) -- The nodes on the context map
- [Strategic Design](../strategic-design/) -- Context maps are a key strategic design artifact
- [Anti-Corruption Layer](../anti-corruption-layer/) -- Translation boundaries shown on the map
- [Integration Patterns](../integration-patterns/) -- Detailed integration mechanisms between contexts
- [Ubiquitous Language](../ubiquitous-language/) -- Each context on the map has its own language

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 14: Maintaining Model Integrity. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 3: Context Maps. Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 7: Context Mapping. O'Reilly, 2021.
- Brandolini, Alberto. "Introducing EventStorming." Leanpub, 2021.
