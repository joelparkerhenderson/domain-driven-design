# Aggregates in Logistics

## Overview

An aggregate is a cluster of related domain objects treated as a single unit for consistency and transactional purposes. Each aggregate has a root entity that serves as the entry point for all interactions. In logistics, aggregates enforce business invariants within bounded contexts -- ensuring that a route's segments are consistent, that a vehicle's maintenance schedule is valid, or that a customs declaration's line items sum correctly.

## Relevance to Logistics

Logistics operations involve complex, interconnected data that must remain consistent under concurrent modification. A route must always have at least one segment. A dock appointment must not double-book a dock door. A freight quote must reflect the actual load characteristics. Aggregates define the consistency boundaries that prevent invalid states, while keeping transactional scope small enough for high-throughput operations.

## Logistics Aggregate Roots

### Fleet Management Context

- **Vehicle**: Root entity owning maintenance records, inspection history, telematics configuration, and current assignment status. Ensures a vehicle cannot be dispatched while in maintenance.
- **Driver**: Root entity owning CDL credentials, HOS records, certifications, and current assignment. Ensures a driver cannot be assigned when HOS limits are exceeded.

### Route Optimization Context

- **Route**: Root entity owning an ordered collection of route segments, stop assignments, and timing constraints. Ensures all segments form a contiguous path and total load does not exceed vehicle capacity.
- **LoadPlan**: Root entity owning the assignment of shipments to vehicles for a planning period. Ensures no shipment is double-assigned and vehicle capacity constraints are respected.

### Freight Brokerage Context

- **LoadPosting**: Root entity owning load details, carrier bids, and acceptance status. Ensures only one carrier can be accepted per load.
- **BrokerContract**: Root entity owning lane commitments, rate agreements, and volume obligations between a broker and carrier. Ensures rate terms are internally consistent.

### Customs & Clearance Context

- **CustomsDeclaration**: Root entity owning line items with HS codes, declared values, and country of origin. Ensures all line items are classified and total declared value matches line item sum.
- **ClearanceRequest**: Root entity owning the filing status, authority responses, and required documentation for a specific border crossing.

### Yard Management Context

- **DockAppointment**: Root entity owning the scheduled time slot, dock door assignment, carrier information, and loading/unloading status. Ensures no time slot conflicts for a given dock door.
- **TrailerPosition**: Root entity owning the current yard location, seal status, and contents summary of a trailer.

### Last-Mile Delivery Context

- **DeliveryRoute**: Root entity owning an ordered sequence of delivery stops, driver assignment, and route status. Ensures stop sequence is valid and all stops belong to the assigned delivery area.
- **ProofOfDelivery**: Root entity owning signature data, photos, timestamps, and recipient information for a completed delivery.

## Aggregate Design Rules

- Keep aggregates small: include only the data needed to enforce invariants within the consistency boundary
- Reference other aggregates by identity (ID), not by direct object reference
- One transaction modifies one aggregate; cross-aggregate consistency is achieved through domain events
- The aggregate root is the sole entry point for modifications to any entity within the aggregate
- Design aggregate boundaries based on business invariants, not on data access convenience

## Relationships to Other Topics

- [Entities](../entities/) -- Aggregate roots are entities; aggregates may contain additional entities
- [Value Objects](../value-objects/) -- Aggregates contain value objects that describe attributes without identity
- [Repositories](../repositories/) -- Each aggregate root has a corresponding repository for persistence
- [Domain Events](../domain-events/) -- Aggregates publish events to communicate changes to other aggregates and contexts
- [Domain Services](../domain-services/) -- Operations spanning multiple aggregates are modeled as domain services
- [Bounded Contexts](../bounded-contexts/) -- Each bounded context contains its own set of aggregates

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 6: The Life Cycle of a Domain Object. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 10: Aggregates. Addison-Wesley, 2013.
- Vernon, Vaughn. "Effective Aggregate Design." Three-part series. DDD Community, 2011.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 5: Implementing Business Logic. O'Reilly, 2021.
