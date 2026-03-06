# Bounded Contexts in Logistics

## Overview

A bounded context is a logical boundary within which a specific domain model is defined and consistent. In logistics, each bounded context encapsulates a distinct area of the transportation and freight movement lifecycle, with its own ubiquitous language, aggregates, and business rules. The same real-world concept -- such as "shipment" or "carrier" -- may have different meanings and attributes in different bounded contexts.

## Relevance to Logistics

Logistics operations span multiple organizational functions, each with its own expertise, vocabulary, and operational cadence. A fleet manager thinks about vehicles and maintenance schedules. A freight broker thinks about lanes and spot rates. A customs agent thinks about HS codes and duty calculations. A yard supervisor thinks about dock doors and trailer positions. Without bounded contexts, these different perspectives would collapse into a single, unmanageable model where every change risks unintended consequences across the entire system.

## Logistics Bounded Contexts

### Fleet Management Context

Manages the lifecycle of transportation assets and personnel. Owns the Vehicle and Driver aggregates, maintenance schedules, compliance records, and telematics data. The concept of "vehicle" in this context includes detailed mechanical specifications, maintenance history, and regulatory certifications that are irrelevant to other contexts.

### Route Optimization Context

Plans and optimizes freight movement from origin to destination. Owns the Route and LoadPlan aggregates, scheduling algorithms, and real-time rerouting logic. A "shipment" in this context is primarily a set of constraints (weight, dimensions, pickup window, delivery window) that feed into optimization algorithms, rather than a legal or financial document.

### Freight Brokerage Context

Manages the commercial relationship between shippers and carriers. Owns the LoadPosting, FreightQuote, and BrokerContract aggregates. A "carrier" in this context is a business entity with contract terms, performance history, and rate structures, rather than a physical fleet of vehicles.

### Customs & Clearance Context

Handles import/export documentation and regulatory compliance for international freight. Owns the CustomsDeclaration, TradeCompliance, and ClearanceRequest aggregates. A "shipment" in this context is a legal declaration of goods crossing borders, with HS codes, duty calculations, and country-of-origin documentation.

### Yard Management Context

Coordinates the physical movement of trailers within a facility's yard. Owns the DockAppointment, TrailerPosition, and GateTransaction aggregates. A "trailer" in this context is a physical asset with a specific yard location, dock assignment, and loading/unloading status, rather than a fleet asset with maintenance history.

### Last-Mile Delivery Context

Manages the final delivery leg from distribution hub to end recipient. Owns the DeliveryRoute, DeliveryStop, and ProofOfDelivery aggregates. A "driver" in this context is a delivery agent with a mobile device and a sequence of stops, rather than a long-haul operator with HOS compliance requirements.

## Context Boundary Principles

- Each bounded context has a single team responsible for its model and code
- Concepts that appear in multiple contexts (e.g., "shipment," "carrier," "driver") have context-specific definitions
- Communication between contexts uses domain events, published languages, or anti-corruption layers
- Each context can evolve its internal model independently without breaking other contexts
- Database schemas are not shared between bounded contexts

## Relationships to Other Topics

- [Strategic Design](../strategic-design/) -- Bounded contexts are the primary output of strategic design
- [Context Map](../context-map/) -- Documents the relationships between bounded contexts
- [Ubiquitous Language](../ubiquitous-language/) -- Each bounded context has its own consistent vocabulary
- [Anti-Corruption Layer](../anti-corruption-layer/) -- Protects bounded context models from external system pollution
- [Aggregates](../aggregates/) -- Each bounded context contains its own aggregate roots
- [Domain Events](../domain-events/) -- Events flow between bounded contexts for integration

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 14: Maintaining Model Integrity. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 2: Domains, Subdomains, and Bounded Contexts. Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 6: Bounded Contexts. O'Reilly, 2021.
- Vernon, Vaughn. "Domain-Driven Design Distilled." Chapter 4: Strategic Design with Context Mapping. Addison-Wesley, 2016.
