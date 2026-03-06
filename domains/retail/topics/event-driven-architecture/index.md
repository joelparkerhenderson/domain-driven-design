# Event-Driven Architecture

## Overview

Event-driven architecture (EDA) is an architectural pattern in which bounded contexts communicate through the production, detection, and consumption of domain events. In the retail domain, event-driven architecture enables loose coupling between contexts like Product Catalog, Pricing, Point of Sale, Customer and Loyalty, Inventory, and Omnichannel, allowing each to evolve independently while maintaining system-wide consistency through eventual consistency guarantees.

Retail systems are particularly well-suited to event-driven architecture because retail operations are inherently event-driven: a customer makes a purchase, a shipment arrives, a price changes, a promotion starts. These real-world events map naturally to domain events that flow through the system.

## Event Patterns in Retail

### Event Notification

The simplest pattern, where an event signals that something happened and consumers may react if interested. Example: the Product Catalog Context publishes a ProductUpdated event. The Omnichannel Context consumes it to update the online product display. The Inventory Context ignores it because product description changes do not affect stock levels.

### Event-Carried State Transfer

Events carry sufficient data for consumers to update their own local state without calling back to the source. Example: the TransactionCompleted event from the POS Context carries line items, tenders, and totals. The Inventory Context uses this data to decrement stock without querying the POS Context. The Customer and Loyalty Context uses it to accrue loyalty points without additional service calls.

### Event Sourcing

Instead of storing current state, the system stores the sequence of domain events that produced the current state. The current state is reconstructed by replaying events. Example: a Loyalty Account's point balance can be reconstructed from the sequence of PointsEarned, PointsRedeemed, and PointsExpired events. Event sourcing provides a complete audit trail, which is valuable for financial reconciliation and regulatory compliance in retail.

### CQRS with Events

Command Query Responsibility Segregation separates write operations (commands that produce events) from read operations (queries against optimized read models built from events). Example: the POS Context writes transactions as a stream of events, while reporting dashboards and sales analytics are built as read models populated by consuming those events. This separation allows the high-throughput write path (processing transactions) to operate independently from the analytical read path (generating sales reports).

## Retail Event Flows

**Sale Flow**: Customer scans items at POS -> POS requests price from Pricing Context -> POS completes transaction -> TransactionCompleted event is published -> Inventory Context decrements stock -> Customer and Loyalty Context accrues points -> Reporting systems update sales dashboards.

**Replenishment Flow**: Inventory Context detects stock below reorder point -> ReplenishmentTriggered event is published -> Purchase Order is generated -> Order is sent to supplier via EDI 850 -> Supplier ships merchandise with EDI 856 -> StockReceived event is published -> Stock position is updated.

**Omnichannel Order Flow**: Customer places order online -> OrderPlaced event is published -> FulfillmentRoutingService assigns fulfillment location -> Inventory Context allocates stock (StockAllocated event) -> Store associate picks order -> BOPISReadyForPickup event is published -> Customer is notified.

**Price Change Flow**: Merchandiser updates price -> PriceChanged event is published -> POS systems update local price cache -> Online storefront updates displayed prices -> Price comparison engines receive updated data.

## Infrastructure Considerations

Retail event-driven systems must handle significant volume variations. Black Friday sales may generate 10-100 times the normal event volume. Infrastructure must support elastic scaling, back-pressure handling, and graceful degradation.

Event ordering matters within certain partitions. Events for a single transaction must be processed in order. Events for a single product's inventory must be processed in order. But events across unrelated products or transactions can be processed in parallel. Partition keys (transaction ID, product ID, store ID) enable parallel processing while maintaining ordering guarantees within a partition.

Idempotent event processing is essential. Network issues and retries mean consumers may receive the same event multiple times. Consumers must handle duplicates gracefully, typically through deduplication using event identifiers.

## Relationships to Other Topics

- [Domain Events](../domain-events/index.md): The building blocks of event-driven architecture.
- [Bounded Contexts](../bounded-contexts/index.md): Events flow between bounded contexts.
- [Context Map](../context-map/index.md): Event flows are documented in the context map.
- [Integration Patterns](../integration-patterns/index.md): EDA is a key integration pattern.
- [Aggregates](../aggregates/index.md): Aggregates produce events.
- [Repositories](../repositories/index.md): Event-sourced repositories store events.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 8: Domain Events.
- Kleppmann, Martin. *Designing Data-Intensive Applications*. O'Reilly, 2017. Chapters 11-12: Stream Processing and the Future of Data Systems.
- Hohpe, Gregor, and Bobby Woolf. *Enterprise Integration Patterns*. Addison-Wesley, 2003.
- Young, Greg. "CQRS and Event Sourcing." CQRS Documents, 2010.
