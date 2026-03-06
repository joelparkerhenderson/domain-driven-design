# Repositories

## Overview

In Domain-Driven Design, a Repository is an abstraction that mediates between the domain layer and the data mapping layer, providing collection-like access to aggregate roots. Repositories encapsulate the logic required to persist and retrieve aggregates, shielding the domain model from infrastructure concerns such as database technology, query languages, and serialization formats.

In the OKR domain, each aggregate root has a corresponding repository. Repositories operate at the aggregate boundary: they store and retrieve entire aggregates, never individual contained entities or value objects in isolation.

## OKR Domain Repositories

### ObjectiveRepository

**Aggregate Root**: Objective

**Bounded Context**: Objective Setting Context

**Description**: The ObjectiveRepository provides persistence and retrieval for the Objective Aggregate, including the Objective root entity and its contained Key Results, value objects (SMARTCriteria, OKRType, Priority, DateRange), and lifecycle state.

**Core Operations**:
- `save(objective: Objective)`: Persist a new or modified Objective Aggregate.
- `findById(objectiveId: ObjectiveId): Objective`: Retrieve an Objective Aggregate by its identity.
- `findByCycleId(cycleId: CycleId): List<Objective>`: Retrieve all objectives associated with a given cycle.
- `findByOwnerId(ownerId: PersonId): List<Objective>`: Retrieve all objectives owned by a specific person.
- `findByTeamId(teamId: TeamId): List<Objective>`: Retrieve all objectives owned by a specific team.
- `findByState(state: ObjectiveState): List<Objective>`: Retrieve objectives in a given lifecycle state.
- `delete(objectiveId: ObjectiveId)`: Remove a Draft objective that has not yet been approved.

**Invariants Enforced**:
- Only whole Objective Aggregates (including all Key Results) are persisted and retrieved.
- Deleting an objective is only permitted in the Draft state.
- Optimistic concurrency control ensures that concurrent modifications to the same objective are detected and resolved.

### OKRCycleRepository

**Aggregate Root**: OKRCycle

**Bounded Context**: Review & Cadence Context

**Description**: The OKRCycleRepository manages the persistence and retrieval of OKR Cycle aggregates. Cycles define the temporal structure for all OKR activities and are referenced by many objectives across the organization.

**Core Operations**:
- `save(cycle: OKRCycle)`: Persist a new or modified OKR Cycle.
- `findById(cycleId: CycleId): OKRCycle`: Retrieve a cycle by its identity.
- `findByState(state: CycleState): List<OKRCycle>`: Retrieve cycles in a given state (e.g., all Active cycles).
- `findByDateRange(date: Date): List<OKRCycle>`: Retrieve cycles whose DateRange contains the given date.
- `findByOrganizationalScope(scope: OrganizationalScope): List<OKRCycle>`: Retrieve cycles at a specific organizational level.
- `findCurrentCycle(scope: OrganizationalScope): OKRCycle`: Convenience method to retrieve the currently active cycle for a given scope.

**Invariants Enforced**:
- Cycle DateRanges at the same organizational scope must not overlap. The repository validates this constraint on save.

### ReviewSessionRepository

**Aggregate Root**: ReviewSession

**Bounded Context**: Review & Cadence Context

**Description**: The ReviewSessionRepository manages the persistence and retrieval of ReviewSession aggregates, including their contained CheckInEntry records. Review sessions capture the data produced during OKR ceremonies.

**Core Operations**:
- `save(session: ReviewSession)`: Persist a new or modified ReviewSession.
- `findById(sessionId: SessionId): ReviewSession`: Retrieve a review session by its identity.
- `findByCycleId(cycleId: CycleId): List<ReviewSession>`: Retrieve all sessions within a cycle.
- `findByType(cycleId: CycleId, type: SessionType): List<ReviewSession>`: Retrieve sessions of a specific type within a cycle.
- `findByFacilitator(facilitatorId: PersonId): List<ReviewSession>`: Retrieve sessions facilitated by a specific person.
- `findUpcoming(fromDate: DateTime): List<ReviewSession>`: Retrieve sessions scheduled in the future.

**Invariants Enforced**:
- A session's ScheduledDate must fall within the associated cycle's DateRange.
- Finalized sessions cannot be modified.

### AlignmentRepository

**Aggregate Root**: AlignmentTree

**Bounded Context**: Alignment & Cascading Context

**Description**: The AlignmentRepository manages the persistence and retrieval of AlignmentTree aggregates, which model the hierarchical and lateral relationships between objectives across organizational levels.

**Core Operations**:
- `save(tree: AlignmentTree)`: Persist a new or modified AlignmentTree.
- `findById(treeId: TreeId): AlignmentTree`: Retrieve an alignment tree by its identity.
- `findByCycleId(cycleId: CycleId): AlignmentTree`: Retrieve the alignment tree for a specific cycle.
- `findParentLinks(objectiveId: ObjectiveId): List<AlignmentLink>`: Find all links where the given objective is a child.
- `findChildLinks(objectiveId: ObjectiveId): List<AlignmentLink>`: Find all links where the given objective is a parent.
- `findCrossTeamLinks(cycleId: CycleId): List<AlignmentLink>`: Retrieve all cross-team alignment links for a cycle.

**Invariants Enforced**:
- Acyclicity is validated on every save operation to prevent circular alignment dependencies.
- Cross-team links must be bidirectionally acknowledged before becoming active.

## Repository Design Principles

### Aggregate-Level Persistence

Repositories always operate on complete aggregates. The ObjectiveRepository saves and retrieves the entire Objective Aggregate including all Key Results, never a Key Result in isolation. This preserves the aggregate's consistency boundary.

### Collection-Oriented Interface

Repositories present a collection-like interface to the domain layer. From the domain's perspective, the repository behaves like an in-memory collection of aggregates, abstracting away persistence details.

### No Domain Logic

Repositories contain no business logic. Validation, scoring, and state transitions belong in domain services, aggregates, and entities. Repositories only persist and retrieve.

### Infrastructure Independence

Repository interfaces are defined in the domain layer. Implementations reside in the infrastructure layer and may use any persistence technology (relational databases, document stores, event stores) without affecting the domain model.

## Relationships to Other Topics

- **Aggregates**: Each repository corresponds to exactly one aggregate root. See [Aggregates](../aggregates/).
- **Entities**: Aggregate root entities are the objects stored and retrieved by repositories. See [Entities](../entities/).
- **Domain Services**: Domain services use repositories to load and persist aggregates. See [Domain Services](../domain-services/).
- **Domain Events**: Repository save operations may trigger domain event publication. See [Domain Events](../domain-events/).
- **Bounded Contexts**: Each repository belongs to a specific bounded context. See [Bounded Contexts](../bounded-contexts/).

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003. Chapter 6: The Life Cycle of a Domain Object (Repositories).
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013. Chapter 12: Repositories.
- Doerr, John. "Measure What Matters: How Google, Bono, and the Gates Foundation Rock the World with OKRs." Portfolio/Penguin, 2018.
