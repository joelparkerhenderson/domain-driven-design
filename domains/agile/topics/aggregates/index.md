# Aggregates in Agile Management

## Overview

An aggregate is a cluster of domain objects that are treated as a single unit for the purpose of data changes and consistency enforcement. Each aggregate has a root entity (the aggregate root) through which all external access is managed. The aggregate root enforces invariants -- business rules that must always be true -- across all objects within the aggregate boundary. In agile management, aggregates define the transactional boundaries around sprints, backlogs, teams, retrospectives, and releases.

## Relevance to Agile Management

Agile data involves complex relationships: a sprint contains backlog items that have tasks, estimates, and acceptance criteria; a team has members with availability and skills; a retrospective produces action items that feed into future sprints. Aggregates define which objects must be consistent together in a single transaction and which can be eventually consistent across transactions, communicated via domain events.

## Key Concepts

### Aggregate Root

The single entity through which all modifications to the aggregate pass. External code never directly modifies objects inside the aggregate -- it always goes through the root. In agile management, Sprint is the aggregate root for all sprint-related data; external services request sprint changes through the Sprint entity, not by directly modifying individual sprint backlog items.

### Invariants

Business rules that must hold at all times within the aggregate boundary. Agile examples include:

- A sprint cannot contain more story points than the team's calculated capacity
- A retrospective must produce at least one action item before it can be closed
- A release cannot be deployed if it contains stories that do not meet the Definition of Done

### Transactional Consistency

All changes within an aggregate are committed or rolled back as a single transaction. Changes between aggregates are eventually consistent, communicated via domain events.

## Agile Aggregates

### Sprint Aggregate

- **Root**: Sprint (identified by SprintID)
- **Internal entities**: SprintBacklogItem
- **Internal value objects**: SprintGoal, TimeBox, BurndownSnapshot, DefinitionOfDone
- **Invariants**:
  - Sprint duration (time-box) is fixed once the sprint starts and cannot be changed
  - Sprint must have a SprintGoal before it can transition to "Active" state
  - Total committed story points must not exceed team capacity
  - Only items meeting the Definition of Done can be marked as completed
  - Items cannot be added to an active sprint without team agreement (recorded as a SprintScopeChange event)
  - A sprint must belong to exactly one team
- **Events produced**: SprintPlanned, SprintStarted, SprintCompleted, StoryCompleted, StoryBlocked, SprintScopeChanged, DailyStandupRecorded
- **Lifecycle states**: Draft -> Planned -> Active -> InReview -> Completed

### ProductBacklog Aggregate

- **Root**: ProductBacklog (identified by ProductBacklogID)
- **Internal entities**: BacklogItem (UserStory, Bug, Spike, TechnicalDebt)
- **Internal value objects**: Priority (MoSCoW or RICE), StoryPoint, AcceptanceCriteria, EstimateHistory
- **Invariants**:
  - Each backlog item must have a unique priority rank within the backlog (no two items share the same rank)
  - Backlog items marked as "ready" must have acceptance criteria and an estimate
  - Epics must decompose into at least one user story before they can be marked as refined
  - The Product Owner is the sole authority for priority ordering
- **Events produced**: BacklogItemCreated, BacklogItemEstimated, BacklogItemPrioritized, BacklogRefined, EpicDecomposed, StoryReady
- **Lifecycle states**: N/A (backlog is a long-lived aggregate that evolves continuously)

### Team Aggregate

- **Root**: Team (identified by TeamID)
- **Internal entities**: TeamMember
- **Internal value objects**: Velocity, Capacity, SkillMatrix, Availability
- **Invariants**:
  - Velocity is calculated only from completed sprints (not from in-progress or cancelled sprints)
  - Team capacity cannot exceed the sum of individual member availability for a given sprint
  - A team member cannot be allocated to more than their available capacity across all teams
  - A team must have at least one member to be considered active
- **Events produced**: TeamFormed, MemberAdded, MemberRemoved, VelocityCalculated, CapacityUpdated, SkillMatrixUpdated
- **Lifecycle states**: Forming -> Active -> Disbanded

### Retrospective Aggregate

- **Root**: Retrospective (identified by RetrospectiveID)
- **Internal entities**: ActionItem
- **Internal value objects**: Observation (WentWell, CouldImprove, Idea), FacilitationFormat
- **Invariants**:
  - A retrospective must be associated with a completed sprint
  - Every retrospective must produce at least one action item before it can be closed
  - Action items must have an assignee and a target completion date
  - A retrospective cannot be reopened once it has been closed
- **Events produced**: RetroStarted, ObservationRecorded, ActionItemCreated, RetroCompleted, ActionItemCompleted
- **Lifecycle states**: Open -> InProgress -> Closed

### Release Aggregate

- **Root**: Release (identified by ReleaseID)
- **Internal entities**: ReleaseItem, FeatureFlag
- **Internal value objects**: SemanticVersion, ReleaseNotes, DeploymentTarget, RollbackPlan
- **Invariants**:
  - A release must contain at least one completed increment
  - All release items must meet the Definition of Done
  - Feature flags must have an owner and an expiration policy
  - Rollback procedures must be defined before a release can be deployed to production
  - A release version must follow semantic versioning rules (MAJOR.MINOR.PATCH)
- **Events produced**: ReleasePlanned, ReleaseCandidateCreated, ReleaseDeployed, ReleaseRolledBack, FeatureFlagToggled
- **Lifecycle states**: Planned -> Candidate -> Deploying -> Deployed -> RolledBack (terminal) or Archived (terminal)

## Aggregate Design Rules

1. **Keep aggregates small**: Prefer smaller aggregates with fewer internal objects. A Sprint aggregate contains SprintBacklogItems but references the Team by TeamID, not by containing the Team object.

2. **Reference other aggregates by identity**: A Sprint references a Team by TeamID, not by containing the Team object. A Retrospective references a Sprint by SprintID.

3. **Use domain events for cross-aggregate coordination**: When a SprintCompleted event occurs, the Team aggregate updates its velocity, the Retrospective aggregate initiates a new retrospective, and the Release aggregate evaluates whether a release candidate can be created -- but these happen asynchronously, not in the same transaction.

4. **Design for eventual consistency between aggregates**: Accept that the Team's velocity calculation may happen milliseconds after the sprint completes, not at the exact same instant.

5. **One aggregate per transaction**: A single use case should modify only one aggregate. If sprint completion needs to update both the Sprint and the Team, use a domain event to trigger the Team update in a separate transaction.

## Relationships to Other Topics

- [Entities](../entities/) -- Aggregate roots are entities; aggregates contain entities
- [Value Objects](../value-objects/) -- Aggregates contain value objects for immutable domain concepts
- [Domain Events](../domain-events/) -- Aggregates produce events to communicate changes to other aggregates
- [Repositories](../repositories/) -- One repository per aggregate root
- [Domain Services](../domain-services/) -- Coordinate operations across multiple aggregates
- [Bounded Contexts](../bounded-contexts/) -- Each bounded context contains its own aggregates

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 6: The Life Cycle of a Domain Object. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 10: Aggregates. Addison-Wesley, 2013.
- Vernon, Vaughn. "Domain-Driven Design Distilled." Chapter 5: Tactical Design with Aggregates. Addison-Wesley, 2016.
- Schwaber, Ken and Sutherland, Jeff. "The Scrum Guide." 2020.
- Cohn, Mike. "Agile Estimating and Planning." Prentice Hall, 2005.
