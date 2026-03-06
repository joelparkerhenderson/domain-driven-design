# Aggregates

## Overview

In Domain-Driven Design, an Aggregate is a cluster of related domain objects that are treated as a single unit for the purpose of data changes and consistency enforcement. Each Aggregate has a root entity (the Aggregate Root) through which all external access is mediated. Aggregates define transactional consistency boundaries: invariants within an aggregate are enforced immediately, while consistency across aggregates is maintained through eventual consistency via domain events.

In the OKR domain, aggregates model the natural groupings of objectives with their key results, cycles with their temporal rules, alignment trees with their structural constraints, and review sessions with their check-in data.

## OKR Domain Aggregates

### 1. Objective Aggregate

**Aggregate Root**: Objective

**Bounded Context**: Objective Setting Context

**Contains**:
- Objective (root entity)
- KeyResult (entity, contained within the aggregate)
- SMARTCriteria (value object, applied to each KeyResult)
- OKRType (value object: Committed or Aspirational)
- Priority (value object)
- DateRange (value object, representing the objective's time frame)

**Description**: The Objective Aggregate is the central aggregate in the OKR domain. It encapsulates an objective and all of its associated key results as a single consistency unit. This design ensures that invariants such as "an objective must have at least one key result before activation" and "all key results must satisfy SMART criteria" are enforced atomically.

**Invariants**:
- An Objective must have between 1 and 5 Key Results before transitioning from Draft to Active state.
- Each Key Result must have a valid baseline, target, and measurement type.
- Each Key Result must pass SMART criteria validation.
- The Objective's DateRange must fall within the associated OKR Cycle's DateRange.
- The OKRType (Committed/Aspirational) is set at creation and cannot be changed after the objective becomes Active.
- An Owner must be assigned before the Objective can be approved.

**Lifecycle States**: Draft -> Approved -> Active -> Scoring -> Completed | Retired

**Design Rationale**: Key Results are contained within the Objective Aggregate rather than being separate aggregates because they have no independent lifecycle. A Key Result cannot exist without an Objective, and modifications to Key Results must be validated against the Objective's constraints (e.g., cannot add a sixth Key Result). This containment ensures transactional consistency.

**Example**:
```
Objective Aggregate
├── Objective (Root)
│   ├── ObjectiveId: UUID
│   ├── Title: String
│   ├── Description: String
│   ├── OwnerId: PersonId (reference)
│   ├── OKRType: Committed | Aspirational
│   ├── State: Draft | Approved | Active | Scoring | Completed | Retired
│   ├── CycleId: CycleId (reference to OKRCycle Aggregate)
│   ├── Priority: Priority (value object)
│   ├── DateRange: DateRange (value object)
│   └── KeyResults: List<KeyResult>
│       ├── KeyResult
│       │   ├── KeyResultId: UUID
│       │   ├── Description: String
│       │   ├── Baseline: Decimal
│       │   ├── Target: Decimal
│       │   ├── MeasurementType: Percentage | Absolute | Binary | Currency
│       │   ├── SMARTCriteria: SMARTCriteria (value object)
│       │   └── Weight: Decimal (0.0-1.0, weights sum to 1.0)
│       └── ...
```

### 2. OKRCycle Aggregate

**Aggregate Root**: OKRCycle

**Bounded Context**: Review & Cadence Context

**Contains**:
- OKRCycle (root entity)
- DateRange (value object)
- CycleState (value object: Planning, Active, Scoring, Closed)
- CheckInCadence (value object: defines check-in frequency)

**Description**: The OKRCycle Aggregate manages the temporal container within which OKR activities occur. It owns the cycle state machine that governs transitions from Planning through Active to Scoring and Closed. The cycle defines the time boundaries that constrain objective setting, progress tracking, and score finalization.

**Invariants**:
- Cycle states must transition in order: Planning -> Active -> Scoring -> Closed. No state can be skipped.
- A Cycle cannot transition from Active to Scoring before its end date.
- A Cycle cannot transition from Scoring to Closed until all associated key results have been scored (enforced via eventual consistency with Key Result Tracking Context).
- DateRanges of cycles within the same organizational scope must not overlap.
- CheckInCadence must define a frequency that fits within the cycle's DateRange.

**Lifecycle States**: Planning -> Active -> Scoring -> Closed

**Design Rationale**: The OKRCycle is a separate aggregate from Objective because cycles have an independent lifecycle and serve as shared temporal containers for many objectives across the organization. The cycle does not contain objectives; instead, objectives reference the cycle by ID.

**Example**:
```
OKRCycle Aggregate
├── OKRCycle (Root)
│   ├── CycleId: UUID
│   ├── Name: String (e.g., "Q1 2026")
│   ├── DateRange: DateRange (value object)
│   │   ├── StartDate: Date
│   │   └── EndDate: Date
│   ├── State: Planning | Active | Scoring | Closed
│   ├── CheckInCadence: CheckInCadence (value object)
│   │   ├── Frequency: Weekly | Biweekly | Monthly
│   │   └── DayOfWeek: Monday | ... | Friday
│   └── OrganizationalScope: Company | Division | Team
```

### 3. AlignmentTree Aggregate

**Aggregate Root**: AlignmentTree

**Bounded Context**: Alignment & Cascading Context

**Contains**:
- AlignmentTree (root entity)
- AlignmentLink (entity, representing a parent-child objective relationship)
- AlignmentType (value object: Hierarchical, CrossTeam, Supporting)

**Description**: The AlignmentTree Aggregate models the hierarchical and lateral relationships between objectives across organizational levels. It is the aggregate that enforces structural invariants on how objectives relate to each other, ensuring strategic coherence.

**Invariants**:
- The alignment tree must be acyclic (directed acyclic graph). No circular dependencies are permitted.
- A child objective's cycle must fall within or equal its parent objective's cycle.
- Each alignment link must specify its type (Hierarchical, CrossTeam, Supporting).
- Cross-team alignment links must be bidirectionally acknowledged (both team owners agree).
- An objective cannot have more than one hierarchical parent (tree structure for hierarchical links; DAG allowed for cross-team links).

**Design Rationale**: Alignment is modeled as its own aggregate rather than being embedded in the Objective Aggregate because alignment relationships span multiple objectives owned by different teams and contexts. The AlignmentTree maintains the global structural invariants (acyclicity, consistency) that cannot be enforced within a single Objective Aggregate.

**Example**:
```
AlignmentTree Aggregate
├── AlignmentTree (Root)
│   ├── TreeId: UUID
│   ├── CycleId: CycleId (reference)
│   ├── RootObjectiveId: ObjectiveId (the top-level strategic objective)
│   └── Links: List<AlignmentLink>
│       ├── AlignmentLink
│       │   ├── LinkId: UUID
│       │   ├── ParentObjectiveId: ObjectiveId
│       │   ├── ChildObjectiveId: ObjectiveId
│       │   ├── AlignmentType: Hierarchical | CrossTeam | Supporting
│       │   ├── Acknowledged: Boolean (for cross-team)
│       │   └── CreatedAt: DateTime
│       └── ...
```

### 4. ReviewSession Aggregate

**Aggregate Root**: ReviewSession

**Bounded Context**: Review & Cadence Context

**Contains**:
- ReviewSession (root entity)
- CheckInEntry (entity, individual check-in records within a session)
- SessionType (value object: WeeklyCheckIn, MidCycleReview, EndOfCycleGrading, Retrospective)
- Attendance (value object)

**Description**: The ReviewSession Aggregate captures the data produced during OKR review ceremonies. Each session contains check-in entries that record progress updates, confidence levels, blockers, and discussion notes. The aggregate preserves the integrity of a review session as an atomic event.

**Invariants**:
- A ReviewSession must have at least one CheckInEntry to be considered complete.
- CheckInEntries cannot be added to a session after it is finalized.
- A ReviewSession must reference an active OKRCycle.
- The SessionType determines required fields: EndOfCycleGrading sessions must include scores; WeeklyCheckIn sessions must include confidence levels.
- Each CheckInEntry references a specific KeyResult by ID.

**Design Rationale**: Review sessions are modeled as separate aggregates because they represent distinct temporal events with their own lifecycle (Scheduled -> InProgress -> Finalized). They are not contained within OKRCycle because a cycle may have many review sessions, and loading the entire cycle aggregate for each check-in would be impractical.

**Example**:
```
ReviewSession Aggregate
├── ReviewSession (Root)
│   ├── SessionId: UUID
│   ├── CycleId: CycleId (reference)
│   ├── SessionType: WeeklyCheckIn | MidCycleReview | EndOfCycleGrading | Retrospective
│   ├── ScheduledDate: DateTime
│   ├── State: Scheduled | InProgress | Finalized
│   ├── Attendance: Attendance (value object)
│   │   ├── RequiredParticipants: List<PersonId>
│   │   └── ActualParticipants: List<PersonId>
│   └── Entries: List<CheckInEntry>
│       ├── CheckInEntry
│       │   ├── EntryId: UUID
│       │   ├── KeyResultId: KeyResultId (reference)
│       │   ├── OwnerId: PersonId
│       │   ├── CurrentValue: Decimal
│       │   ├── ConfidenceLevel: High | Medium | Low
│       │   ├── Narrative: String
│       │   ├── Blockers: List<String>
│       │   └── RecordedAt: DateTime
│       └── ...
```

## Aggregate Design Principles Applied

### Small Aggregates

Following Vernon's guidance, OKR aggregates are kept small. The Objective Aggregate contains KeyResults (typically 1-5 per objective) but does not contain ProgressUpdates or Scores, which live in the Key Result Tracking Context. This prevents the aggregate from growing unbounded over time.

### Reference by Identity

Aggregates reference each other by identity (ID) rather than direct object references. The Objective Aggregate holds a CycleId, not a full OKRCycle object. This allows aggregates to reside in different bounded contexts and even different data stores.

### Consistency Boundaries

Within an aggregate, consistency is immediate. Across aggregates, consistency is eventual. When a ScoreFinalized event occurs in the Key Result Tracking Context, the OKRCycle Aggregate in the Review & Cadence Context eventually receives it and can determine whether all scores are complete.

### Aggregate Size Justification

- **Objective + KeyResults**: Combined because KeyResults have no independent lifecycle and business rules require joint validation (e.g., key result count limits, weight sum to 1.0).
- **OKRCycle**: Standalone because cycles are shared containers referenced by many objectives.
- **AlignmentTree**: Standalone because alignment spans objectives from different teams and contexts.
- **ReviewSession + CheckInEntries**: Combined because check-in entries only make sense within a session, and session-level invariants (e.g., at least one entry required) require containment.

## Relationships to Other Topics

- **Entities**: The entities within each aggregate are detailed in [Entities](../entities/).
- **Value Objects**: The value objects used by aggregates are defined in [Value Objects](../value-objects/).
- **Domain Events**: Events emitted by aggregates are cataloged in [Domain Events](../domain-events/).
- **Repositories**: Each aggregate root has a corresponding repository, defined in [Repositories](../repositories/).
- **Domain Services**: Operations spanning multiple aggregates are handled by domain services, described in [Domain Services](../domain-services/).
- **Bounded Contexts**: Aggregates live within specific bounded contexts, defined in [Bounded Contexts](../bounded-contexts/).
- **Strategic Design**: Aggregate boundaries are identified during Event Storming as part of Strategic Design, described in [Strategic Design](../strategic-design/).

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003. Chapter 6: The Life Cycle of a Domain Object (Aggregates).
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013. Chapter 10: Aggregates.
- Vernon, Vaughn. "Effective Aggregate Design." Three-part series, 2011. dddcommunity.org.
- Doerr, John. "Measure What Matters: How Google, Bono, and the Gates Foundation Rock the World with OKRs." Portfolio/Penguin, 2018.
- Wodtke, Christina. "Radical Focus: Achieving Your Most Important Goals with Objectives and Key Results." Cucina Media, 2016.
