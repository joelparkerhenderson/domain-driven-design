# Event-Driven Architecture: Autism Domain

Event-driven architecture enables asynchronous, loosely coupled communication between bounded contexts in the autism spectrum management domain through the publication and consumption of domain events.

## Purpose

The autism domain's six bounded contexts must coordinate care without creating tight coupling between their internal models. Event-driven architecture provides this coordination by allowing contexts to publish significant occurrences as domain events, which other contexts consume and react to independently. This approach enables each bounded context to evolve autonomously while maintaining the cross-context coordination essential for integrated care delivery.

## Architecture Overview

The event-driven architecture in the autism domain follows a publish-subscribe pattern. Each bounded context publishes events when significant state changes occur within its domain model. Other bounded contexts subscribe to the events they need and react accordingly. The publishing context does not know or depend on its consumers, and consuming contexts do not depend on the internal model of the publishing context.

## Event Flow Patterns

### Diagnostic-Triggered Cascade

The most significant event cascade begins when the Diagnostic Assessment Context publishes a DiagnosisConfirmed event. This single event triggers parallel reactions across multiple downstream contexts:

- Behavioral Intervention Context: Initiates a new intake process and schedules an initial functional behavior assessment.
- Sensory Processing Context: Schedules a comprehensive sensory profile assessment.
- Communication Support Context: Schedules a communication evaluation and AAC candidacy assessment.
- Educational Planning Context: Initiates the special education eligibility determination process.
- Outcomes Tracking Context: Establishes baseline measurement procedures.

Each downstream context reacts independently based on its own business rules, without requiring coordination from the Diagnostic Assessment Context.

### Intervention-to-Outcome Flow

As intervention contexts (Behavioral, Sensory, Communication) complete assessments, create plans, and record progress, they publish events that flow to the Outcomes Tracking Context:

- SkillMasteryAchieved events trigger milestone recording and program completion tracking.
- SensoryProfileCompleted events establish sensory baselines for longitudinal comparison.
- CommunicationGoalMet events record communication development milestones.

The Outcomes Tracking Context aggregates these events over time to build comprehensive outcome profiles without querying the source contexts.

### Cross-Intervention Coordination

Some events trigger coordination between intervention contexts:

- When BehaviorIncidentRecorded includes a sensory antecedent, the Sensory Processing Context evaluates whether the individual's sensory diet or environmental modifications need adjustment.
- When SensoryProfileCompleted reveals new sensory concerns, the Behavioral Intervention Context reviews whether current behavior plans adequately address sensory antecedents.
- When CommunicationAssessmentCompleted identifies new communication capabilities, the Behavioral Intervention Context evaluates whether functional communication training targets should be updated.

### Educational Integration Flow

The Educational Planning Context consumes events from all intervention contexts to maintain alignment between IEP goals and clinical intervention goals:

- InterventionPlanCreated and InterventionPlanModified events trigger review of IEP behavioral goals.
- SensoryDietUpdated events trigger review of classroom sensory accommodations.
- AACDeviceAssigned events trigger arrangements for device availability at school.

## Event Design Principles

### Event Naming

Events are named in past tense to indicate that something has already happened. The name clearly identifies the bounded context of origin and the nature of the occurrence (e.g., DiagnosisConfirmed, not ConfirmDiagnosis).

### Event Payload

Each event carries sufficient data for consumers to react without querying the source context. This reduces coupling and prevents the need for synchronous callbacks. However, events do not carry the entire source aggregate -- they carry only the data relevant to the occurrence and likely consumer needs.

### Event Ordering

Within a single bounded context, events for the same aggregate are ordered by occurrence. Across bounded contexts, consumers handle events idempotently and do not depend on strict cross-context ordering.

### Event Versioning

As the domain model evolves, event schemas may change. Events include a version identifier, and consumers support both current and previous versions during migration periods to avoid breaking downstream contexts.

### Eventual Consistency

Event-driven architecture introduces eventual consistency between bounded contexts. After a DiagnosisConfirmed event is published, the Behavioral Intervention Context does not immediately reflect the new diagnosis. Systems are designed to tolerate this temporary inconsistency and to handle event processing failures with retry and dead-letter mechanisms.

## Error Handling

- Failed event processing: Events that fail processing are retried with exponential backoff before being routed to a dead-letter queue for manual review.
- Idempotent consumers: All event consumers are idempotent, producing the same result regardless of how many times the same event is processed.
- Compensating events: When a previously published event must be corrected (e.g., a diagnostic determination is revised), a compensating event is published that downstream contexts process to update their state.

## Monitoring

Event flow monitoring tracks event publication rates, processing latency, failure rates, and dead-letter queue depth. Alerts are configured for abnormal patterns such as event processing backlogs or elevated failure rates that could indicate integration issues between bounded contexts.

## References

- Evans, E. (2003). *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley. Chapter 14 on maintaining model integrity through events.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley. Chapter 8 on domain events and event-driven architecture.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media. Chapter 10 on event-driven integration between bounded contexts.
- Hohpe, G., & Woolf, B. (2003). *Enterprise Integration Patterns*. Addison-Wesley.
