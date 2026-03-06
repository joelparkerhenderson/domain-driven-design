# Repositories in Logistics

## Overview

A repository is an abstraction that encapsulates the logic for storing, retrieving, and querying aggregate roots. Repositories present a collection-like interface to the domain model, hiding the details of the underlying persistence mechanism. In logistics, repositories provide access to vehicles, routes, shipments, customs declarations, dock appointments, and delivery routes without exposing database schemas or query languages to the domain layer.

## Relevance to Logistics

Logistics systems must persist and retrieve aggregates under high-throughput, real-time conditions. A route optimization engine queries routes by lane and time window. A fleet management system retrieves vehicles by availability and location. A yard management system looks up dock appointments by facility and time slot. Repositories abstract these persistence operations so that the domain model remains pure and infrastructure-independent, enabling the same domain logic to work with different storage technologies.

## Logistics Repositories by Bounded Context

### Fleet Management Context

- **VehicleRepository**: Stores and retrieves Vehicle aggregates. Supports queries by fleet ID, vehicle status (active, in-maintenance, decommissioned), location proximity, and equipment type.
- **DriverRepository**: Stores and retrieves Driver aggregates. Supports queries by home terminal, license class, available HOS hours, and certification type.

### Route Optimization Context

- **RouteRepository**: Stores and retrieves Route aggregates. Supports queries by origin-destination pair, departure time window, and vehicle assignment.
- **LoadPlanRepository**: Stores and retrieves LoadPlan aggregates. Supports queries by planning period, vehicle, and shipment inclusion.

### Freight Brokerage Context

- **LoadPostingRepository**: Stores and retrieves LoadPosting aggregates. Supports queries by lane, equipment type, pickup date, and posting status (open, tendered, accepted, completed).
- **CarrierRepository**: Stores and retrieves Carrier reference data. Supports queries by lane coverage, equipment type, safety rating, and contract status.
- **BrokerContractRepository**: Stores and retrieves BrokerContract aggregates. Supports queries by carrier, lane, and effective date range.

### Customs & Clearance Context

- **CustomsDeclarationRepository**: Stores and retrieves CustomsDeclaration aggregates. Supports queries by filing status, customs authority, and declaration date.
- **ClearanceRequestRepository**: Stores and retrieves ClearanceRequest aggregates. Supports queries by shipment reference, border crossing point, and clearance status.

### Yard Management Context

- **DockAppointmentRepository**: Stores and retrieves DockAppointment aggregates. Supports queries by facility, dock door, date, time slot, and appointment status.
- **TrailerPositionRepository**: Stores and retrieves TrailerPosition aggregates. Supports queries by facility, yard zone, trailer number, and occupancy status.

### Last-Mile Delivery Context

- **DeliveryRouteRepository**: Stores and retrieves DeliveryRoute aggregates. Supports queries by driver, delivery date, route status, and delivery area.
- **ProofOfDeliveryRepository**: Stores and retrieves ProofOfDelivery aggregates. Supports queries by delivery stop, date range, and POD type (signature, photo).

## Repository Design Principles

- Repositories operate on aggregate roots only; child entities within an aggregate are accessed through the root
- Repository interfaces are defined in the domain layer; implementations live in the infrastructure layer
- Repositories should not expose raw query capabilities; they provide named finder methods that reflect domain use cases
- Each aggregate root has exactly one repository
- Repositories handle optimistic concurrency to prevent lost updates on high-contention aggregates
- Repositories may support both synchronous retrieval and event-sourced reconstruction

## Relationships to Other Topics

- [Aggregates](../aggregates/) -- Repositories persist and retrieve aggregate roots
- [Entities](../entities/) -- Entities within aggregates are accessed through the aggregate root's repository
- [Domain Services](../domain-services/) -- Domain services use repositories to load aggregates for cross-aggregate operations
- [Domain Events](../domain-events/) -- Event-sourced repositories reconstruct aggregates from stored event streams
- [Bounded Contexts](../bounded-contexts/) -- Each bounded context has its own set of repositories with separate storage

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 6: The Life Cycle of a Domain Object. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 12: Repositories. Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 5: Implementing Business Logic. O'Reilly, 2021.
- Fowler, Martin. "Patterns of Enterprise Application Architecture." Chapter 10: Data Source Architectural Patterns. Addison-Wesley, 2002.
