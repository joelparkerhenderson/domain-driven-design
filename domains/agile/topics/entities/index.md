# Entities in Agile Management

## Overview

An entity is a domain object that is defined by its identity rather than its attributes. Entities persist over time, and their attributes may change while their identity remains constant. A Sprint entity, for example, remains the same sprint even as stories are added, completed, or removed -- it is identified by its SprintID, not by its current contents. Entities are the building blocks of aggregates and carry the core business logic of the domain.

## Relevance to Agile Management

Agile management is fundamentally about tracking things that change over time: sprints progress from planning to completion, stories move through workflow states, team members join and leave, and releases evolve from planning to deployment. Each of these concepts has a persistent identity that must be preserved across state changes, making them natural candidates for entity modeling.

## Key Concepts

### Identity

Every entity has a unique identifier that distinguishes it from all other entities of the same type, even if all other attributes are identical. Two sprints with the same duration, team, and goal are still different sprints if they have different SprintIDs.

### Lifecycle

Entities have a lifecycle -- they are created, they change state, and they may eventually reach a terminal state. The Sprint entity transitions through Draft, Planned, Active, InReview, and Completed states. Understanding and modeling these lifecycles is essential for correct domain behavior.

### Equality

Two entity references are equal if and only if they have the same identity. Attribute equality is irrelevant for entities (contrast with value objects, where equality is based entirely on attributes).

## Agile Entities

### Sprint

- **Identity**: SprintID (globally unique identifier)
- **Key Attributes**: SprintGoal, StartDate, EndDate, TimeBox, Status
- **Lifecycle**: Draft -> Planned -> Active -> InReview -> Completed (or Cancelled)
- **Behavior**:
  - Validates that sprint goal is set before transitioning to Active
  - Enforces time-box integrity (duration cannot change once active)
  - Tracks burndown by calculating remaining story points daily
  - Manages sprint scope changes with explicit approval
- **Bounded Context**: Sprint Execution Context
- **Aggregate Role**: Aggregate root of the Sprint Aggregate

### UserStory

- **Identity**: StoryID (globally unique identifier)
- **Key Attributes**: Title, Description, AcceptanceCriteria, StoryPoints, Priority, Status, AssignedTo
- **Lifecycle**: Draft -> Ready -> InSprint -> InProgress -> InReview -> Done (or Blocked, Deferred)
- **Behavior**:
  - Validates acceptance criteria are present before marking as "ready"
  - Transitions between workflow states with validation rules
  - Records estimation history for retrospective analysis
  - Tracks blocker associations and resolution
- **Bounded Context**: Appears differently in Backlog Management (as a requirements artifact) and Sprint Execution (as a work item)
- **Aggregate Role**: Internal entity within ProductBacklog Aggregate (backlog context) or SprintBacklogItem within Sprint Aggregate (sprint context)

### Epic

- **Identity**: EpicID (globally unique identifier)
- **Key Attributes**: Title, Description, ChildStories, Status, TargetRelease
- **Lifecycle**: Draft -> Active -> InProgress -> Done (or Cancelled)
- **Behavior**:
  - Decomposes into child user stories
  - Tracks completion percentage based on child story completion
  - Validates that at least one child story exists before marking as "refined"
  - Aggregates story points from child stories for sizing
- **Bounded Context**: Backlog Management Context
- **Aggregate Role**: Internal entity within ProductBacklog Aggregate, or may serve as its own aggregate root for large-scale planning

### Team

- **Identity**: TeamID (globally unique identifier)
- **Key Attributes**: Name, Members, Velocity, Capacity, Status
- **Lifecycle**: Forming -> Active -> Disbanded
- **Behavior**:
  - Manages membership (add, remove team members)
  - Calculates velocity from completed sprint history
  - Computes capacity for upcoming sprints based on member availability
  - Maintains skill matrix across all members
- **Bounded Context**: Team & Capacity Context
- **Aggregate Role**: Aggregate root of the Team Aggregate

### TeamMember

