# Event-Driven Architecture in the Psychiatry Domain

## Overview

Event-driven architecture (EDA) is the communication pattern used for inter-context integration in the psychiatry domain. Rather than bounded contexts calling each other directly through synchronous requests, they communicate by publishing and subscribing to domain events. When a significant clinical occurrence happens in one context, it publishes an event that other interested contexts consume asynchronously. This approach maintains loose coupling between contexts while enabling coordinated clinical workflows across the system.

## Rationale for Event-Driven Communication

Psychiatry systems involve multiple clinical workflows that operate on different timelines and are managed by different teams. A diagnostic evaluation may take an hour, medication monitoring spans weeks, psychotherapy courses last months, and outcome measurement occurs at defined intervals. Direct synchronous coupling between these workflows would create fragile dependencies: if the outcomes measurement system were unavailable, it could block diagnostic evaluations from completing.

Event-driven architecture resolves this by decoupling the producer of clinical information from its consumers. The Diagnostic Assessment Context publishes a Diagnosis Assigned event without knowing or caring which other contexts will consume it. The Medication Management, Psychotherapy, and Outcomes Measurement contexts each subscribe independently and process the event on their own timelines.

## Event Flow Patterns

### Diagnostic-to-Treatment Flow

This is the primary clinical information flow in the psychiatry domain:

1. The Diagnostic Assessment Context publishes **Diagnosis Assigned** after a psychiatric evaluation is finalized.
2. The Medication Management Context receives the event and updates its view of the patient's indication profile, potentially triggering medication review workflows.
3. The Psychotherapy Context receives the event and evaluates whether the diagnosis suggests a modality change or new treatment protocol.
4. The Outcomes Measurement Context receives the event and configures the appropriate measurement battery for the assigned diagnosis.

Each consumer processes the event independently. If the psychotherapy context is slow to process, it does not delay the medication context.

### Crisis Notification Flow

Crisis events require broader notification:

1. The Crisis Management Context publishes **Crisis Activated** when a psychiatric emergency is identified.
2. The Psychotherapy Context receives the event and notifies the patient's treating therapist.
3. The Medication Management Context receives the event and flags the patient's medication record for crisis-related review.
4. After resolution, **Crisis Resolved** is published with disposition data.
5. The Psychotherapy Context schedules a follow-up session.
6. The Outcomes Measurement Context records the crisis event in the patient's outcome timeline.

### Medication Change Flow

Medication changes propagate to downstream contexts:

1. The Medication Management Context publishes **Medication Prescribed** or **Medication Discontinued**.
2. The Crisis Management Context updates its cached medication list for the patient (shared kernel refresh).
3. The Outcomes Measurement Context records the medication change as a treatment variable in outcome analysis.

### Outcome Feedback Flow

Outcome data flows back to inform clinical decisions:

1. The Outcomes Measurement Context publishes **Clinical Deterioration Detected** when a patient's scores indicate significant worsening.
2. The Diagnostic Assessment Context may schedule a diagnostic reassessment.
3. The Medication Management Context may trigger a medication regimen review.
4. The Psychotherapy Context may adjust the treatment plan or assess for alliance rupture.

## Event Design Principles

### Event Completeness

Events carry sufficient data for consumers to act without needing to call back to the producing context. A Diagnosis Assigned event includes the diagnosis code, specifiers, patient ID, assigning clinician, and timestamp. Consumers do not need to query the Diagnostic Assessment Context for additional information to perform their initial processing.

### Event Ordering

Events from the same aggregate are guaranteed to be processed in order. The diagnostic context ensures that Diagnosis Assigned and Diagnosis Revised events for the same patient are consumed sequentially by downstream contexts. Cross-aggregate ordering is not guaranteed and is handled through idempotent consumer design.

### Idempotent Consumers

All event consumers are designed to handle duplicate event delivery without producing incorrect results. If a Diagnosis Assigned event is delivered twice due to a network retry, the medication context's processing produces the same result as if it were delivered once.

### Event Versioning

Events include version information to support schema evolution. When a new field is added to the Medication Prescribed event (e.g., pharmacogenomic consultation flag), the version is incremented, and consumers that have not been updated to handle the new version continue to process the event using the previous schema with the new field ignored.

## Eventual Consistency

The psychiatry domain operates under eventual consistency between bounded contexts. When a diagnosis is assigned, the medication context's view of the patient's diagnostic profile is briefly stale until it processes the event. This delay (typically milliseconds to seconds) is clinically acceptable because prescribing decisions are not made in the same instant as diagnostic assignment.

The one exception is the Crisis Management shared kernel, which uses synchronous access to current diagnoses and medications because crisis situations require real-time information. This synchronous access is deliberately limited to the narrow shared kernel scope.

## Event Store Considerations

The psychiatry domain benefits from event sourcing for clinical history reconstruction. A patient's complete diagnostic history is best represented as a stream of diagnostic events (assigned, revised, resolved) rather than a mutable current-state record. This event stream preserves the clinical narrative and supports audit requirements. The Diagnostic Assessment and Medication Management contexts are strong candidates for event sourcing due to the clinical significance of their change histories.

## Failure Handling

When event processing fails (e.g., a consumer encounters invalid data), the event is routed to a dead letter queue for investigation. Failed events are not silently dropped because clinical data loss can have patient safety implications. A monitoring system alerts operations staff to dead letter queue accumulation, and clinical staff are notified when event processing delays exceed defined thresholds that could impact patient care.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 14 on bounded context integration.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapters 8 and 13 on domain events and integration.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 11 on event-driven architecture.
- Stopford, Ben. *Designing Event-Driven Systems*. O'Reilly Media, 2018.
