# Event-Driven Architecture in the Sales Domain

## Overview

Event-driven architecture (EDA) is the communication pattern used to connect bounded contexts
in the Sales domain through domain events. Rather than bounded contexts calling each other
directly, each context publishes events when significant business occurrences happen, and
interested contexts subscribe to and react to those events. This architectural style enables
loose coupling between contexts, allows each context to evolve independently, and provides a
natural audit trail of everything that happens in the sales process.

The Sales domain is inherently event-driven. The sales lifecycle is a sequence of business
events: leads are captured, scored, and qualified; opportunities are created, negotiated, and
closed; orders are placed and fulfilled; accounts are managed and renewed. Event-driven
architecture mirrors this reality in the technical architecture.

## Key Concepts

**Event Publishing**: Each bounded context publishes domain events when business-significant
state changes occur. The Opportunity Management Context publishes DealClosedWon when a deal
is successfully closed. Publishers do not know or care which contexts consume their events,
maintaining loose coupling.

**Event Subscription**: Bounded contexts subscribe to events from other contexts that are
relevant to their responsibilities. The Order Management Context subscribes to DealClosedWon
events from Opportunity Management to trigger order creation. The Sales Analytics Context
subscribes to events from all other contexts to maintain its reporting data.

**Event Bus/Broker**: A messaging infrastructure component that routes events from publishers
to subscribers. The event bus decouples the timing and location of event production from
event consumption. In the Sales domain, the event bus handles events like LeadQualified,
OpportunityStageChanged, OrderConfirmed, and ContractRenewed.

**Eventual Consistency**: Event-driven communication introduces eventual consistency between
bounded contexts. When a deal is closed in Opportunity Management, the corresponding order
in Order Management and the updated account in Account Management will be consistent
eventually, not immediately. The Sales domain's business processes tolerate this delay
because cross-context consistency does not need to be instantaneous.

**Event Ordering and Idempotency**: The Sales domain requires that events for a given
aggregate are processed in order (a DealClosedWon cannot be processed before the
corresponding OpportunityCreated). Event consumers must handle duplicate deliveries
idempotently to ensure correctness under retry scenarios.

**At-Least-Once Delivery**: The messaging infrastructure guarantees at-least-once delivery
of events. Combined with idempotent consumers, this ensures no events are lost even in the
presence of transient failures.

## Sales Domain Event Flows

**Lead-to-Opportunity Flow**: Lead Generation publishes LeadQualified. Opportunity Management
consumes it to create a new opportunity. Sales Analytics consumes it to update funnel
conversion metrics.

**Deal Closure Flow**: Opportunity Management publishes DealClosedWon. Order Management
consumes it to create an order. Account Management consumes it to update the account record.
Sales Pipeline consumes it to update forecasts. Sales Analytics consumes it to update
revenue metrics.

**Order Fulfillment Flow**: Order Management publishes FulfillmentHandoffRequested. The
external Fulfillment domain consumes it through an anti-corruption layer. When fulfillment
acknowledges receipt, FulfillmentHandoffCompleted is published back.

**Contract Renewal Flow**: Account Management publishes ContractRenewed. Sales Analytics
consumes it to update retention metrics. Opportunity Management may consume it if the
renewal involves upsell opportunities.

**Alert Flow**: Sales Pipeline publishes VelocityAlertTriggered when sales velocity drops
below threshold. Sales Analytics consumes it for trend tracking. Managers are notified
for corrective action.

## Relationship to Other Topics

- [Domain Events](../domain-events/) define the events flowing through the architecture.
- [Context Map](../context-map/) documents which contexts publish and subscribe to which events.
- [Integration Patterns](../integration-patterns/) describe relationship types between contexts.
- [Bounded Contexts](../bounded-contexts/) are the publishers and subscribers.
- [Sales Analytics Context](../sales-analytics-context/) is the primary consumer of events.
- [Anti-Corruption Layer](../anti-corruption-layer/) translates events for external systems.

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software."
  Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
  Chapter 8: Domain Events.
- Khononov, Vlad. "Learning Domain-Driven Design." O'Reilly Media, 2021.
  Chapter 9: Communication Patterns.
- Hohpe, Gregor and Woolf, Bobby. "Enterprise Integration Patterns."
  Addison-Wesley, 2003.
- Stopford, Ben. "Designing Event-Driven Systems." O'Reilly Media, 2018.
- Bellemare, Adam. "Building Event-Driven Microservices." O'Reilly Media, 2020.
