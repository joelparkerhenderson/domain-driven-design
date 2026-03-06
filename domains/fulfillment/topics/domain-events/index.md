# Domain Events in Fulfillment

## Overview

A domain event represents a significant occurrence within the domain that domain experts care about. Domain events are named in past tense using the ubiquitous language (e.g., OrderReceived, ShipmentDispatched) and capture what happened, when it happened, and the relevant data at that moment. In fulfillment, domain events are the primary mechanism for communicating state changes across bounded contexts, enabling loose coupling between the stages of the order-to-delivery pipeline.

## Relevance to Fulfillment

Fulfillment is inherently a pipeline of events: an order is received, inventory is allocated, items are picked, packages are packed, shipments are dispatched, deliveries are attempted, and returns are processed. Each step produces events that trigger the next step. Domain events make this flow explicit in the model, enabling:

- Loose coupling between bounded contexts (Order Processing does not call Warehouse Operations directly)
- Audit trails for compliance and customer service
- Real-time status updates for customers and operations teams
- Retry and compensation patterns when steps fail
- Analytics and operational intelligence

## Event Structure

Every domain event should contain:

- **EventId**: Unique identifier for idempotency
- **EventType**: The event name (e.g., "OrderReceived")
- **OccurredAt**: Timestamp when the event occurred
- **AggregateId**: The identifier of the aggregate that produced the event
- **AggregateType**: The type of aggregate (e.g., "FulfillmentOrder")
- **Payload**: The event-specific data
- **CorrelationId**: Links related events across contexts for tracing
- **CausationId**: The EventId of the event that caused this event

## Fulfillment Domain Events

### Order Processing Events

**OrderReceived**

- Triggered when: A new order is accepted from a sales channel
- Payload: FulfillmentOrderId, ChannelOrderId, ChannelSource, OrderLines (SKU, quantity, price), ShippingAddress, RequestedServiceLevel, OrderPriority
- Consumed by: Inventory Allocation Context (to begin stock reservation)

**OrderValidated**

- Triggered when: An order passes all validation checks (address verified, items exist, payment confirmed)
- Payload: FulfillmentOrderId, ValidatedAddress, ValidationResults
- Consumed by: Inventory Allocation Context (to confirm allocation readiness)

**OrderSplit**

- Triggered when: A multi-warehouse order is split into separate fulfillment orders
- Payload: OriginalOrderId, SplitOrders (list of new FulfillmentOrderIds with their respective line items and assigned warehouses)
- Consumed by: Inventory Allocation Context, Warehouse Operations Context

**OrderCancelled**

- Triggered when: An order is cancelled before fulfillment completion
- Payload: FulfillmentOrderId, CancellationReason, CancelledLines
- Consumed by: Inventory Allocation Context (to release reserved stock), Shipping Context (to void labels if generated)

### Inventory Allocation Events

**InventoryAllocated**

- Triggered when: Stock is successfully reserved for an order
- Payload: AllocationId, FulfillmentOrderId, AllocatedItems (SKU, quantity, WarehouseId, BinLocation)
- Consumed by: Warehouse Operations Context (to create pick tasks)

**AllocationFailed**

- Triggered when: Insufficient stock is available to fulfill an order
- Payload: FulfillmentOrderId, FailedItems (SKU, requestedQuantity, availableQuantity), SuggestedActions (backorder, partial ship, cancel)
- Consumed by: Order Processing Context (to notify customer or place backorder)

**BackorderCreated**

- Triggered when: An order or order line is placed on backorder
- Payload: FulfillmentOrderId, BackorderedItems (SKU, quantity), EstimatedAvailabilityDate
- Consumed by: Order Processing Context (for customer notification)

**AllocationReleased**

- Triggered when: Previously allocated inventory is released back to available stock
- Payload: AllocationId, FulfillmentOrderId, ReleasedItems (SKU, quantity, WarehouseId)
- Consumed by: Inventory Allocation Context (to update available-to-sell quantities)

### Warehouse Operations Events

**PickStarted**

- Triggered when: A picker begins working on a pick list
- Payload: PickListId, PickWaveId, WarehouseId, PickerId, PickItems (SKU, quantity, BinLocation)
- Consumed by: Operations dashboards for real-time monitoring

**PickCompleted**

- Triggered when: A picker finishes all items on a pick list
- Payload: PickListId, PickWaveId, PickedItems (SKU, quantity, actualBinLocation), ShortPicks (SKU, expectedQuantity, actualQuantity)
- Consumed by: Pack station assignment, Inventory Allocation Context (for short pick reconciliation)

**PackCompleted**

- Triggered when: Items are packed into a shipping container
- Payload: PackageId, FulfillmentOrderId, ShipmentId, PackedItems (SKU, quantity), Weight, Dimensions, PackStationId, PackedBy
- Consumed by: Shipping & Carrier Context (to generate labels)

