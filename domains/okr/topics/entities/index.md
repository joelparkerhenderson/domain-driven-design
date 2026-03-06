# Entities

## Overview

In Domain-Driven Design, an Entity is a domain object that is defined by its identity rather than by its attributes. Two entities with the same attributes are still distinct if they have different identities. Entities have a lifecycle: they are created, they change over time, and they may eventually be deactivated or archived. Their identity persists throughout these changes.

In the OKR domain, entities represent the core objects that participants create, modify, track, and evaluate throughout the OKR lifecycle. Each entity belongs to a specific aggregate and bounded context, and its identity enables references across aggregate and context boundaries.

## OKR Domain Entities

### Objective

**Aggregate**: Objective Aggregate (Aggregate Root)

**Bounded Context**: Objective Setting Context

**Identity**: ObjectiveId (UUID)

**Description**: An Objective is a qualitative, inspirational goal that describes what an organization, team, or individual wants to achieve within a defined time period. As the aggregate root, the Objective controls access to its contained Key Results and enforces aggregate-level invariants.

**Key Attributes**:
- ObjectiveId: UUID -- unique identifier, immutable after creation.
- Title: String -- concise, action-oriented statement of the objective.
- Description: String -- detailed elaboration of the objective's intent and context.
- OwnerId: PersonId -- reference to the person or team accountable for the objective.
- CycleId: CycleId -- reference to the OKR cycle this objective belongs to.
- State: ObjectiveState -- lifecycle state (Draft, Approved, Active, Scoring, Completed, Retired).
- OKRType: OKRType -- classification as Committed or Aspirational.
- Priority: Priority -- relative importance within the owner's set of objectives.
- DateRange: DateRange -- the time period during which this objective is pursued.
- CreatedAt: DateTime -- timestamp of creation.
- LastModifiedAt: DateTime -- timestamp of last modification.

**Lifecycle**: Draft -> Approved -> Active -> Scoring -> Completed (or Retired at any point before Completed).

**Business Rules**:
- Title must be non-empty and under 200 characters.
- An objective must have at least 1 and at most 5 Key Results before transitioning from Draft to Approved.
- An objective cannot be modified after entering the Completed state.
- The OKRType cannot be changed after the objective becomes Active.

### KeyResult

**Aggregate**: Objective Aggregate (contained entity)

**Bounded Context**: Objective Setting Context (defined), Key Result Tracking Context (tracked)

**Identity**: KeyResultId (UUID)

**Description**: A Key Result is a quantitative, measurable outcome that indicates progress toward an objective. Key Results define the measurable criteria for success. They exist within the Objective Aggregate but are referenced by ID in the Key Result Tracking Context for progress tracking and scoring.

**Key Attributes**:
- KeyResultId: UUID -- unique identifier.
- ObjectiveId: ObjectiveId -- reference to the parent objective.
- Description: String -- clear statement of the measurable outcome.
- Baseline: Decimal -- starting measurement value at cycle beginning.
- Target: Decimal -- desired measurement value at cycle end.
- CurrentValue: Decimal -- most recent measurement (updated during tracking).
- MeasurementType: MeasurementType -- how the key result is measured (Percentage, Absolute, Binary, Currency).
- Weight: Decimal -- relative importance within the objective (0.0-1.0, all weights sum to 1.0).
- SMARTCriteria: SMARTCriteria -- validation record confirming SMART compliance.
- DataSource: DataSourceType -- whether progress is manually reported or automatically ingested.

**Business Rules**:
- Target must differ from Baseline.
- Weight must be between 0.0 and 1.0.
- All Key Result weights within an Objective must sum to 1.0.
- Description must satisfy SMART criteria before the parent Objective can be approved.

### OKRCycle

**Aggregate**: OKRCycle Aggregate (Aggregate Root)

**Bounded Context**: Review & Cadence Context

**Identity**: CycleId (UUID)

**Description**: An OKR Cycle is a time-bounded period during which objectives and key results are pursued, tracked, and evaluated. Cycles provide the temporal structure for OKR practice, typically operating on a quarterly cadence with annual strategic cycles.

**Key Attributes**:
- CycleId: UUID -- unique identifier.
- Name: String -- human-readable name (e.g., "Q1 2026", "H1 2026").
- DateRange: DateRange -- start and end dates of the cycle.
- State: CycleState -- lifecycle state (Planning, Active, Scoring, Closed).
- CheckInCadence: CheckInCadence -- the scheduled frequency of check-ins during this cycle.
- OrganizationalScope: OrganizationalScope -- whether this cycle applies to the entire company, a division, or a team.
- CreatedBy: PersonId -- the person who created the cycle.

**Lifecycle**: Planning -> Active -> Scoring -> Closed.

**Business Rules**:
- State transitions must follow the defined order; no state may be skipped.
- DateRange must not overlap with another cycle at the same organizational scope.
- Cannot transition to Closed until all associated key results have finalized scores.

### Team

**Aggregate**: Referenced entity (not an aggregate root in the OKR domain; sourced from HR/organizational systems)

**Bounded Context**: Used across multiple contexts; canonical definition may reside in an external HR system, accessed via Anti-Corruption Layer.

**Identity**: TeamId (UUID)

**Description**: A Team is a group of individuals who collaborate on shared objectives. In the OKR domain, Teams serve as owners or contributors for objectives and key results. The team entity within the OKR system is a lightweight representation, with full team management delegated to HR systems.

**Key Attributes**:
- TeamId: UUID -- unique identifier.
- Name: String -- team name.
- LeaderId: PersonId -- the person who leads the team and is typically responsible for team-level OKRs.
- MemberIds: List of PersonId -- team members.
- OrganizationalLevel: OrganizationalLevel -- where this team sits in the hierarchy (e.g., Division, Department, Squad).
- ParentTeamId: TeamId (optional) -- reference to the parent team in the hierarchy.

