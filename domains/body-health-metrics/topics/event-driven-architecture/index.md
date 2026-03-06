# Event-Driven Architecture for Body Health Metrics

## Overview

Event-driven architecture (EDA) in the body health metrics domain provides the communication backbone between bounded contexts. Rather than contexts directly calling each other through synchronous interfaces, they communicate through domain events -- immutable records of significant occurrences that are published by one context and consumed by others. This architectural style enables loose coupling between contexts, supports temporal decoupling, and naturally models the asynchronous nature of body health measurement workflows.

## Architectural Motivation

Body health measurement workflows are inherently asynchronous and event-driven. When a person steps on a scale, a chain of downstream activities unfolds: body composition is recalculated, fitness baselines are updated, health scores are recomputed, progress toward goals is evaluated, and clinical reports may need regeneration. These downstream activities do not need to happen synchronously or atomically with the weight measurement. Event-driven architecture aligns the technical architecture with this natural domain workflow.

## Event Flow Patterns

### Measurement Capture Flow

1. Biometric Collection Context records a weight measurement
2. BiometricMeasurementRecorded event is published
3. Body Composition Context consumes the event and triggers composition recalculation
4. CompositionProfileUpdated event is published
5. Health Scoring Context consumes both measurement and composition events
6. HealthScoreCalculated event is published
7. Progress Tracking Context consumes measurement, composition, and scoring events to update goals, trends, and streaks

### Threshold Alert Flow

1. Health Scoring Context detects a score crossing a clinical threshold
2. HealthThresholdCrossed event is published
3. Clinical Integration Context evaluates whether a clinical notification is warranted
4. If warranted, a clinical report is generated and ClinicalReportSubmitted event is published
5. Progress Tracking Context evaluates whether the threshold crossing affects any active goals

### Goal Achievement Flow

1. Progress Tracking Context detects that a measurement meets a goal target
2. GoalCompleted event is published
3. Goal recommendation service evaluates whether a new goal should be suggested
4. Notification systems deliver achievement recognition

## Event Categories

### Command Events

Events that request an action from a consuming context:

- RecalculateCompositionRequested: triggers composition analysis with new measurement data
- GenerateClinicalReportRequested: triggers clinical report creation for a set of measurements

### Notification Events

Events that inform consuming contexts of state changes:

- BiometricMeasurementRecorded: notifies that new measurement data is available
- HealthScoreCalculated: notifies that a new composite score has been computed
- MilestoneAchieved: notifies that a progress milestone has been reached

### Integration Events

Events that cross system boundaries:

- ClinicalReportSubmitted: notifies that data has been sent to an external clinical system
- ExternalMeasurementImported: notifies that data has been received from an external system

## Event Design Standards

**Event schema**: Each event carries a standard envelope (event ID, event type, timestamp, source context, correlation ID) plus a domain-specific payload. The payload contains all data needed for consumers to act without querying back to the source context.

**Event versioning**: Event schemas are versioned to support backward-compatible evolution. Consumers tolerate unknown fields. Breaking schema changes result in new event types rather than modifications to existing types.

**Idempotency**: Event consumers are designed to be idempotent. Receiving the same event twice produces the same result as receiving it once. This is essential for body health data integrity -- a duplicate BiometricMeasurementRecorded event must not create a duplicate measurement in downstream contexts.

**Ordering**: Events for a single person (keyed by PersonId) are ordered chronologically. This ensures that downstream contexts process measurements in the sequence they occurred. Cross-person event ordering is not guaranteed and not required.

**Event sourcing consideration**: The Biometric Collection Context is a natural candidate for event sourcing, where the complete measurement history is stored as a sequence of events rather than as current state. This provides a natural audit trail for all body health data and supports temporal queries (e.g., "what was this person's weight on a specific date?").

## Resilience Patterns

**Dead letter handling**: Events that cannot be processed after a configured number of retries are routed to a dead letter queue for manual investigation. A malformed measurement event that causes a composition calculation failure is preserved for diagnostic review rather than silently dropped.

**Circuit breaker**: When a consuming context becomes unavailable, the publishing context continues to operate normally. Events accumulate and are processed when the consumer recovers. A Health Scoring Context outage does not prevent measurement capture.

**Compensating events**: When a domain error is discovered after events have been published, compensating events are issued rather than modifying or deleting published events. A MeasurementCorrected event supersedes the original BiometricMeasurementRecorded event.

## Eventual Consistency

The event-driven architecture embraces eventual consistency between bounded contexts. When a new weight measurement is recorded, the health score does not update instantaneously. This delay is acceptable and aligns with domain reality -- clinicians and individuals do not expect real-time composite score updates at the moment of measurement. The system achieves consistency within seconds or minutes, well within the acceptable latency for body health applications.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 8 on domain events.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 11 on event-driven architecture.
- Stopford, Ben. *Designing Event-Driven Systems*. O'Reilly Media, 2018.
