# Context Map in Logistics

## Overview

A context map is a visual and conceptual representation of the relationships and interactions between bounded contexts in a domain. In logistics, the context map documents how Fleet Management, Route Optimization, Freight Brokerage, Customs & Clearance, Yard Management, and Last-Mile Delivery contexts communicate, share data, and depend on one another. It identifies integration patterns, team relationships, and data translation requirements.

## Relevance to Logistics

Logistics operations involve continuous coordination between fleet assets, route plans, freight transactions, regulatory processes, yard operations, and delivery execution. The context map makes these dependencies explicit so that teams understand which contexts are upstream, which are downstream, and what translation is needed at each boundary. Without a context map, integration points become hidden, leading to brittle coupling and cascading failures when one context changes.

## Logistics Context Map

### Fleet Management <-> Route Optimization

- **Relationship**: Partnership
- **Integration**: Fleet Management publishes vehicle availability and driver status; Route Optimization consumes this to plan feasible routes. Route Optimization publishes route assignments; Fleet Management consumes these to update vehicle and driver schedules.
- **Shared data**: Vehicle capacity, driver availability windows, current vehicle location

### Route Optimization -> Freight Brokerage

- **Relationship**: Upstream-Downstream (Route Optimization is downstream)
- **Integration**: Freight Brokerage publishes accepted loads with origin, destination, and time constraints. Route Optimization consumes these loads and plans routes. Route Optimization publishes route confirmations back to Freight Brokerage.
- **Translation**: Load details from brokerage are translated into route constraints via an anti-corruption layer

### Freight Brokerage -> Fleet Management

- **Relationship**: Customer-Supplier (Freight Brokerage is the customer)
- **Integration**: Freight Brokerage queries Fleet Management for available capacity on specific lanes. Fleet Management provides capacity data to support carrier matching decisions.

### Route Optimization -> Yard Management

- **Relationship**: Upstream-Downstream
- **Integration**: Route Optimization publishes estimated arrival times for inbound vehicles. Yard Management uses these to pre-schedule dock appointments and prepare yard positions.

### Yard Management -> Last-Mile Delivery

- **Relationship**: Upstream-Downstream
- **Integration**: Yard Management publishes trailer loading completion events. Last-Mile Delivery consumes these to initiate delivery route dispatch from the facility.

### Customs & Clearance <-> Freight Brokerage

- **Relationship**: Partnership
- **Integration**: Freight Brokerage provides shipment details for international loads. Customs & Clearance publishes clearance status updates that Freight Brokerage uses to confirm delivery feasibility.

### External Systems

- **ELD Providers**: Conformist relationship -- Fleet Management conforms to external ELD data formats
- **Customs Authorities (CBP, CBSA)**: Conformist relationship -- Customs & Clearance conforms to government filing requirements
- **TMS Platforms**: Anti-corruption layer -- External TMS data is translated into internal domain models
- **Load Board APIs**: Anti-corruption layer -- Freight Brokerage translates external load board data

## Context Map Patterns Used

- **Partnership**: Fleet Management and Route Optimization evolve together with mutual coordination
- **Customer-Supplier**: Freight Brokerage depends on Fleet Management capacity data
- **Conformist**: Fleet Management accepts ELD provider data formats without translation
- **Anti-Corruption Layer**: Freight Brokerage protects its model from external load board data structures
- **Published Language**: Domain events use a shared event schema for inter-context communication
- **Open Host Service**: Fleet Management exposes a well-defined API for capacity queries

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) -- The context map documents relationships between bounded contexts
- [Strategic Design](../strategic-design/) -- Context mapping is a core strategic design activity
- [Anti-Corruption Layer](../anti-corruption-layer/) -- ACL pattern appears at multiple integration points
- [Integration Patterns](../integration-patterns/) -- Context map patterns guide integration design
- [Domain Events](../domain-events/) -- Events are the primary communication mechanism between contexts

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 14: Maintaining Model Integrity. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 3: Context Maps. Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 8: Context Mapping Patterns. O'Reilly, 2021.
- Vernon, Vaughn. "Domain-Driven Design Distilled." Chapter 4: Strategic Design with Context Mapping. Addison-Wesley, 2016.
- Brandolini, Alberto. "Introducing EventStorming." Leanpub, 2021.
