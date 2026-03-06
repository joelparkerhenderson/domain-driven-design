# Domain Events in Agile Management

## Overview

A domain event is a record of something significant that happened in the domain. Domain events are named in the past tense because they represent facts that have already occurred: SprintStarted, StoryCompleted, BacklogRefined. They are immutable -- once an event has occurred, it cannot be changed or undone, only compensated for by subsequent events. In agile management, domain events enable loosely coupled communication between bounded contexts, allowing the Sprint Execution Context to notify the Team & Capacity Context that a sprint has completed without either context depending directly on the other.

## Relevance to Agile Management

Agile workflows are inherently event-driven. A sprint completes, triggering velocity recalculation and retrospective scheduling. A story is blocked, triggering impediment tracking and notification. A backlog is refined, triggering readiness updates for sprint planning. Modeling these occurrences as explicit domain events makes the system's behavior transparent, auditable, and extensible -- new reactions to existing events can be added without modifying the event source.

## Key Concepts

### Event Naming

Domain events use past-tense verb phrases that reflect the ubiquitous language: SprintStarted, not StartSprint. The name should be immediately understandable to a domain expert without technical explanation.

### Immutability

Once published, a domain event cannot be altered. If a SprintCompleted event was published in error (e.g., the sprint was reopened), a compensating event such as SprintReopened is published rather than deleting the original event.

### Event Payload

Each event carries the data necessary for consumers to react without querying back to the source. A StoryCompleted event includes the StoryID, SprintID, StoryPoints, and CompletionTimestamp so that the velocity calculation service can process it independently.

## Agile Domain Events

### SprintStarted

- **Source Aggregate**: Sprint (Sprint Execution Context)
- **Payload**: SprintID, TeamID, SprintGoal, StartDate, EndDate, CommittedStoryPoints
- **Consumers**: Team & Capacity Context (locks capacity allocation), Release Management Context (updates release timeline)
- **Trigger**: Sprint transitions from Planned to Active state

### SprintCompleted

- **Source Aggregate**: Sprint (Sprint Execution Context)
- **Payload**: SprintID, TeamID, CompletedStoryPoints, CommittedStoryPoints, SprintGoalMet (boolean), CompletionDate
- **Consumers**: Team & Capacity Context (triggers velocity recalculation), Continuous Improvement Context (schedules retrospective), Release Management Context (evaluates release candidate readiness)
- **Trigger**: Sprint transitions from InReview to Completed state

### StoryCompleted

- **Source Aggregate**: Sprint (Sprint Execution Context)
- **Payload**: StoryID, SprintID, StoryPoints, CompletionTimestamp, CycleTime
- **Consumers**: Backlog Management Context (updates epic progress), Continuous Improvement Context (records cycle time metric), Release Management Context (adds to release candidate)
- **Trigger**: A sprint backlog item meets the Definition of Done

### StoryBlocked

- **Source Aggregate**: Sprint (Sprint Execution Context)
- **Payload**: StoryID, SprintID, BlockerDescription, BlockedTimestamp, ReportedBy
- **Consumers**: Continuous Improvement Context (tracks impediment frequency), Team & Capacity Context (adjusts effective capacity)
- **Trigger**: A team member raises an impediment against a story

### BacklogRefined

- **Source Aggregate**: ProductBacklog (Backlog Management Context)
- **Payload**: BacklogID, RefinedItemIDs, NewlyReadyItemCount, SessionDate, ParticipantCount
- **Consumers**: Sprint Execution Context (updates pool of ready items for sprint planning), Team & Capacity Context (informs capacity planning)
- **Trigger**: A refinement session concludes and the backlog is updated

### RetroCompleted

- **Source Aggregate**: Retrospective (Continuous Improvement Context)
- **Payload**: RetrospectiveID, SprintID, ActionItemCount, ActionItemSummaries, CompletionDate
- **Consumers**: Sprint Execution Context (incorporates action items into next sprint planning), Team & Capacity Context (updates team health indicators)
- **Trigger**: Retrospective transitions from InProgress to Closed state

### ReleaseDeployed

- **Source Aggregate**: Release (Release Management Context)
- **Payload**: ReleaseID, Version, DeploymentTarget, DeployedTimestamp, FeatureFlagStates, IncludedStoryIDs
- **Consumers**: Continuous Improvement Context (records lead time for delivered stories), Backlog Management Context (closes delivered epics)
- **Trigger**: Release transitions from Deploying to Deployed state

### VelocityCalculated

- **Source Aggregate**: Team (Team & Capacity Context)
- **Payload**: TeamID, NewVelocity, SprintCount, CalculationMethod, CalculationTimestamp
- **Consumers**: Sprint Execution Context (updates planning baseline), Backlog Management Context (informs epic timeline forecasting)
- **Trigger**: Team aggregate recalculates velocity after a SprintCompleted event

## Event Design Guidelines

1. **Include sufficient context**: Events should carry enough data for consumers to act without calling back to the producer. Avoid events that contain only an ID and force consumers to query for details.

2. **Name events in domain language**: Use terms that domain experts recognize. BacklogRefined, not BacklogItemsUpdatedInBulk.

3. **Keep events focused**: Each event should represent one thing that happened. Do not combine SprintCompleted and VelocityCalculated into a single event -- they are separate occurrences.

4. **Version events explicitly**: As the domain evolves, event schemas change. Use explicit versioning (e.g., SprintCompleted_v2) to support backward compatibility.

5. **Timestamp every event**: Every event must include a precise timestamp for ordering, auditing, and metric calculation.

## Relationships to Other Topics

- [Aggregates](../aggregates/) -- Aggregates produce domain events when their state changes
- [Entities](../entities/) -- Entity lifecycle transitions are the primary source of domain events
- [Repositories](../repositories/) -- Event-sourced repositories can reconstruct aggregate state from event streams
- [Domain Services](../domain-services/) -- Domain services often react to events and coordinate cross-aggregate workflows
- [Event-Driven Architecture](../event-driven-architecture/) -- Defines the infrastructure patterns for publishing and consuming events
- [Bounded Contexts](../bounded-contexts/) -- Domain events are the primary communication mechanism between bounded contexts

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 8: Domain Events. Addison-Wesley, 2013.
- Schwaber, Ken and Sutherland, Jeff. "The Scrum Guide." 2020.
- Anderson, David. "Kanban: Successful Evolutionary Change for Your Technology Business." Blue Hole Press, 2010.
- Cohn, Mike. "Agile Estimating and Planning." Prentice Hall, 2005.