**Business Rules**:
- A team must have at least one member (the leader).
- A team's organizational level must be below its parent team's level in the hierarchy.

### Individual

**Aggregate**: Referenced entity (sourced from HR systems via Anti-Corruption Layer)

**Bounded Context**: Used across multiple contexts.

**Identity**: PersonId (UUID)

**Description**: An Individual represents a person who participates in the OKR process as an owner, contributor, reviewer, or observer. Like Team, the Individual entity in the OKR domain is a lightweight projection of data from HR systems.

**Key Attributes**:
- PersonId: UUID -- unique identifier.
- DisplayName: String -- name displayed in the OKR system.
- Email: String -- contact email for notifications.
- TeamMemberships: List of TeamId -- teams this person belongs to.
- Role: OKRRole -- the person's role in the OKR process (OKRCoach, Manager, IndividualContributor, Executive).

**Business Rules**:
- Each individual must belong to at least one team.
- An individual's OKR role determines their permissions (e.g., only OKRCoach and Executive roles can create company-level objectives).

### ReviewSession

**Aggregate**: ReviewSession Aggregate (Aggregate Root)

**Bounded Context**: Review & Cadence Context

**Identity**: SessionId (UUID)

**Description**: A ReviewSession represents a scheduled or ad-hoc meeting where OKR progress is reviewed, confidence levels are updated, scores are assigned, or retrospective learnings are captured. Each session contains one or more CheckInEntries.

**Key Attributes**:
- SessionId: UUID -- unique identifier.
- CycleId: CycleId -- the cycle this session belongs to.
- SessionType: SessionType -- the type of review (WeeklyCheckIn, MidCycleReview, EndOfCycleGrading, Retrospective).
- ScheduledDate: DateTime -- when the session is scheduled.
- State: SessionState -- lifecycle state (Scheduled, InProgress, Finalized).
- FacilitatorId: PersonId -- the person leading the session.
- Attendance: Attendance -- tracks required and actual participants.
- Notes: String -- free-form session notes.

**Lifecycle**: Scheduled -> InProgress -> Finalized.

**Business Rules**:
- A session cannot be finalized without at least one CheckInEntry.
- EndOfCycleGrading sessions must include scores for all key results under review.
- A session's ScheduledDate must fall within its associated cycle's DateRange.

### AlignmentLink

**Aggregate**: AlignmentTree Aggregate (contained entity)

**Bounded Context**: Alignment & Cascading Context

**Identity**: LinkId (UUID)

**Description**: An AlignmentLink represents a directed relationship between two objectives, indicating that one (child) supports or contributes to the other (parent). Links are the building blocks of the AlignmentTree.

**Key Attributes**:
- LinkId: UUID -- unique identifier.
- ParentObjectiveId: ObjectiveId -- the objective being supported.
- ChildObjectiveId: ObjectiveId -- the objective providing support.
- AlignmentType: AlignmentType -- classification as Hierarchical, CrossTeam, or Supporting.
- Acknowledged: Boolean -- for CrossTeam links, whether both teams have acknowledged the relationship.
- CreatedAt: DateTime -- when the link was established.
- CreatedBy: PersonId -- who created the link.

**Business Rules**:
- ParentObjectiveId and ChildObjectiveId must differ (no self-alignment).
- Creating a link must not introduce a cycle in the alignment tree.
- CrossTeam links require acknowledgment from both team owners before becoming active.

## Entity Design Principles

### Identity Generation

All entity identities use UUIDs generated at creation time. This enables:
- Distributed ID generation without central coordination.
- Cross-context references without tight coupling.
- Event-sourced architectures where entities can be reconstructed from events.

### Identity Persistence

Entity identity is immutable after creation. An Objective retains its ObjectiveId throughout its lifecycle, even as its title, description, owner, and state change.

### Entity vs. Value Object Decision Criteria

In the OKR domain, the distinction between entities and value objects follows these criteria:
- If the object has a lifecycle and its identity matters (two objectives with the same title are still distinct), it is an entity.
- If the object is defined entirely by its attributes and is interchangeable with any other object having the same attributes (a Score of 0.8 is identical to any other Score of 0.8), it is a value object.

## Relationships to Other Topics

- **Aggregates**: Entities exist within aggregates; their aggregate membership is defined in [Aggregates](../aggregates/).
- **Value Objects**: The value objects that entities contain or reference are defined in [Value Objects](../value-objects/).
- **Domain Events**: Entities emit domain events when significant state changes occur, documented in [Domain Events](../domain-events/).
- **Repositories**: Aggregate root entities are persisted and retrieved via repositories, described in [Repositories](../repositories/).
- **Ubiquitous Language**: Entity names are drawn from the ubiquitous language, defined in [Ubiquitous Language](../ubiquitous-language/).
- **Bounded Contexts**: Each entity belongs to a specific bounded context, defined in [Bounded Contexts](../bounded-contexts/).
- **Anti-Corruption Layer**: Team and Individual entities are sourced via ACLs from external systems, described in [Anti-Corruption Layer](../anti-corruption-layer/).

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003. Chapter 5: A Model Expressed in Software (Entities).
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013. Chapter 5: Entities.
- Doerr, John. "Measure What Matters: How Google, Bono, and the Gates Foundation Rock the World with OKRs." Portfolio/Penguin, 2018.
- Wodtke, Christina. "Radical Focus: Achieving Your Most Important Goals with Objectives and Key Results." Cucina Media, 2016.
- Niven, Paul R. and Lamorte, Ben. "Objectives and Key Results: Driving Focus, Alignment, and Engagement with OKRs." Wiley, 2016.
