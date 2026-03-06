# Repositories in Agile Management

## Overview

A repository is an abstraction that provides the illusion of an in-memory collection of aggregate roots. Repositories encapsulate the logic required to persist, retrieve, and search for aggregates, shielding the domain model from infrastructure concerns such as databases, file systems, or external APIs. Each aggregate root has exactly one repository. In agile management, repositories provide clean interfaces for storing and retrieving sprints, backlogs, teams, releases, and retrospectives without coupling domain logic to any specific storage technology.

## Relevance to Agile Management

Agile management systems must persist complex aggregates -- sprints with their backlog items and burndown snapshots, teams with their members and velocity history, retrospectives with their observations and action items. Repositories define the contract between the domain model and persistence, ensuring that domain logic remains pure and testable regardless of whether data is stored in a relational database, a document store, or an external tool like Jira.

## Key Concepts

### Collection-Oriented Repositories

Mimic an in-memory collection with add, remove, and query methods. The caller adds an aggregate to the repository and later retrieves it by identity or criteria. The repository handles all persistence mechanics transparently.

### Persistence-Oriented Repositories

Require explicit save operations. The caller retrieves an aggregate, modifies it, and calls save to persist the changes. This style is common when using ORM frameworks or event-sourced storage.

### One Repository Per Aggregate Root

Internal entities within an aggregate are persisted through the aggregate root's repository. There is no separate repository for SprintBacklogItem -- it is saved and loaded as part of the Sprint aggregate through SprintRepository.

## Agile Repositories

### SprintRepository

- **Aggregate Root**: Sprint
- **Bounded Context**: Sprint Execution Context
- **Core Operations**:
  - `findById(SprintID)` -- Retrieve a sprint with all its sprint backlog items, burndown snapshots, and impediments
  - `findActiveByTeam(TeamID)` -- Retrieve the currently active sprint for a team
  - `findCompletedByTeam(TeamID, dateRange)` -- Retrieve completed sprints for velocity calculation
  - `save(Sprint)` -- Persist the sprint aggregate including all internal entities and value objects
  - `findByStatus(SprintStatus)` -- Retrieve sprints in a given lifecycle state
- **Invariants Enforced**: Ensures only one sprint per team can be in Active status at any time
- **Query Support**: Supports filtering by team, date range, status, and sprint goal keywords

### BacklogRepository

- **Aggregate Root**: ProductBacklog
- **Bounded Context**: Backlog Management Context
- **Core Operations**:
  - `findById(ProductBacklogID)` -- Retrieve the full product backlog with all items, priorities, and estimates
  - `findReadyItems(ProductBacklogID)` -- Retrieve only items that meet the Definition of Ready
  - `findByEpic(EpicID)` -- Retrieve all backlog items associated with an epic
  - `save(ProductBacklog)` -- Persist the backlog aggregate including all items and their priority rankings
  - `findByPriorityRange(ProductBacklogID, topN)` -- Retrieve the top N prioritized items for sprint planning
- **Invariants Enforced**: Maintains unique priority rankings across all items within a backlog
- **Query Support**: Supports filtering by priority, estimation status, epic association, and readiness

### TeamRepository

- **Aggregate Root**: Team
- **Bounded Context**: Team & Capacity Context
- **Core Operations**:
  - `findById(TeamID)` -- Retrieve a team with all members, velocity history, and skill matrix
  - `findByMember(MemberID)` -- Retrieve all teams a member belongs to (for cross-team allocation checks)
  - `findActive()` -- Retrieve all teams in Active status
  - `save(Team)` -- Persist the team aggregate including member assignments and velocity snapshots
  - `findWithCapacityFor(sprintDateRange)` -- Retrieve teams that have available capacity for a given sprint period
- **Invariants Enforced**: Validates member allocation does not exceed available capacity across teams
- **Query Support**: Supports filtering by member, status, velocity range, and skill requirements

### ReleaseRepository

- **Aggregate Root**: Release
- **Bounded Context**: Release Management Context
- **Core Operations**:
  - `findById(ReleaseID)` -- Retrieve a release with all release items, feature flags, and deployment history
  - `findByVersion(SemanticVersion)` -- Retrieve a release by its version number
  - `findPending()` -- Retrieve releases in Planned or Candidate status
  - `save(Release)` -- Persist the release aggregate including feature flag states and rollback plans
  - `findDeployedTo(DeploymentTarget)` -- Retrieve the currently deployed release for a given target environment
- **Invariants Enforced**: Ensures semantic version uniqueness across all releases
- **Query Support**: Supports filtering by version, status, deployment target, and date range

### RetrospectiveRepository

- **Aggregate Root**: Retrospective
- **Bounded Context**: Continuous Improvement Context
- **Core Operations**:
  - `findById(RetrospectiveID)` -- Retrieve a retrospective with all observations and action items
  - `findBySprint(SprintID)` -- Retrieve the retrospective associated with a specific sprint
  - `findOpenActionItems(TeamID)` -- Retrieve all uncompleted action items across retrospectives for a team
  - `save(Retrospective)` -- Persist the retrospective aggregate including observations and action items
  - `findByDateRange(startDate, endDate)` -- Retrieve retrospectives within a time period for trend analysis
- **Invariants Enforced**: Ensures one retrospective per completed sprint
- **Query Support**: Supports filtering by sprint, team, action item status, and date range

## Repository Design Guidelines

1. **Keep repositories focused on aggregate roots**: Never create repositories for internal entities or value objects. SprintBacklogItem is persisted through SprintRepository, not through its own repository.

2. **Define repository interfaces in the domain layer**: The repository interface belongs to the domain; the implementation belongs to the infrastructure. This allows domain logic to be tested with in-memory implementations.

3. **Avoid business logic in repositories**: Repositories retrieve and persist aggregates. Business rules belong in the aggregate or in domain services, not in repository query logic.

4. **Support domain-meaningful queries**: Repository query methods should use domain language. Use `findReadyItems` rather than `findByStatusEqualsReady`. The repository translates domain queries into storage queries.

5. **Consider event sourcing**: For aggregates with rich audit requirements (common in regulated agile environments), event-sourced repositories store the sequence of domain events rather than current state, enabling full reconstruction of aggregate history.

## Relationships to Other Topics

- [Aggregates](../aggregates/) -- One repository per aggregate root; repositories persist entire aggregates
- [Entities](../entities/) -- Aggregate root entities are the objects repositories store and retrieve
- [Domain Events](../domain-events/) -- Event-sourced repositories store event streams instead of current state
- [Domain Services](../domain-services/) -- Domain services use repositories to load and save aggregates
- [Bounded Contexts](../bounded-contexts/) -- Each bounded context owns its repositories independently

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 6: The Life Cycle of a Domain Object -- Repositories. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 12: Repositories. Addison-Wesley, 2013.
- Schwaber, Ken and Sutherland, Jeff. "The Scrum Guide." 2020.
- Cohn, Mike. "Agile Estimating and Planning." Prentice Hall, 2005.
