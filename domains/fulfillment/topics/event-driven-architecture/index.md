# Event-Driven Architecture in Fulfillment

## Overview

Event-driven architecture (EDA) is a design approach in which bounded contexts communicate by producing and consuming domain events rather than making direct synchronous calls to each other. In fulfillment, EDA is the natural architectural pattern because the order-to-delivery lifecycle is inherently a sequence of events: an order is received, inventory is allocated, items are picked, packages are packed, shipments are dispatched, deliveries are tracked, and returns are processed. Each step produces events that trigger the next step, creating a loosely coupled, resilient pipeline.

## Relevance to Fulfillment

Fulfillment systems must coordinate six bounded contexts (Order Processing, Inventory Allocation, Warehouse Operations, Shipping & Carrier, Delivery & Tracking, Returns & Exceptions) without creating tight coupling between them. EDA enables each context to evolve independently, scale according to its own load patterns, and recover from failures without cascading effects across the pipeline. It also provides a natural audit trail and enables real-time operational visibility.

## Core Event Flow

The primary fulfillment event flow follows the order-to-delivery pipeline.

### Happy Path Flow

```
OrderReceived (Order Processing)
    -> InventoryAllocated (Inventory Allocation)
        -> WaveReleased (Warehouse Operations)
            -> PickCompleted (Warehouse Operations)
                -> PackCompleted (Warehouse Operations)
                    -> ShipmentLabeled (Shipping & Carrier)
                        -> ShipmentDispatched (Shipping & Carrier)
                            -> TrackingEventReceived (Delivery & Tracking)
                                -> DeliveryConfirmed (Delivery & Tracking)
```

### Exception Flows

**Allocation failure**:
```
OrderReceived -> AllocationFailed -> BackorderCreated -> (await stock) -> InventoryAllocated -> ...
```

**Short pick**:
```
PickStarted -> ShortPickReported -> AllocationReleased -> (reallocate) -> InventoryAllocated -> ...
```

**Delivery failure**:
```
ShipmentDispatched -> DeliveryAttemptFailed -> DeliveryFailed -> ReturnedToSender
```

**Customer return**:
```
DeliveryConfirmed -> ReturnRequested -> ReturnAuthorized -> ReturnReceived -> ReturnInspected -> ReturnResolved
```

## Event Design Principles

### Event Naming

Events are named in past tense using the ubiquitous language. They describe something that has already happened, not something that should happen. Use `OrderReceived`, not `ReceiveOrder` or `OrderToReceive`.

### Event Granularity

Events should represent a single, meaningful state change in the domain. Avoid overly coarse events (e.g., `OrderProcessed` that collapses multiple distinct steps) and overly fine events (e.g., `OrderFieldUpdated` that provides no business meaning).

### Event Payload Design

Each event carries sufficient data for consumers to act without calling back to the producer.

- Include the aggregate identity and the data relevant to the state change
- Do not include the entire aggregate state unless event sourcing requires it
- Include correlation and causation IDs for distributed tracing
- Include a timestamp for ordering and audit purposes

### Eventual Consistency

Bounded contexts achieve consistency through events, not through distributed transactions. When an InventoryAllocated event is published, the Warehouse Operations Context will process it within milliseconds to seconds, not instantaneously. The system design must tolerate this delay.

## Messaging Patterns

### Publish-Subscribe

A producer publishes an event to a topic; all interested consumers receive a copy. This is the primary pattern for cross-context communication in fulfillment.

- Order Processing publishes OrderReceived; Inventory Allocation subscribes
- Warehouse Operations publishes PackCompleted; Shipping & Carrier subscribes
- Delivery & Tracking publishes DeliveryConfirmed; Order Processing and Returns & Exceptions subscribe

### Event Sourcing

Rather than storing only the current state of an aggregate, store the complete sequence of events that produced the current state. The aggregate is reconstructed by replaying events.

- Provides a complete audit trail for every change to every aggregate
- Enables temporal queries ("What was the state of this order at 3pm yesterday?")
- Supports debugging and compliance by preserving the full history
- Adds complexity in event schema evolution and snapshot management

### Saga / Process Manager

Long-running business processes that span multiple bounded contexts are coordinated by a saga or process manager that listens for events and issues commands.

- **Order Fulfillment Saga**: Coordinates the flow from OrderReceived through DeliveryConfirmed, handling exceptions and compensating actions at each step
- **Return Processing Saga**: Coordinates from ReturnRequested through ReturnResolved, including inspection and refund steps

### Dead Letter and Retry

Events that cannot be processed are routed to a dead letter queue for investigation and manual replay. Consumers implement retry logic with exponential backoff for transient failures.

## Idempotency

Every event consumer must handle duplicate events gracefully. Network issues, consumer restarts, and infrastructure retries can all cause the same event to be delivered more than once.

- Use the EventId to detect and skip duplicate processing
- Design state transitions to be idempotent (applying the same event twice produces the same result)
- Maintain a processed-event log for deduplication verification

## Ordering Guarantees

Events within a single aggregate should be processed in order. Events across different aggregates may arrive in any order.

- Partition event streams by aggregate ID to maintain per-aggregate ordering
- Design consumers to tolerate out-of-order events across aggregates
- Use sequence numbers or vector clocks when strict cross-aggregate ordering is required

## Relationships to Other Topics

- [Domain Events](../domain-events/) -- EDA is the infrastructure that delivers domain events
- [Bounded Contexts](../bounded-contexts/) -- EDA decouples bounded contexts
- [Aggregates](../aggregates/) -- Aggregates produce the events that flow through the architecture
- [Integration Patterns](../integration-patterns/) -- EDA is the primary integration pattern between contexts
- [Domain Services](../domain-services/) -- Sagas and process managers coordinate domain services
- [Repositories](../repositories/) -- Event-sourced repositories store and replay event streams

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 8: Domain Events. Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 11: Domain Events. O'Reilly, 2021.
- Young, Greg. "CQRS and Event Sourcing." CQRS.nu.
- Hohpe, Gregor & Woolf, Bobby. "Enterprise Integration Patterns." Addison-Wesley, 2003.
- Richardson, Chris. "Microservices Patterns." Chapter 4: Managing Transactions with Sagas. Manning, 2018.
