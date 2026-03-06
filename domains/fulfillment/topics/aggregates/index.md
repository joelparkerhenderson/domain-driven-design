# Aggregates in Fulfillment

## Overview

An aggregate is a cluster of domain objects that are treated as a single unit for the purpose of data changes and consistency enforcement. Each aggregate has a root entity (the aggregate root) through which all external access is managed. The aggregate root enforces invariants -- business rules that must always be true -- across all objects within the aggregate boundary. In fulfillment, aggregates model the transactional boundaries of orders, shipments, inventory allocations, warehouse operations, and returns.

## Relevance to Fulfillment

Fulfillment data involves complex relationships: a fulfillment order has line items and allocations; a shipment contains packages with labels and tracking numbers; a pick wave contains pick lists assigned to zones. Aggregates define which objects must be consistent together in a single transaction and which can be eventually consistent across transactions.

## Key Concepts

### Aggregate Root

The single entity through which all modifications to the aggregate pass. External code never directly modifies objects inside the aggregate -- it always goes through the root.

### Invariants

Business rules that must hold at all times within the aggregate boundary. For example:

- A fulfillment order cannot be shipped if any line item is unallocated
- A shipment's total weight must equal the sum of its package weights
- A pick wave cannot be released if it contains zero pick lists

### Transactional Consistency

All changes within an aggregate are committed or rolled back as a single transaction. Changes between aggregates are eventually consistent, communicated via domain events.

## Fulfillment Aggregates

### FulfillmentOrder Aggregate

- **Root**: FulfillmentOrder (identified by FulfillmentOrderId)
- **Internal entities**: OrderLine
- **Internal value objects**: Address (shipping address), OrderPriority, ChannelSource
- **Invariants**:
  - FulfillmentOrderId is unique and immutable once assigned
  - Every order must have at least one order line
  - Shipping address must be validated before the order can proceed to allocation
  - Order cannot transition to "Shipped" unless all lines are fulfilled or explicitly cancelled
  - Order priority determines processing sequence and cannot be downgraded once upgraded
  - Total order value must equal the sum of line item values
- **Lifecycle states**: Received, Validated, Allocated, InPicking, Packed, Shipped, Delivered, Cancelled
- **Events produced**: OrderReceived, OrderValidated, OrderAllocated, OrderCancelled, OrderCompleted

### Shipment Aggregate

- **Root**: Shipment (identified by ShipmentId)
- **Internal entities**: Package
- **Internal value objects**: ShippingLabel, TrackingNumber, CarrierRate, ServiceLevel, Dimensions, Weight
- **Invariants**:
  - A shipment must have at least one package
  - Every package must have a shipping label before dispatch
  - Total shipment weight is the sum of package weights
  - Carrier and service level must be assigned before label generation
  - A dispatched shipment cannot have packages added or removed
  - Each package must have valid dimensions and weight for carrier compliance
- **Lifecycle states**: Created, Labeled, Staged, Dispatched, InTransit, Delivered, Returned
- **Events produced**: ShipmentCreated, ShipmentLabeled, ShipmentDispatched, ShipmentDelivered

### InventoryAllocation Aggregate

- **Root**: InventoryAllocation (identified by AllocationId)
- **Internal value objects**: AllocatedQuantity, SKU, WarehouseId, BinLocation
- **Invariants**:
  - Allocated quantity cannot exceed available quantity at the time of allocation
  - An allocation must reference a valid fulfillment order and SKU
  - Soft allocations expire after a configurable timeout period
  - Hard allocations can only be released by explicit cancellation or fulfillment completion
  - A single SKU-warehouse combination cannot have conflicting allocation states
- **Lifecycle states**: SoftAllocated, HardAllocated, PickConfirmed, Released, Expired
- **Events produced**: InventoryAllocated, AllocationConfirmed, AllocationReleased, AllocationExpired

### PickWave Aggregate

- **Root**: PickWave (identified by PickWaveId)
- **Internal entities**: PickList
- **Internal value objects**: WaveSchedule, ZoneAssignment, PickerAssignment
- **Invariants**:
  - A pick wave must contain at least one pick list
  - A pick wave cannot be released to the floor if any pick list references unavailable inventory
  - All pick lists within a wave must target the same warehouse
  - A wave's carrier cutoff time determines the latest completion time
  - Pick lists cannot be reassigned once picking has started
- **Lifecycle states**: Planned, Released, InProgress, Completed, Cancelled
- **Events produced**: WavePlanned, WaveReleased, PickStarted, PickCompleted, WaveCompleted

### ReturnRequest Aggregate

- **Root**: ReturnRequest (identified by ReturnRequestId)
- **Internal entities**: ReturnLineItem, Inspection
- **Internal value objects**: ReturnReason, ReturnLabel, Disposition, RefundAmount
- **Invariants**:
  - A return request must reference a valid delivered fulfillment order
  - Return line items cannot exceed the originally shipped quantities
  - Inspection must be completed before disposition can be assigned
  - Refund amount cannot exceed the original payment amount for the returned items
  - Disposition must be assigned to every inspected line item before the return is closed
- **Lifecycle states**: Requested, Authorized, InTransit, Received, Inspected, Resolved, Closed
- **Events produced**: ReturnRequested, ReturnAuthorized, ReturnReceived, ReturnInspected, ReturnResolved

## Aggregate Design Rules

1. **Keep aggregates small**: Prefer smaller aggregates with fewer internal objects. A FulfillmentOrder contains OrderLines but references Shipments by ShipmentId, not by containing Shipment objects.

2. **Reference other aggregates by identity**: A Shipment references a FulfillmentOrder by FulfillmentOrderId, not by containing the FulfillmentOrder object. A PickWave references InventoryAllocations by AllocationId.

3. **Use domain events for cross-aggregate coordination**: When an InventoryAllocated event occurs, the Order Processing context updates the FulfillmentOrder status -- but this happens asynchronously, not in the same transaction.

4. **Design for eventual consistency between aggregates**: Accept that the Shipping context may learn about a completed pick wave milliseconds after it completes, not at the exact same instant.

5. **One transaction per aggregate**: Never modify multiple aggregates in a single database transaction. If you need to update both a FulfillmentOrder and a Shipment, use domain events to coordinate.

## Relationships to Other Topics

- [Entities](../entities/) -- Aggregate roots are entities; aggregates contain entities
- [Value Objects](../value-objects/) -- Aggregates contain value objects for immutable domain concepts
- [Domain Events](../domain-events/) -- Aggregates produce events to communicate changes
- [Repositories](../repositories/) -- One repository per aggregate root
- [Domain Services](../domain-services/) -- Coordinate operations across multiple aggregates
- [Bounded Contexts](../bounded-contexts/) -- Each bounded context defines its own aggregates

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 6: The Life Cycle of a Domain Object. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 10: Aggregates. Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 10: Aggregates. O'Reilly, 2021.
- Vernon, Vaughn. "Domain-Driven Design Distilled." Chapter 5: Tactical Design with Aggregates. Addison-Wesley, 2016.
