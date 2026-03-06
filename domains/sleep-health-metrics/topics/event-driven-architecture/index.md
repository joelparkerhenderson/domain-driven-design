# Event-Driven Architecture - Sleep Health Metrics

## Overview

Event-driven architecture (EDA) enables asynchronous communication between bounded contexts in the sleep health metrics domain. Rather than bounded contexts calling each other directly through synchronous interfaces, they communicate by publishing and consuming domain events. This decoupling allows each context to evolve independently, scale according to its own workload characteristics, and maintain its bounded context integrity.

## Rationale for Event-Driven Communication

Sleep health data flows through a natural pipeline: collection, scoring, assessment, circadian analysis, intervention evaluation, and clinical reporting. However, these stages do not all execute synchronously. A polysomnography recording may be collected overnight and scored the next day. Quality metrics may be computed immediately after scoring or deferred for batch processing. CPAP adherence data may arrive daily while clinical reports are generated weekly. Event-driven architecture accommodates these temporal variations without creating blocking dependencies between contexts.

## Event Flow Topology

The primary event flow follows the clinical workflow:

1. **Sleep Data Collection Context** publishes SleepStudyRecorded, SleepDiarySubmitted, and WearableDataSynced events.
2. **Sleep Stage Analysis Context** subscribes to SleepStudyRecorded and publishes ScoringSessionCompleted, RespiratoryEventsScored, and ArousalIndexComputed events upon completion.
3. **Sleep Quality Assessment Context** subscribes to ScoringSessionCompleted and publishes QualityMetricsCalculated, SubjectiveAssessmentScored, and SleepQualityTrendDetected events.
4. **Circadian Rhythm Context** publishes CircadianPhaseAssessed and CircadianMisalignmentDetected events, which are consumed by both the Sleep Quality Assessment and Intervention Tracking Contexts.
5. **Intervention Tracking Context** subscribes to QualityMetricsCalculated and CircadianPhaseAssessed events and publishes InterventionStarted, AdherenceReported, and InterventionOutcomeAssessed events.
6. **Clinical Integration Context** subscribes to events from all other contexts and publishes ClinicalReportGenerated and FHIRResourcePublished events.

## Event Design

Each domain event in the sleep health metrics domain adheres to the following structure:

- **Event ID**: A globally unique identifier for the event instance.
- **Event Type**: A descriptive name in past tense (e.g., ScoringSessionCompleted).
- **Timestamp**: The time at which the event occurred in the domain.
- **Aggregate ID**: The identity of the aggregate that produced the event.
- **Aggregate Type**: The type of aggregate (e.g., ScoringSession, QualityAssessment).
- **Payload**: The domain-relevant data carried by the event, sufficient for consumers to act without querying the producer.
- **Correlation ID**: An identifier linking related events across the workflow (e.g., a StudyId that connects recording, scoring, and assessment events).

## Event Delivery Guarantees

The architecture employs at-least-once delivery semantics. Consumers must be idempotent -- processing the same event multiple times must produce the same result as processing it once. This is critical in clinical contexts where duplicate quality metric calculations or duplicate report generations would create data integrity issues.

Event ordering is guaranteed within a single aggregate (events from the same ScoringSession are delivered in order) but not guaranteed across aggregates. Consumers that require ordering across multiple aggregates implement ordering logic based on correlation IDs and timestamps.

## Event Sourcing Considerations

Certain aggregates in the sleep health metrics domain benefit from event sourcing, where state is reconstructed from a sequence of domain events rather than stored as a snapshot. The ScoringSession aggregate is a candidate: the sequence of epoch scoring decisions, corrections, and finalization forms a natural audit trail that is clinically valuable for inter-scorer reliability assessment and quality assurance review.

Event sourcing also supports temporal queries: "What was the scored architecture at the time the interim report was generated?" This capability is relevant in clinical settings where scoring may be revised after initial interpretation.

## Saga Patterns

Cross-context workflows that require coordination use the saga pattern. For example, the "Sleep Study Complete Workflow" saga coordinates:

1. SleepStudyRecorded triggers scoring initiation.
2. ScoringSessionCompleted triggers quality metric computation.
3. QualityMetricsCalculated triggers clinical report generation.

If any step fails (e.g., scoring fails due to insufficient signal quality), the saga publishes a compensating event (ScoringFailed) that the upstream context can use to flag the study for technician review rather than leaving it in a partially processed state.

## Temporal Patterns

Sleep health data has inherent temporal characteristics that influence event architecture:

- **Nightly batch events**: CPAP adherence data arrives as a daily summary.
- **Real-time streaming events**: Wearable sensor data may arrive continuously.
- **Periodic assessment events**: PSQI and ESS assessments occur monthly or at clinical intervals.
- **On-demand events**: Clinical report generation is triggered by clinician request.

The event architecture handles all these temporal patterns through a combination of event scheduling, windowed aggregation, and on-demand event publication.

## Error Handling

Failed event processing is handled through dead-letter queues with clinical escalation. A failed QualityMetricsCalculated event triggers review by the quality assurance team. A failed FHIRResourcePublished event triggers retry with exponential backoff, followed by manual intervention notification if retries are exhausted.

## References

- Evans, E. (2003). *Domain-Driven Design*. Addison-Wesley.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley. Chapter 8: Domain Events.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media. Chapter 13: Event-Driven Architecture.
- Richardson, C. (2018). *Microservices Patterns*. Manning Publications. Chapters on event-driven architecture and sagas.
