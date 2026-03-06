# Event-Driven Architecture in Learning Management System

## Overview

Event-driven architecture in an LMS enables loose coupling between bounded contexts, allowing enrollment events to trigger progress tracking, assessment results to update completion records, and content interactions to feed analytics—all without direct dependencies.

## Key Event Flows

### Enrollment Flow

```
StudentEnrolled → [Enrollment Context]
  → Progress Context: Create LearnerRecord
  → Content Delivery: Provision content access
  → Notifications: Send enrollment confirmation
  → Analytics: Record enrollment event
```

### Learning Flow

```
ContentAccessed → [Content Delivery Context]
  → Progress: Record activity
  → Analytics: Track engagement

ContentCompleted → [Content Delivery Context]
  → Progress: Update lesson/module completion
  → Adaptive Learning: Adjust content sequence
```

### Assessment Flow

```
AssessmentSubmitted → [Assessment Context]
  → Grading: Queue for auto/manual grading

AssessmentGraded → [Assessment Context]
  → Progress: Update grades and completion
  → Notifications: Alert student of grade
  → Analytics: Update performance metrics
```

### Completion Flow

```
CourseCompleted → [Progress Context]
  → Certification: Issue certificate
  → Enrollment: Update enrollment status
  → Transcript: Update learner record
  → Analytics: Record completion
  → External: xAPI statement to LRS
```

## Event Infrastructure

### Message Broker

- Events published to topic-based channels (e.g., `enrollment.events`, `assessment.events`)
- Consumers subscribe to relevant topics
- At-least-once delivery with idempotent consumers
- Dead letter queues for failed event processing

### Event Store

- Immutable log of all domain events
- Supports event sourcing for audit-critical aggregates (e.g., grades, completions)
- Enables temporal queries ("What was the student's progress on date X?")
- Replay capability for rebuilding read models

### CQRS Pattern

- **Command side**: Aggregate operations that produce events
- **Query side**: Read-optimized projections for dashboards, reports, analytics
- **Projections**: Progress dashboards, grade books, enrollment reports, transcript views

## Event Design Principles

1. **Past tense naming**: StudentEnrolled, AssessmentGraded, CourseCompleted
2. **Self-contained events**: Include all data consumers need; avoid callbacks
3. **Versioned schemas**: Events evolve with backward-compatible schema versions
4. **Idempotent consumers**: Processing the same event twice produces the same result
5. **Ordering guarantees**: Events for the same aggregate are processed in order

## Saga Patterns

### Enrollment Saga

1. Check prerequisites (Progress Context)
2. Check capacity (Enrollment Context)
3. Create enrollment (Enrollment Context)
4. Create learner record (Progress Context)
5. Provision content access (Content Delivery)
- **Compensation**: If any step fails, reverse completed steps

### Grading Saga

1. Submit assessment (Assessment Context)
2. Auto-grade objective questions (Assessment Context)
3. Queue subjective questions for manual review (Assessment Context)
4. Calculate final grade when all grading complete
5. Update progress (Progress Context)

## Relationships to Other Topics

- [Domain Events](../domain-events/) — Events that flow through the architecture
- [Bounded Contexts](../bounded-contexts/) — Contexts communicate via events
- [Integration Patterns](../integration-patterns/) — Event-driven integration strategies
- [Context Map](../context-map/) — Relationships between event producers and consumers

## References

- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 8. Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 9. O'Reilly, 2021.
- Richardson, Chris. "Microservices Patterns." Chapter 4. Manning, 2018.
