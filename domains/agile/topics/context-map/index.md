# Context Map in Agile Management

## Overview

A context map is a visual and conceptual tool that shows how bounded contexts relate to one another, documenting the integration points, data flows, and organizational relationships between teams. In agile management, the context map reveals how backlog decisions flow into sprint planning, how sprint execution feeds velocity data back to capacity planning, and how completed work triggers release processes.

## Relevance to Agile Management

Agile processes are inherently interconnected: a Product Owner refines stories in the Backlog Management Context, those stories flow into the Sprint Execution Context during planning, sprint velocity feeds back into the Team & Capacity Context for future planning, retrospective insights from the Continuous Improvement Context influence how all other contexts evolve, and completed increments flow into the Release Management Context. Understanding these relationships is critical for designing a system that supports agile workflows without creating tight coupling.

## Context Relationship Patterns

### Customer-Supplier Relationships

In a customer-supplier relationship, the upstream context provides data or services that the downstream context depends on. The upstream team has some obligation to accommodate the downstream team's needs.

**Backlog Management (Upstream) -> Sprint Execution (Downstream)**

- The Backlog Management Context supplies prioritized, refined stories to the Sprint Execution Context
- Sprint Planning pulls from the product backlog based on team capacity and priority
- The Sprint Execution Context depends on stories being "ready" (meeting Definition of Ready)
- The Product Owner (upstream) negotiates sprint scope with the delivery team (downstream)

**Sprint Execution (Upstream) -> Team & Capacity (Downstream)**

- Sprint completion data (velocity, completed points) flows into the Team & Capacity Context
- The Team & Capacity Context depends on accurate sprint execution data for velocity calculation and future capacity planning
- Historical sprint data is the primary input for velocity prediction

**Sprint Execution (Upstream) -> Continuous Improvement (Downstream)**

- Sprint execution data (blockers, cycle times, completion rates) feeds retrospective analysis
- The Continuous Improvement Context consumes sprint metrics to identify patterns and improvement opportunities
- Sprint outcomes trigger retrospective ceremonies

### Conformist Relationships

**Team & Capacity (Upstream) -> Sprint Execution (Downstream)**

- Sprint planning must conform to the capacity constraints defined by the Team & Capacity Context
- The Sprint Execution Context accepts the team's calculated capacity as a hard constraint
- Sprint commitment is bounded by velocity and availability data from the Team & Capacity Context

### Partnership Relationships

**Sprint Execution <-> Release Management**

- These contexts operate as partners: sprint completion triggers release candidate creation, while release scheduling influences sprint planning priorities
- Both teams must coordinate closely, especially for release trains or fixed release cadences
- Deployment feedback (success, failure, rollback) flows back to sprint tracking

**Continuous Improvement <-> All Contexts**

- The Continuous Improvement Context has a partnership relationship with every other context
- Retrospective insights can change how any context operates
- Metrics are gathered from all contexts but improvements are applied context-specifically

## Integration Points

### Backlog Management -> Sprint Execution

- **Trigger**: Sprint Planning ceremony
- **Data Flow**: Prioritized backlog items with estimates, acceptance criteria, and priority
- **Translation**: Backlog items become Sprint Backlog Items with sprint-specific attributes (assignment, task breakdown)
- **Events**: BacklogItemSelected, SprintBacklogCreated

### Sprint Execution -> Team & Capacity

- **Trigger**: Sprint completion
- **Data Flow**: Completed story points, sprint duration, team composition during sprint
- **Translation**: Sprint results become velocity data points and capacity utilization metrics
- **Events**: SprintCompleted, VelocityCalculated

### Sprint Execution -> Continuous Improvement

- **Trigger**: Sprint retrospective ceremony
- **Data Flow**: Sprint metrics (burndown, impediments, completion rate), team observations
- **Translation**: Raw sprint data becomes improvement metrics and retrospective inputs
- **Events**: SprintRetrospected, MetricsComputed

### Sprint Execution -> Release Management

- **Trigger**: Increment completion
- **Data Flow**: Completed, tested, integrated features meeting Definition of Done
- **Translation**: Sprint increments become release candidates with deployment metadata
- **Events**: IncrementCompleted, ReleaseCandidateCreated

### Team & Capacity -> Sprint Execution

- **Trigger**: Sprint planning
- **Data Flow**: Team velocity, member availability, capacity for upcoming sprint
- **Translation**: Capacity data becomes sprint commitment constraints
- **Events**: CapacityCalculated, AvailabilityUpdated

### Continuous Improvement -> All Contexts

- **Trigger**: Action item implementation
- **Data Flow**: Process changes, updated definitions (DoD, DoR), workflow modifications
- **Translation**: Retrospective action items become configuration changes or process updates
- **Events**: ProcessUpdated, DefinitionOfDoneRevised

## Visual Context Map

```
+---------------------+     Customer-Supplier     +---------------------+
| Backlog Management  | -----------------------> | Sprint Execution    |
| Context             |    (prioritized stories)  | Context             |
+---------------------+                           +---------------------+
                                                    |    |    |    ^
                                                    |    |    |    |
                                        Upstream    |    |    |    | Conformist
                                                    |    |    |    |
                                                    v    |    |    |
                                  +---------------------+    |    |
                                  | Team & Capacity     |----+    |
                                  | Context             |         |
                                  +---------------------+         |
                                                    |             |
                                          Upstream  |             |
                                                    v             |
                                  +---------------------+         |
                                  | Continuous          |---------+
                                  | Improvement Context | Partnership
                                  +---------------------+
                                                                  |
                              Partnership                         |
+---------------------+                           +---------------------+
| Release Management  | <------------------------ | Sprint Execution    |
| Context             |    (completed increments)  | Context             |
+---------------------+                           +---------------------+
```

## Relationships to Other Topics

- [Strategic Design](../strategic-design/) -- Context maps are a key output of strategic design
- [Bounded Contexts](../bounded-contexts/) -- The contexts being mapped
- [Ubiquitous Language](../ubiquitous-language/) -- Context maps reveal where language translation is needed
- [Anti-Corruption Layer](../anti-corruption-layer/) -- ACLs are placed at integration points identified by the context map
- [Event-Driven Architecture](../event-driven-architecture/) -- Domain events flow along the paths defined in the context map
- [Integration Patterns](../integration-patterns/) -- Implementation of the integration points in the context map

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 14: Maintaining Model Integrity. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 3: Context Maps. Addison-Wesley, 2013.
- Vernon, Vaughn. "Domain-Driven Design Distilled." Chapter 4: Strategic Design with Context Mapping. Addison-Wesley, 2016.
- Brandolini, Alberto. "Introducing EventStorming." Leanpub, 2021.
- Schwaber, Ken and Sutherland, Jeff. "The Scrum Guide." 2020.