**ItemReceived**

- Triggered when: Inbound inventory is received at the warehouse
- Payload: ReceivingOrderId, WarehouseId, ReceivedItems (SKU, quantity, condition), PurchaseOrderId
- Consumed by: Inventory Allocation Context (to update stock levels), put-away process

### Shipping Events

**ShipmentCreated**

- Triggered when: A new shipment is created from packed packages
- Payload: ShipmentId, FulfillmentOrderId, Packages (PackageId, Weight, Dimensions), DestinationAddress, RequestedServiceLevel
- Consumed by: Carrier selection process

**ShipmentLabeled**

- Triggered when: Shipping labels are generated and applied to packages
- Payload: ShipmentId, CarrierId, ServiceLevel, TrackingNumber, LabelData, Packages (PackageId, TrackingNumber)
- Consumed by: Staging and dispatch process

**ShipmentDispatched**

- Triggered when: A shipment is handed off to the carrier
- Payload: ShipmentId, CarrierId, TrackingNumber, DispatchedAt, ManifestId, EstimatedDeliveryDate
- Consumed by: Delivery & Tracking Context, Order Processing Context (to update order status)

### Delivery Events

**DeliveryAttempted**

- Triggered when: A carrier attempts delivery at the destination
- Payload: ShipmentId, TrackingNumber, AttemptNumber, AttemptResult (delivered, not home, address issue), AttemptedAt, DriverNotes
- Consumed by: Customer notification service, exception handling

**DeliveryConfirmed**

- Triggered when: A package is successfully delivered
- Payload: ShipmentId, TrackingNumber, DeliveredAt, ProofOfDelivery (signature, photo, GPS coordinates), RecipientName
- Consumed by: Order Processing Context (to mark order as delivered), customer notification

**DeliveryFailed**

- Triggered when: All delivery attempts are exhausted or the package is undeliverable
- Payload: ShipmentId, TrackingNumber, FailureReason, TotalAttempts, ReturnToSenderInitiated
- Consumed by: Returns & Exceptions Context, customer service notification

### Returns Events

**ReturnRequested**

- Triggered when: A customer initiates a return
- Payload: ReturnRequestId, FulfillmentOrderId, ReturnItems (SKU, quantity, reason), CustomerComments
- Consumed by: Return authorization process

**ReturnAuthorized**

- Triggered when: A return request is approved and a return label is generated
- Payload: ReturnRequestId, ReturnLabel, ReturnInstructions, ExpirationDate
- Consumed by: Customer notification

**ReturnReceived**

- Triggered when: Returned items arrive at the warehouse
- Payload: ReturnRequestId, ReceivedItems (SKU, quantity, initialCondition), ReceivedAt, WarehouseId
- Consumed by: Inspection process

**ReturnInspected**

- Triggered when: Returned items have been inspected and disposition is assigned
- Payload: ReturnRequestId, InspectionResults (SKU, condition, disposition), RefundEligibility
- Consumed by: Refund processing, Inventory Allocation Context (for restocking)

**ReturnResolved**

- Triggered when: A return is fully processed (refund issued, exchange shipped, or case closed)
- Payload: ReturnRequestId, Resolution (refund, exchange, credit), RefundAmount, ExchangeOrderId
- Consumed by: Financial systems, customer notification

## Event Design Guidelines

1. **Name events in past tense**: They describe something that has already happened (OrderReceived, not ReceiveOrder).

2. **Include sufficient data**: Events should carry enough data for consumers to act without calling back to the producer.

3. **Events are immutable**: Once published, an event's data cannot change. If a correction is needed, publish a compensating event.

4. **Use correlation IDs**: Link all events in a fulfillment flow with a correlation ID (typically the FulfillmentOrderId) for end-to-end tracing.

5. **Design for idempotency**: Consumers must handle duplicate events gracefully using the EventId for deduplication.

## Relationships to Other Topics

- [Aggregates](../aggregates/) -- Aggregates produce domain events when state changes
- [Event-Driven Architecture](../event-driven-architecture/) -- The infrastructure that delivers domain events
- [Bounded Contexts](../bounded-contexts/) -- Events cross bounded context boundaries
- [Entities](../entities/) -- Entity state transitions produce events
- [Domain Services](../domain-services/) -- Services may produce events as outcomes of operations
- [Integration Patterns](../integration-patterns/) -- Events are the primary integration mechanism

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 8: Domain Events. Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 11: Domain Events. O'Reilly, 2021.
- Fowler, Martin. "Domain Event." martinfowler.com, 2005.
- Young, Greg. "CQRS and Event Sourcing." CQRS.nu.