- **Identity**: MemberID (globally unique identifier)
- **Key Attributes**: Name, Role, Skills, Availability, TeamAssignments
- **Lifecycle**: Active -> OnLeave -> Active (or Departed)
- **Behavior**:
  - Tracks availability per sprint (accounting for holidays, PTO, partial allocation)
  - Maintains skill profile for task assignment optimization
  - Manages allocation across multiple teams (if applicable)
  - Records individual contribution metrics for capacity planning
- **Bounded Context**: Team & Capacity Context
- **Aggregate Role**: Internal entity within Team Aggregate

### Release

- **Identity**: ReleaseID (globally unique identifier)
- **Key Attributes**: Version, Status, PlannedDate, ActualDate, ReleaseItems, ReleaseNotes
- **Lifecycle**: Planned -> Candidate -> Deploying -> Deployed (or RolledBack)
- **Behavior**:
  - Manages release items (add, remove completed increments)
  - Validates all items meet Definition of Done before deployment
  - Coordinates deployment through pipeline stages
  - Manages feature flag states for the release
  - Generates release notes from completed stories
- **Bounded Context**: Release Management Context
- **Aggregate Role**: Aggregate root of the Release Aggregate

### Retrospective

- **Identity**: RetrospectiveID (globally unique identifier)
- **Key Attributes**: SprintID (reference), FacilitationFormat, Observations, ActionItems, Status
- **Lifecycle**: Open -> InProgress -> Closed
- **Behavior**:
  - Associates with a completed sprint
  - Collects observations (what went well, what could improve, ideas)
  - Generates action items from observations
  - Validates that at least one action item exists before closing
  - Tracks follow-up on previous retrospective action items
- **Bounded Context**: Continuous Improvement Context
- **Aggregate Role**: Aggregate root of the Retrospective Aggregate

### ActionItem

- **Identity**: ActionItemID (globally unique identifier)
- **Key Attributes**: Description, Assignee, TargetDate, Status, SourceRetrospectiveID
- **Lifecycle**: Open -> InProgress -> Completed (or Cancelled)
- **Behavior**:
  - Tracks progress toward completion
  - Escalates if target date passes without completion
  - Links back to the retrospective that created it
  - Reports completion status at subsequent retrospectives
- **Bounded Context**: Continuous Improvement Context
- **Aggregate Role**: Internal entity within Retrospective Aggregate

## Entity Design Guidelines

1. **Focus on identity and lifecycle**: Model entities when the object has a meaningful identity that persists across state changes. A sprint is an entity because "Sprint 42" is a specific sprint regardless of what stories it contains.

2. **Encapsulate state transitions**: State changes should be methods on the entity that validate invariants. `sprint.start()` should verify the sprint goal exists and capacity is committed, not just flip a status flag.

3. **Keep entities lean**: Entities should contain only the attributes and behavior relevant to their bounded context. The UserStory entity in the Backlog Management Context does not need sprint execution tracking attributes.

4. **Reference other aggregates by identity**: A Sprint references a Team by TeamID, not by containing the Team entity. This keeps aggregate boundaries clean and avoids transactional coupling.

5. **Distinguish entities from value objects**: If two objects with identical attributes are interchangeable, use a value object. If identity matters (e.g., two sprints with the same duration are still different sprints), use an entity.

## Relationships to Other Topics

- [Aggregates](../aggregates/) -- Entities serve as aggregate roots or internal entities within aggregates
- [Value Objects](../value-objects/) -- Entities contain value objects as attributes (e.g., Sprint contains SprintGoal as a value object)
- [Domain Events](../domain-events/) -- Entity state transitions produce domain events
- [Repositories](../repositories/) -- Repositories persist and retrieve aggregate root entities
- [Ubiquitous Language](../ubiquitous-language/) -- Entity names come directly from the ubiquitous language

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 5: A Model Expressed in Software -- Entities. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 5: Entities. Addison-Wesley, 2013.
- Schwaber, Ken and Sutherland, Jeff. "The Scrum Guide." 2020.
- Anderson, David. "Kanban: Successful Evolutionary Change for Your Technology Business." Blue Hole Press, 2010.
- Cohn, Mike. "Agile Estimating and Planning." Prentice Hall, 2005.
