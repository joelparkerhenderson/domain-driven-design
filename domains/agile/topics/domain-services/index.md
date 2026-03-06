# Domain Services in Agile Management

## Overview

A domain service is a stateless operation that encapsulates domain logic which does not naturally belong to any single entity or value object. Domain services coordinate behavior across multiple aggregates, enforce business rules that span aggregate boundaries, and perform calculations that require data from several sources. In agile management, domain services handle cross-cutting operations such as velocity calculation (which spans Sprint and Team aggregates), capacity planning (which combines team availability with backlog estimates), and sprint planning (which coordinates between backlog prioritization and team capacity).

## Relevance to Agile Management

Many agile operations inherently span multiple aggregates. Calculating velocity requires completed sprint data and team configuration. Planning a sprint requires backlog priorities, team capacity, and historical velocity. Prioritizing a backlog may involve scoring models that reference market data, team capacity, and strategic objectives. These operations do not belong to any single aggregate root -- they orchestrate across aggregate boundaries while maintaining the domain's invariants.

## Key Concepts

### Statelessness

Domain services do not hold state between invocations. All required data is passed in as parameters or retrieved from repositories within the operation. The VelocityCalculationService does not remember previous calculations -- it computes velocity fresh each time from the data provided.

### Domain Logic, Not Infrastructure

Domain services contain pure business logic. They do not send emails, write to databases, or call external APIs. Those responsibilities belong to application services or infrastructure services. A domain service calculates velocity; an application service decides to persist the result and notify stakeholders.

### Named with Domain Language

Domain services are named using the ubiquitous language: VelocityCalculationService, not DataProcessingHelper. The name should communicate the business operation to a domain expert.

## Agile Domain Services

### VelocityCalculationService

- **Purpose**: Calculate a team's velocity from completed sprint data
- **Inputs**: TeamID, list of completed Sprint aggregates (or SprintCompleted events), calculation method (simple average, weighted average, rolling average), number of sprints to include
- **Outputs**: Velocity value object (pointsPerSprint, sprintCount, calculationMethod, confidenceInterval)
- **Business Rules**:
  - Only completed sprints contribute to velocity (cancelled or in-progress sprints are excluded)
  - Weighted average gives higher weight to recent sprints (common weighting: most recent sprint counts double)
  - Rolling average uses a sliding window (typically last 3-6 sprints)
  - Confidence interval is calculated from the standard deviation across included sprints
- **Aggregates Involved**: Team (consumer of the result), Sprint (source of completion data)
- **Events Produced**: VelocityCalculated
- **Bounded Contexts**: Operates across Sprint Execution and Team & Capacity contexts

### CapacityPlanningService

- **Purpose**: Determine a team's available capacity for an upcoming sprint
- **Inputs**: TeamID, sprint date range (TimeBox), team member availability data
- **Outputs**: Capacity value object (totalAvailablePoints, memberBreakdown, loadFactor)
- **Business Rules**:
  - Capacity equals the sum of individual member availability multiplied by team load factor
  - Load factor accounts for meetings, support duties, and other non-sprint work (typically 0.6-0.8)
  - Members on partial allocation contribute proportionally to their allocation percentage
  - Members on leave or holiday contribute zero capacity for affected days
  - Capacity cannot exceed historical velocity by more than a configurable threshold (guards against over-commitment)
- **Aggregates Involved**: Team (source of member data and velocity), Sprint (consumer of capacity for planning)
- **Bounded Contexts**: Operates primarily within Team & Capacity Context, consumed by Sprint Execution Context

### BacklogPrioritizationService

- **Purpose**: Apply a prioritization framework to rank backlog items
- **Inputs**: ProductBacklogID, prioritization method (RICE or MoSCoW), scoring criteria per item
- **Outputs**: Ordered list of backlog items with computed priority scores
- **Business Rules**:
  - RICE scoring: Score = (Reach x Impact x Confidence) / Effort
  - MoSCoW categorization: Items are classified as Must, Should, Could, or Won't, then rank-ordered within each category
  - Items with dependencies on blocked or unfinished items receive a priority penalty
  - The Product Owner retains final authority to override computed rankings
  - Re-prioritization preserves unique rank ordering (no two items share the same rank)
- **Aggregates Involved**: ProductBacklog (source and target of prioritization)
- **Bounded Contexts**: Operates within Backlog Management Context

### SprintPlanningService

- **Purpose**: Assist in selecting backlog items for a sprint based on capacity and priority
- **Inputs**: ProductBacklogID (ready items), TeamID, sprint TimeBox, Velocity, Capacity
- **Outputs**: Proposed sprint backlog (list of items fitting within capacity), SprintGoal recommendation, commitment confidence percentage
- **Business Rules**:
  - Items are selected in priority order until capacity is exhausted
  - The total committed story points must not exceed the lesser of velocity and capacity
  - Items that are not "ready" (lacking acceptance criteria or estimates) cannot be selected
  - Dependencies between items are respected (dependent items are either both included or neither included)
  - A buffer of 10-20% capacity is recommended to account for unplanned work
- **Aggregates Involved**: ProductBacklog (source of ready items), Team (source of velocity and capacity), Sprint (target for commitment)
- **Bounded Contexts**: Coordinates across Backlog Management, Team & Capacity, and Sprint Execution contexts

## Domain Service Design Guidelines

1. **Use services for cross-aggregate operations**: If an operation naturally belongs to a single aggregate, put it there. Use a domain service only when the operation spans multiple aggregates or does not fit any single aggregate.

2. **Keep services stateless**: Domain services should not maintain state between method calls. Pass all required data as parameters or retrieve it from repositories.

3. **Return value objects or domain events**: Domain services should return domain concepts (value objects, events), not raw primitives or infrastructure types.

4. **Name services with domain verbs**: Use names like VelocityCalculationService or CapacityPlanningService, not AgileUtilities or DataProcessor.

5. **Separate domain services from application services**: Domain services contain business logic. Application services orchestrate use cases, handle transactions, and coordinate infrastructure concerns.

## Relationships to Other Topics

- [Aggregates](../aggregates/) -- Domain services coordinate operations across aggregate boundaries
- [Value Objects](../value-objects/) -- Domain services consume and produce value objects
- [Domain Events](../domain-events/) -- Domain services may produce events as outcomes of their operations
- [Repositories](../repositories/) -- Domain services use repositories to retrieve aggregates
- [Bounded Contexts](../bounded-contexts/) -- Domain services may operate across bounded context boundaries

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 5: A Model Expressed in Software -- Services. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 7: Services. Addison-Wesley, 2013.
- Schwaber, Ken and Sutherland, Jeff. "The Scrum Guide." 2020.
- Cohn, Mike. "Agile Estimating and Planning." Chapters 16-18: Planning Iterations and Releases. Prentice Hall, 2005.
- Anderson, David. "Kanban: Successful Evolutionary Change for Your Technology Business." Blue Hole Press, 2010.
