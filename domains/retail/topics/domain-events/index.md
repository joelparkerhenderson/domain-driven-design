# Domain Events

## Overview

A domain event is a record of something significant that happened in the domain. Domain events are named in the past tense using the ubiquitous language: ProductCreated, PriceChanged, TransactionCompleted, PointsEarned, StockDepleted. In the retail domain, domain events are the primary mechanism for communication between bounded contexts, enabling loose coupling while maintaining eventual consistency across the system.

Domain events are immutable facts. Once a TransactionCompleted event is published, it cannot be changed or retracted. Other bounded contexts react to these events to update their own models, trigger workflows, or record audit trails.

## Retail Domain Events by Bounded Context

### Product Catalog Context (Published Events)

- **ProductCreated**: A new product has been added to the catalog with its initial attributes, categories, and variants.
- **ProductUpdated**: Product attributes, descriptions, or images have been modified.
- **ProductDiscontinued**: A product has been marked as no longer available for sale.
- **VariantAdded**: A new SKU variant (size, color) has been added to an existing product.
- **AssortmentChanged**: The product assortment for a store or channel has been modified.

### Pricing and Promotions Context (Published Events)

- **PriceChanged**: The regular or sale price for a product in a price zone has been updated.
- **MarkdownApplied**: A markdown has been applied to a product, reducing its price.
- **PromotionActivated**: A promotional campaign has started and its rules are now in effect.
- **PromotionExpired**: A promotional campaign has ended.
- **CouponRedeemed**: A coupon has been applied and consumed during a transaction.

### Point of Sale Context (Published Events)

- **TransactionCompleted**: A sale transaction has been successfully tendered and completed.
- **TransactionVoided**: A transaction has been voided before completion or post-voided after completion.
- **ReturnProcessed**: A merchandise return has been processed with refund tendered.
- **RegisterOpened**: A register has been opened for business by a cashier.
- **RegisterClosed**: A register has been closed and reconciled at end of shift or end of day.

### Customer and Loyalty Context (Published Events)

- **CustomerRegistered**: A new customer has been enrolled in the system.
- **LoyaltyPointsEarned**: Points have been accrued to a loyalty account from a qualifying transaction.
- **LoyaltyPointsRedeemed**: Points have been consumed from a loyalty account as payment.
- **TierChanged**: A loyalty member has progressed to a higher tier or been demoted to a lower tier.
- **CustomerPreferenceUpdated**: A customer has changed communication or privacy preferences.

### Inventory and Merchandising Context (Published Events)

- **StockReceived**: Merchandise has been received at a location from a supplier or transfer.
- **StockAllocated**: Stock has been reserved for a customer order or transfer.
- **StockDepleted**: A SKU's available-to-sell quantity at a location has reached zero.
- **ReplenishmentTriggered**: Stock levels have fallen below the reorder point, triggering a replenishment order.
- **ShrinkageRecorded**: An inventory discrepancy (theft, damage, administrative error) has been recorded.

### Omnichannel Context (Published Events)

- **OrderPlaced**: A customer has placed an order through a digital channel.
- **BOPISReadyForPickup**: A buy-online-pick-up-in-store order has been picked and is ready for customer collection.
- **ShipFromStoreDispatched**: A ship-from-store order has been packaged and handed to a carrier.
- **OrderCancelled**: A customer or system has cancelled an order before fulfillment.

## Event Design in Retail

Retail domain events should carry sufficient data for consumers to act without needing to call back to the source context. A TransactionCompleted event should include the store, register, transaction number, line items, tenders, and totals. This avoids tight coupling through synchronous queries.

Events should be versioned to support evolution over time. Adding fields to an event is generally backward-compatible. Removing or renaming fields requires a new event version and a migration strategy for consumers.

Retail systems must handle high event volumes, particularly during peak selling periods like Black Friday. Event infrastructure must support reliable delivery, ordering within a partition (e.g., per store or per product), and idempotent processing by consumers.

## Relationships to Other Topics

- [Aggregates](../aggregates/index.md): Aggregates emit domain events when their state changes.
- [Event-Driven Architecture](../event-driven-architecture/index.md): The architectural pattern that enables event-based communication.
- [Bounded Contexts](../bounded-contexts/index.md): Events flow between bounded contexts.
- [Context Map](../context-map/index.md): Event flows are part of the context map.
- [Repositories](../repositories/index.md): Repositories may store events in event-sourced systems.
- [Integration Patterns](../integration-patterns/index.md): Events are a key integration mechanism.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 14: Maintaining Model Integrity.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 8: Domain Events.
- Kleppmann, Martin. *Designing Data-Intensive Applications*. O'Reilly, 2017. Chapter 11: Stream Processing.
