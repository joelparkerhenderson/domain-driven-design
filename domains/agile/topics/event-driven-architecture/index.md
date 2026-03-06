# Event-Driven Architecture in Agile Management

## Overview

Event-driven architecture (EDA) is a design approach where bounded contexts communicate by producing and consuming domain events rather than through direct synchronous calls. In agile management, EDA enables the five bounded contexts (Backlog Management, Sprint Execution, Team & Capacity, Continuous Improvement, Release Management) to remain loosely coupled while staying coordinated. Each context publishes events when significant state changes occur and subscribes to events from other contexts that affect its own behavior.

## Relevance to Agile Management

Agile bounded contexts are inherently interconnected: sprint completion triggers velocity recalculation, retrospective scheduling, and release candidate evaluation. Without event-driven architecture, these interactions would require direct dependencies between contexts, creating a tightly coupled system that is difficult to evolve independently. EDA preserves context autonomy -- each context can be developed, deployed, and scaled independently while maintaining system-wide coherence through event flows.

## Event Flow Map

The following flows describe how domain events propagate across agile bounded contexts.

### Sprint Completion Flow

1. **SprintCompleted** is published by the Sprint Execution Context
2. Team & Capacity Context consumes it and triggers **VelocityCalculated**
3. Continuous Improvement Context consumes it and creates a Retrospective, producing **RetroStarted**
4. Release Management Context consumes it and evaluates whether a release candidate can be created

### Story Completion Flow

1. **StoryCompleted** is published by the Sprint Execution Context
2. Backlog Management Context consumes it and updates epic progress
3. Continuous Improvement Context consumes it and records the cycle time data point
4. Release Management Context consumes it and adds the story to the release candidate pool

### Backlog Refinement Flow

1. **BacklogRefined** is published by the Backlog Management Context
2. Sprint Execution Context consumes it and updates the pool of ready items available for sprint planning
3. Release Management Context consumes it and adjusts release scope forecasts

### Retrospective Completion Flow

1. **RetroCompleted** is published by the Continuous Improvement Context
2. Sprint Execution Context consumes it and incorporates action items into the next sprint planning
3. Team & Capacity Context consumes it and updates team health indicators

### Release Deployment Flow

1. **ReleaseDeployed** is published by the Release Management Context
2. Continuous Improvement Context consumes it and calculates lead time for all delivered stories
3. Backlog Management Context consumes it and closes delivered epics

### Velocity Calculation Flow

1. **VelocityCalculated** is published by the Team & Capacity Context
2. Sprint Execution Context consumes it and updates the planning baseline for sprint commitment
3. Backlog Management Context consumes it and recalculates epic delivery forecasts

## Event Delivery Patterns

### Publish-Subscribe

The primary pattern for inter-context communication. A context publishes an event to a message broker or event bus, and all interested contexts receive a copy. Publishers do not know or care which contexts subscribe. This enables adding new consumers without modifying the publisher.

### Event Store

A persistent log of all domain events in chronological order. Event stores enable event sourcing (reconstructing aggregate state from events), temporal queries (what was the state at a given point in time), and audit trails (who changed what and when). For agile management, an event store provides a complete history of every sprint, backlog change, and release.

### Eventual Consistency

Consumers process events asynchronously, meaning there is a brief delay between when an event is published and when all consumers have reacted. The Team's velocity may be recalculated milliseconds after the sprint completes, not at the exact same instant. The system is designed to tolerate this delay -- velocity does not need to be updated instantaneously for the system to function correctly.

### Idempotent Consumers

Consumers must handle receiving the same event more than once (due to network retries or redelivery). An idempotent VelocityCalculated handler produces the same result whether it processes the event once or three times.

## Event Schema Governance

### Versioning

Events evolve as the domain evolves. Schema changes must be backward-compatible: new fields can be added with defaults, but existing fields must not be removed or renamed without a major version change. Event versioning (e.g., SprintCompleted_v1, SprintCompleted_v2) allows producers and consumers to evolve at different rates.

### Contracts

Published event schemas serve as contracts between contexts. Changes to event schemas require coordination between producing and consuming contexts, typically managed through a schema registry or published language document.

## Relationships to Other Topics

- [Domain Events](../domain-events/) -- Domain events are the messages that flow through the event-driven architecture
- [Bounded Contexts](../bounded-contexts/) -- EDA is the primary integration mechanism between bounded contexts
- [Integration Patterns](../integration-patterns/) -- EDA complements other integration patterns such as anti-corruption layers and published language
- [Aggregates](../aggregates/) -- Aggregates produce the events that drive the architecture
- [Domain Services](../domain-services/) -- Domain services often react to incoming events and produce outgoing events

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 14: Maintaining Model Integrity. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 8: Domain Events. Addison-Wesley, 2013.
- Schwaber, Ken and Sutherland, Jeff. "The Scrum Guide." 2020.
- Anderson, David. "Kanban: Successful Evolutionary Change for Your Technology Business." Blue Hole Press, 2010.
- Richardson, Chris. "Microservices Patterns." Chapter 3: Interprocess Communication -- Using Asynchronous Messaging. Manning, 2018.
