# Event-Driven Architecture for Mast Cell Activation Syndrome

## Overview

Event-driven architecture (EDA) is the communication backbone of the MCAS domain. It enables bounded contexts to communicate through domain events rather than direct method calls or shared databases. Each context publishes events when significant domain occurrences happen and subscribes to events from other contexts that are relevant to its operations. This architectural style maintains loose coupling between contexts while enabling system-wide coordination.

For the MCAS domain, event-driven architecture is particularly valuable because the clinical workflow involves multiple parallel processes that must remain coordinated. A new symptom entry may simultaneously need to be evaluated for trigger correlation, assessed against medication effectiveness, and incorporated into outcomes measurement, all without the Symptom Tracking Context needing to know about these downstream consumers.

## Event Flow Patterns

### Diagnostic Cascade

When a diagnosis is confirmed, the DiagnosisConfirmed event triggers a cascade of actions across the system. The Trigger Management Context creates an initial trigger profile. The Medication Protocol Context initiates a baseline treatment regimen. The Specialist Coordination Context begins assembling a care team. The Outcomes Measurement Context establishes baseline measurements. Each context reacts independently to the same event, performing its own domain-specific actions.

### Symptom Data Distribution

The Symptom Tracking Context publishes SymptomRecorded and FlareDocumented events at high frequency. These events fan out to multiple consumers. The Trigger Management Context evaluates symptoms for trigger correlations. The Medication Protocol Context assesses treatment response. The Outcomes Measurement Context updates burden scores. Each consumer processes the event according to its own business rules without affecting the publishing context.

### Medication Change Propagation

When the Medication Protocol Context publishes MedicationStarted or MedicationTitrated events, the Outcomes Measurement Context begins an observation window for effectiveness assessment, and the Specialist Coordination Context updates the shared care plan. The Symptom Tracking Context may prompt enhanced monitoring during the initial period after a medication change.

### Care Plan Synchronization

The Specialist Coordination Context publishes CarePlanUpdated events whenever the shared care plan is modified. All clinical contexts receive this event and can adjust their operations to align with the updated plan. This ensures that trigger avoidance strategies, medication protocols, and monitoring schedules remain consistent with the coordinated management approach.

## Event Design

### Event Structure

All MCAS domain events follow a consistent structure. Each event includes a unique event identifier, the event type name, a timestamp, the publishing context identifier, and the event payload. The payload contains sufficient data for consumers to act without needing to query back to the source context.

### Event Immutability

Domain events are immutable records of facts that occurred. Once published, an event cannot be modified or deleted. If the publishing context needs to correct information, it publishes a new corrective event rather than modifying the original. This immutability ensures a reliable audit trail and supports event sourcing patterns.

### Event Versioning

Event schemas are versioned to support evolution without breaking existing consumers. When a new version of an event schema is introduced, backward compatibility is maintained. Consumers that have not upgraded to the new schema can continue processing events using the older format. Version numbers are included in event metadata.

### Event Ordering

Within a single bounded context, events are published in the order they occur. Across contexts, strict global ordering is not guaranteed. Consumers must handle out-of-order delivery gracefully. The MCAS domain addresses this through idempotent event handlers and timestamp-based conflict resolution.

## Eventual Consistency

Event-driven architecture introduces eventual consistency between bounded contexts. When a symptom is recorded in the Symptom Tracking Context, the corresponding updates in the Trigger Management and Outcomes Measurement contexts do not occur instantaneously. There is a brief period during which different contexts may have different views of the system state.

For the MCAS domain, eventual consistency is acceptable because clinical workflows operate on timescales of minutes to hours, not milliseconds. The brief delay between event publication and consumer processing does not impact clinical decision-making. The system design ensures that each context reaches a consistent state within its own boundaries at all times.

## Event Handling Patterns

### Idempotent Handlers

All event handlers in the MCAS domain are idempotent. Processing the same event multiple times produces the same result as processing it once. This is critical because message delivery infrastructure may deliver events more than once. Idempotency is achieved through deduplication checks using event identifiers.

### Compensating Events

When a downstream context needs to undo an action triggered by a previous event, it publishes a compensating event. For example, if a DiagnosisConfirmed event is later followed by a DiagnosisRevised event, downstream contexts that created resources based on the original diagnosis publish their own compensating events to roll back or adjust.

### Dead Letter Handling

Events that cannot be processed by a consumer are routed to a dead letter queue for investigation. The MCAS domain monitors dead letter queues to identify integration issues between contexts. Unprocessable events may indicate schema mismatches, missing reference data, or business rule violations.

## Event Store

The MCAS domain maintains an event store that records all published domain events. The event store serves multiple purposes: it provides an audit trail of all domain activity, supports event replay for debugging and recovery, enables new consumers to catch up on historical events, and supports analytical queries across event streams.

## Monitoring and Observability

Event-driven architecture requires monitoring to ensure reliable event delivery and processing. The MCAS domain tracks event publication rates, consumer lag, processing errors, and dead letter queue depth. Alerting thresholds are configured for critical event types such as DiagnosisConfirmed and MedicationStarted, where processing delays could impact clinical operations.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 8 on domain events.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapters 8 and 13 on events and integration.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 11 on event-driven architecture.
- Stopford, B. Designing Event-Driven Systems. O'Reilly Media, 2018.
