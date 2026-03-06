# Domain Events in Logistics

## Overview

A domain event represents a significant occurrence within the domain that other parts of the system may need to react to. Domain events are named in the past tense using the ubiquitous language (e.g., VehicleDispatched, RouteDeviated, CustomsCleared) and carry the data needed by consumers to act on the event. In logistics, domain events are the primary mechanism for communication between bounded contexts.

## Relevance to Logistics

Logistics operations are inherently event-driven. A vehicle departs a yard, triggering route tracking. A load is tendered to a carrier, triggering rate confirmation. A customs declaration is approved, triggering shipment release. A delivery is completed, triggering proof-of-delivery capture. Domain events model these real-world occurrences as first-class objects, enabling loose coupling between contexts and supporting the real-time coordination that logistics operations demand.

## Domain Events by Bounded Context

### Fleet Management Context (Produces)

- **VehicleRegistered**: A new vehicle has been added to the fleet with its specifications and regulatory documentation.
- **VehicleDispatched**: A vehicle has been assigned to a route and authorized for departure.
- **VehicleReturnedToYard**: A vehicle has arrived back at its home terminal or assigned facility.
- **MaintenanceScheduled**: A preventive or corrective maintenance event has been scheduled for a vehicle.
- **VehiclePlacedOutOfService**: A vehicle has been taken out of active service due to a mechanical issue or failed inspection.
- **DriverHOSWarning**: A driver is approaching hours-of-service limits and must plan a mandatory rest break.

### Route Optimization Context (Produces)

- **RoutePlanned**: A new route has been created with origin, destination, stops, and timing.
- **RouteOptimized**: An existing route has been recalculated to improve efficiency based on new constraints.
- **RouteDeviated**: A vehicle has deviated from its planned route, triggering rerouting analysis.
- **ETAUpdated**: The estimated time of arrival at a stop or destination has been recalculated.
- **LoadConsolidated**: Multiple shipments have been consolidated into a single vehicle load.

### Freight Brokerage Context (Produces)

- **LoadPosted**: A new freight load has been posted to the load board for carrier bidding.
- **LoadTendered**: A load has been formally offered to a specific carrier at an agreed rate.
- **TenderAccepted**: A carrier has accepted a load tender.
- **TenderRejected**: A carrier has declined a load tender.
- **RateNegotiated**: A freight rate has been agreed upon between broker and carrier for a load.
- **CarrierInvoiceReceived**: A carrier has submitted an invoice for a completed load.

### Customs & Clearance Context (Produces)

- **DeclarationFiled**: A customs declaration has been submitted to the relevant authority.
- **CustomsCleared**: A shipment has received clearance to cross the border.
- **CustomsHeld**: A shipment has been flagged for inspection or additional documentation.
- **DutyAssessed**: Import duties and tariffs have been calculated for a shipment.
- **ComplianceViolationDetected**: A trade compliance issue has been identified requiring resolution.

### Yard Management Context (Produces)

- **TrailerCheckedIn**: A trailer has entered the yard and been assigned a position through the gate.
- **TrailerCheckedOut**: A trailer has departed the yard through the gate.
- **DockAppointmentScheduled**: A dock door time slot has been reserved for a carrier.
- **TrailerDockedAtDoor**: A trailer has been moved to a dock door for loading or unloading.
- **LoadingCompleted**: A trailer has been fully loaded and is ready for dispatch.
- **UnloadingCompleted**: A trailer has been fully unloaded and is available for repositioning.

### Last-Mile Delivery Context (Produces)

- **DeliveryRouteDispatched**: A delivery route has been assigned to a driver and released for execution.
- **DeliveryAttempted**: A delivery attempt has been made at a stop (successful or failed).
- **DeliveryCompleted**: A delivery has been successfully completed with proof of delivery captured.
- **DeliveryFailed**: A delivery could not be completed due to absence, refusal, or access issues.
- **CustomerNotified**: A notification has been sent to the recipient with delivery status or ETA.

## Event Design Principles

- Events are immutable facts about something that already happened
- Event names use past tense in the ubiquitous language of the publishing context
- Events carry sufficient data for consumers to act without querying back to the publisher
- Events do not contain the entire aggregate state; they contain only the relevant change data
- Events are ordered within an aggregate but may arrive out of order across aggregates

## Relationships to Other Topics

- [Aggregates](../aggregates/) -- Aggregates publish domain events when state changes
- [Event-Driven Architecture](../event-driven-architecture/) -- Domain events are the building blocks of event-driven architecture
- [Bounded Contexts](../bounded-contexts/) -- Events flow between bounded contexts for integration
- [Integration Patterns](../integration-patterns/) -- Events enable asynchronous integration between contexts
- [Repositories](../repositories/) -- Event-sourced repositories store aggregates as sequences of events

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 8: Domain Events. Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 7: Modeling the Dimension of Time. O'Reilly, 2021.
- Brandolini, Alberto. "Introducing EventStorming." Leanpub, 2021.
- Young, Greg. "Versioning in an Event Sourced System." Leanpub, 2016.
