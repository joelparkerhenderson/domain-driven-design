# Entities in Logistics

## Overview

An entity is a domain object defined by its unique identity rather than its attributes. Two entities with the same attributes but different identities are considered different objects. In logistics, entities represent the core objects that persist and change over time -- vehicles, drivers, shipments, routes, and trailers -- each tracked by a unique identifier throughout its lifecycle.

## Relevance to Logistics

Logistics is an identity-driven domain. A truck (VIN 1HGCM82633A004352) is a specific asset tracked across maintenance events, route assignments, and compliance inspections. A driver (CDL #D12345678) accumulates hours-of-service records, certifications, and performance history. A shipment (BOL #SH-2024-00987) is tracked from tender acceptance through customs clearance to final delivery. Identity continuity is essential for regulatory compliance, audit trails, and operational tracking.

## Key Entities by Bounded Context

### Fleet Management Context

- **Vehicle**: Identified by VIN or fleet number. Attributes include make, model, year, axle configuration, fuel type, and gross vehicle weight rating. Changes over time as maintenance is performed, inspections are recorded, and assignments shift.
- **Driver**: Identified by CDL number or employee ID. Attributes include license class, endorsements, medical certificate expiration, and home terminal. Accumulates HOS records and safety records over time.
- **MaintenanceRecord**: Identified by record ID. Links a specific vehicle to a maintenance event with date, type (preventive, corrective), parts used, and technician.

### Route Optimization Context

- **Route**: Identified by route ID. Composed of ordered route segments with origin, destination, and timing. Evolves as segments are added, reordered, or rerouted.
- **RouteSegment**: Identified within a route. Represents a single leg between two waypoints with distance, estimated duration, and assigned vehicle.

### Freight Brokerage Context

- **Carrier**: Identified by MC number (Motor Carrier number) or DOT number. Attributes include operating authority, insurance coverage, safety rating, and lane preferences.
- **LoadPosting**: Identified by load ID. Represents a freight opportunity with origin, destination, commodity, weight, and equipment type.

### Customs & Clearance Context

- **CustomsDeclaration**: Identified by declaration number. Represents a formal filing with customs authorities including line items, declared values, and HS classifications.
- **TradePartner**: Identified by importer/exporter ID. Represents a business entity with trade compliance history and known shipper status.

### Yard Management Context

- **DockDoor**: Identified by door number within a facility. A physical loading bay with type (inbound, outbound, both) and equipment compatibility.
- **Trailer**: Identified by trailer number or license plate within the yard context. Tracks current position, seal status, and contents.

### Last-Mile Delivery Context

- **DeliveryStop**: Identified by stop ID within a delivery route. Represents a single delivery destination with address, time window, and package count.
- **DeliveryAttempt**: Identified by attempt ID. Records the outcome of a delivery attempt including timestamp, result (delivered, failed, refused), and evidence collected.

## Entity Design Principles

- Every entity has a stable, unique identity that does not change over its lifetime
- Equality is based on identity, not on attribute values
- Entities are mutable: their attributes change over time as the domain evolves
- Entity identity may be natural (VIN, CDL number, MC number) or surrogate (generated UUID)
- Entities track lifecycle state transitions (e.g., Vehicle: Active -> InMaintenance -> Active -> Decommissioned)

## Relationships to Other Topics

- [Aggregates](../aggregates/) -- Entities participate in aggregates; some entities serve as aggregate roots
- [Value Objects](../value-objects/) -- Entities contain value objects as attributes (e.g., Vehicle contains GPSCoordinate)
- [Repositories](../repositories/) -- Entities within aggregates are persisted and retrieved through repositories
- [Domain Events](../domain-events/) -- Entity state changes produce domain events
- [Ubiquitous Language](../ubiquitous-language/) -- Entity names come directly from the ubiquitous language

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 5: A Model Expressed in Software. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 5: Entities. Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 5: Implementing Business Logic. O'Reilly, 2021.
- Millett, Scott & Tune, Nick. "Patterns, Principles, and Practices of Domain-Driven Design." Chapter 14: Entities. Wrox, 2015.
