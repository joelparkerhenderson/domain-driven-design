# Event-Driven Architecture

## Overview

In Domain-Driven Design, Event-Driven Architecture (EDA) is a communication pattern where bounded contexts interact through the publication and consumption of domain events rather than through direct synchronous calls. EDA enables loose coupling, autonomous evolution of contexts, and eventual consistency across the system.

In the OKR domain, event-driven architecture is the primary integration mechanism connecting the five bounded contexts. Events flow from contexts where state changes originate to contexts that need to react, creating a responsive and decoupled system.

## Event Flows Between OKR Contexts

### Objective Setting to Key Result Tracking

**Events**: KeyResultDefined, KeyResultModified

**Flow**: When a Key Result is defined or modified in the Objective Setting Context, events are published so the Key Result Tracking Context can initialize or update its tracking structures. The tracking context creates its own local representation of the Key Result (with tracking-specific attributes like progress history and confidence timeline) without depending on the Objective Setting Context's data store.

### Objective Setting to Alignment & Cascading

**Events**: ObjectiveCreated, ObjectiveUpdated, ObjectiveRetired

**Flow**: When objectives are created or changed, the Alignment & Cascading Context receives notifications to offer alignment suggestions, revalidate existing links, and handle orphaned child objectives when a parent is retired.

### Key Result Tracking to Review & Cadence

**Events**: ScoreFinalized

**Flow**: When a Key Result's score is finalized during end-of-cycle grading, the event is consumed by the Review & Cadence Context to track scoring completion. The CycleTransitionService uses this information to determine when all scores are in and the cycle can transition to Closed.

### Review & Cadence to Objective Setting

**Events**: CycleOpened

**Flow**: When a cycle transitions from Planning to Active, the Objective Setting Context is notified so that draft objectives can be activated for the new cycle. This event initiates the transition of approved objectives into their active state.

### Review & Cadence to Key Result Tracking

**Events**: CycleClosed

**Flow**: When a cycle closes, the Key Result Tracking Context is notified to prevent further progress updates and ensure all scores have been finalized.

### All Contexts to Analytics & Reporting

**Events**: ObjectiveCreated, ObjectiveUpdated, KeyResultDefined, CheckInRecorded, ProgressUpdated, ScoreFinalized, AlignmentCreated, AlignmentChanged, CycleOpened, CycleClosed

**Flow**: The Analytics & Reporting Context subscribes to events from all upstream contexts to build and maintain its read-optimized projections. This is the most event-consuming context, as it must aggregate data from the entire OKR lifecycle for dashboards and reports.

## Event Flow Summary

```
Objective Setting Context
  ├── ObjectiveCreated ──────> Alignment & Cascading Context
  ├── ObjectiveUpdated ──────> Alignment & Cascading Context
  ├── ObjectiveRetired ──────> Alignment & Cascading Context
  ├── KeyResultDefined ──────> Key Result Tracking Context
  ├── KeyResultModified ─────> Key Result Tracking Context
  └── (all events) ─────────> Analytics & Reporting Context

Key Result Tracking Context
  ├── ScoreFinalized ────────> Review & Cadence Context
  ├── CheckInRecorded ───────> Analytics & Reporting Context
  └── (all events) ─────────> Analytics & Reporting Context

Review & Cadence Context
  ├── CycleOpened ───────────> Objective Setting Context
  ├── CycleClosed ───────────> Key Result Tracking Context
  └── (all events) ─────────> Analytics & Reporting Context

Alignment & Cascading Context
  └── (all events) ─────────> Analytics & Reporting Context
```

## Event Infrastructure Principles

### At-Least-Once Delivery

The OKR event infrastructure guarantees at-least-once delivery. Consumers must be idempotent, meaning that processing the same event multiple times produces the same result as processing it once. For example, the Analytics context must handle receiving a duplicate ScoreFinalized event without double-counting.

### Event Ordering

Within a single aggregate, events are strictly ordered by sequence number. Across aggregates, events may arrive out of order. Consumers that depend on ordering (e.g., progress history reconstruction) use event timestamps and sequence numbers to establish correct order.

### Event Versioning

As the domain model evolves, event schemas may change. The system supports event versioning through:
- **Additive changes**: New fields are added with default values; existing consumers ignore unknown fields.
- **Breaking changes**: New event types are introduced alongside deprecated versions, with a migration period.

### Eventual Consistency

Cross-context consistency is eventual, not immediate. When an ObjectiveCreated event is published, there is a brief period where the Alignment & Cascading Context has not yet received it. The system is designed to tolerate this latency, and user-facing features communicate data freshness where relevant.

## Relationships to Other Topics

- **Domain Events**: The specific events flowing through this architecture are defined in [Domain Events](../domain-events/).
- **Bounded Contexts**: Each context's role as producer or consumer is specified in [Bounded Contexts](../bounded-contexts/).
- **Integration Patterns**: External system integration builds on this event infrastructure. See [Integration Patterns](../integration-patterns/).
- **Context Map**: The relationships between contexts that events traverse are mapped in [Context Map](../context-map/).
- **Analytics & Reporting Context**: The primary event consumer is the [Analytics & Reporting Context](../analytics-reporting-context/).

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003. Chapter 14: Maintaining Model Integrity.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013. Chapter 8: Domain Events, Chapter 13: Integrating Bounded Contexts.
- Doerr, John. "Measure What Matters: How Google, Bono, and the Gates Foundation Rock the World with OKRs." Portfolio/Penguin, 2018.
- Khononov, Vlad. "Learning Domain-Driven Design." O'Reilly, 2021. Chapter 9: Communication Patterns.
- Richardson, Chris. "Microservices Patterns." Manning, 2018. Chapter 3: Interprocess Communication (Event-Driven).
