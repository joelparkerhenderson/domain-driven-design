# Domain Events

## Overview

In Domain-Driven Design, a Domain Event represents something significant that happened in the domain. Domain events are immutable records of past occurrences that other parts of the system may react to. They enable loose coupling between bounded contexts by allowing contexts to communicate through asynchronous event-based integration rather than direct synchronous calls.

In the OKR domain, domain events capture the key moments in the OKR lifecycle: when objectives are created, when progress is recorded, when cycles transition, and when scores are finalized. These events drive the coordination between the five bounded contexts.

## OKR Domain Events

### ObjectiveCreated

**Source Context**: Objective Setting Context

**Payload**: ObjectiveId, Title, OwnerId, CycleId, OKRType, Priority, DateRange, CreatedAt.

**Description**: Raised when a new Objective is created in Draft state. Downstream consumers use this event to prepare alignment structures and initialize tracking projections.

**Consumers**: Alignment & Cascading Context (to offer alignment suggestions), Analytics & Reporting Context (to update objective counts).

### KeyResultUpdated

**Source Context**: Objective Setting Context

**Payload**: KeyResultId, ObjectiveId, Description, Baseline, Target, MeasurementType, Weight, UpdatedAt.

**Description**: Raised when a Key Result's definition is modified, such as changes to its target, baseline, or measurement type. This event is distinct from progress updates, which occur in the Key Result Tracking Context.

**Consumers**: Key Result Tracking Context (to update tracking parameters), Analytics & Reporting Context (to refresh projections).

### CycleOpened

**Source Context**: Review & Cadence Context

**Payload**: CycleId, Name, DateRange, CheckInCadence, OrganizationalScope, OpenedAt.

**Description**: Raised when an OKR Cycle transitions from Planning to Active state. This is the trigger for teams to begin finalizing and activating their objectives.

**Consumers**: Objective Setting Context (to enable objective activation for the new cycle), Analytics & Reporting Context (to initialize cycle-level dashboards).

### CycleClosed

**Source Context**: Review & Cadence Context

**Payload**: CycleId, Name, DateRange, ClosedAt, FinalStatistics (total objectives, average score).

**Description**: Raised when a cycle transitions to the Closed state after all scoring is complete. This marks the end of the OKR cycle and triggers retrospective activities.

**Consumers**: Key Result Tracking Context (to prevent further updates), Analytics & Reporting Context (to finalize cycle reports), Objective Setting Context (to archive associated objectives).

### CheckInRecorded

**Source Context**: Key Result Tracking Context

**Payload**: CheckInId, KeyResultId, ObjectiveId, SessionId, CurrentValue, PreviousValue, ConfidenceLevel, Narrative, RecordedAt, RecordedBy.

**Description**: Raised when a progress check-in is recorded during a review session. Each check-in captures the current value, confidence assessment, and contextual narrative for a specific Key Result.

**Consumers**: Analytics & Reporting Context (to update progress dashboards and trend charts), Review & Cadence Context (to track check-in completion rates).

### ScoreFinalized

**Source Context**: Key Result Tracking Context

**Payload**: KeyResultId, ObjectiveId, CycleId, FinalScore (Score value object), ScoredAt, ScoredBy.

**Description**: Raised when a Key Result's score is officially finalized during end-of-cycle grading. Once finalized, the score is immutable. This event is critical for cycle closure logic.

**Consumers**: Review & Cadence Context (to determine if all scores are in and the cycle can close), Analytics & Reporting Context (to compute final cycle statistics), Alignment & Cascading Context (to propagate score visibility to parent objectives).

### AlignmentChanged

**Source Context**: Alignment & Cascading Context

**Payload**: LinkId, ParentObjectiveId, ChildObjectiveId, AlignmentType, ChangeType (Created, Removed, TypeChanged), ChangedAt, ChangedBy.

**Description**: Raised when an alignment link between objectives is created, removed, or modified. This event enables downstream systems to maintain accurate representations of the alignment hierarchy.

**Consumers**: Analytics & Reporting Context (to update alignment coverage metrics), Objective Setting Context (to display alignment status on objectives).

## Event Design Principles

### Immutability

Domain events are immutable facts about the past. An ObjectiveCreated event cannot be retracted or modified. If an objective is later renamed, that produces a separate ObjectiveUpdated event.

### Self-Contained Payloads

Each event carries sufficient data for consumers to process it without querying the source context. The ScoreFinalized event includes the FinalScore, CycleId, and ObjectiveId so that the Analytics context can update its projections without calling back to the Key Result Tracking Context.

### Temporal Ordering

Events include timestamps that establish ordering. Within a single aggregate, events are strictly ordered. Across aggregates and contexts, consumers must handle out-of-order delivery gracefully using idempotent processing.

### Naming Convention

Events are named in past tense (ObjectiveCreated, not CreateObjective) to reflect that they describe something that has already occurred.

## Relationships to Other Topics

- **Aggregates**: Events are emitted by aggregate roots when state changes occur. See [Aggregates](../aggregates/).
- **Entities**: Events reference entities by their identity (ObjectiveId, KeyResultId). See [Entities](../entities/).
- **Value Objects**: Events carry value objects as payload data. See [Value Objects](../value-objects/).
- **Event-Driven Architecture**: The infrastructure for publishing and subscribing to events is described in [Event-Driven Architecture](../event-driven-architecture/).
- **Bounded Contexts**: Each event has a source context and one or more consumer contexts. See [Bounded Contexts](../bounded-contexts/).
- **Repositories**: Events may trigger repository operations in consumer contexts. See [Repositories](../repositories/).

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003. Chapter 8: Breakthrough (domain events as a modeling tool).
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013. Chapter 8: Domain Events.
- Doerr, John. "Measure What Matters: How Google, Bono, and the Gates Foundation Rock the World with OKRs." Portfolio/Penguin, 2018.
- Khononov, Vlad. "Learning Domain-Driven Design." O'Reilly, 2021. Chapter 9: Communication Patterns.
