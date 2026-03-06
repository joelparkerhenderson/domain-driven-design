# Event-Driven Architecture in the Sleep Quality Domain

## Overview

Event-driven architecture (EDA) is the primary communication mechanism between bounded contexts in the sleep quality domain. Rather than bounded contexts calling each other directly through synchronous APIs, they communicate by publishing and subscribing to domain events. This architectural style provides loose coupling, temporal decoupling, and independent scalability. Each bounded context can evolve, deploy, and scale independently while maintaining coherent system-wide behavior through events.

## Motivation

The sleep quality domain requires event-driven architecture for several reasons. Sleep data flows asynchronously: device data arrives on variable schedules, diary entries are submitted at the patient's convenience, and clinical assessments may take days to complete. Treatment adjustments based on accumulated data do not require real-time synchronous coordination. Outcomes tracking aggregates data from multiple sources over extended periods. An event-driven approach naturally accommodates these asynchronous data flows.

## Event Flow Patterns

### Assessment to Diagnosis Flow

When the Sleep Assessment Context completes an assessment, it publishes an AssessmentCompleted event. The Sleep Disorders Context subscribes to this event and evaluates the assessment data against diagnostic criteria. If a diagnosis is confirmed or severity changes, the Disorders Context publishes its own events (DisorderDiagnosed, DisorderSeverityChanged). This chain of events flows from assessment through diagnosis without any direct coupling between the two contexts.

### Assessment to Treatment Flow

When the Sleep Assessment Context records sleep diary entries, it publishes SleepDiaryEntryRecorded events. The Behavioral Interventions Context subscribes to these events and uses the diary data to evaluate whether sleep restriction parameters need adjustment. If an adjustment is warranted, the Interventions Context publishes a SleepRestrictionWindowAdjusted event. This flow enables the treatment context to react to assessment data without polling or direct querying.

### Device to Multiple Consumer Flow

When the Device Integration Context receives and normalizes device data, it publishes a DeviceDataReceived event with a data type indicator. Multiple downstream contexts subscribe to this event and filter by data type: the Sleep Assessment Context processes actigraphy-type data, the Sleep Environment Context processes environmental sensor data, and the Outcomes Tracking Context processes all data types for longitudinal tracking. This fan-out pattern enables a single device data event to serve multiple consumers.

### Intervention to Outcomes Flow

Treatment progress events (InterventionProtocolStarted, SessionCompleted, SleepRestrictionWindowAdjusted) flow from the Behavioral Interventions Context to the Outcomes Tracking Context. The Outcomes Context correlates these treatment events with changes in sleep quality metrics, enabling analysis of intervention effectiveness.

## Event Design

Events in the sleep quality domain follow consistent design principles. Events are named in past tense, indicating that something has already happened (AssessmentCompleted, not CompleteAssessment). Events are immutable and represent facts that cannot be changed after publication. Events carry sufficient data for consumers to act without needing to query the publisher (fat events). Events include metadata: a unique event ID, the event type, the timestamp of occurrence, the producing bounded context, and a correlation ID for tracing related events across contexts.

## Eventual Consistency

Event-driven architecture introduces eventual consistency between bounded contexts. When an assessment is completed, the outcomes record is not updated instantaneously; it is updated when the Outcomes Tracking Context processes the AssessmentCompleted event. The sleep quality domain tolerates this eventual consistency because clinical workflows do not require real-time synchronization between contexts. Clinicians understand that outcome reports reflect data as of the last processing cycle, not necessarily the current instant.

## Event Ordering

Within a single bounded context, events for a given aggregate are produced in order. Across bounded contexts, events may arrive out of order due to processing delays or retry mechanisms. The sleep quality domain handles out-of-order events by including timestamps that allow consumers to detect and handle ordering issues. The Outcomes Tracking Context, which aggregates events from multiple sources, uses event timestamps rather than arrival order to maintain chronological accuracy.

## Error Handling and Retry

When a consumer fails to process an event (due to validation errors, temporary infrastructure issues, or domain rule violations), the event is routed to a retry queue. After configurable retry attempts, unprocessable events are moved to a dead letter queue for manual investigation. The producing context is not affected by consumer failures, maintaining the loose coupling principle.

## Event Versioning

As the domain model evolves, event schemas may change. The sleep quality domain uses event versioning with backward-compatible schema evolution where possible (adding optional fields) and explicit version migration when breaking changes are necessary. Consumers maintain handlers for current and recent event versions to support rolling deployments.

## Idempotency

All event consumers in the sleep quality domain are designed to be idempotent. Processing the same event twice produces the same result as processing it once. This property is essential for reliable event processing, as retry mechanisms may deliver events more than once. Consumers use event IDs to detect and skip duplicate deliveries.

## Monitoring and Observability

The event-driven architecture includes monitoring for event publication rates, consumer processing latency, error rates, dead letter queue depth, and end-to-end event flow latency. These metrics enable operational teams to detect and diagnose issues in cross-context communication.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley. Chapter 8 on domain events and Chapter 13 on integration.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media. Chapter 11 on event-driven architecture.
- Stopford, B. (2018). Designing Event-Driven Systems. O'Reilly Media.
