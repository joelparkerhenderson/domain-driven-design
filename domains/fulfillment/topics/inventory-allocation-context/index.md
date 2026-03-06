# Inventory Allocation Context in Fulfillment

## Overview

The Inventory Allocation Context is the bounded context responsible for reserving stock against customer orders, maintaining real-time inventory availability, managing backorders, and coordinating multi-warehouse stock distribution. It answers the fundamental fulfillment question: "Is the requested product available, and where should it be fulfilled from?" This context bridges the gap between the Order Processing Context (which knows what the customer wants) and the Warehouse Operations Context (which knows what is physically on the shelves).

## Relevance to Fulfillment

Inventory allocation is one of the most operationally critical functions in fulfillment. Incorrect allocation leads to overselling (promising stock that does not exist), underselling (failing to offer stock that is available), split shipments (shipping from multiple warehouses when one would suffice), and delayed fulfillment (waiting for stock that is allocated elsewhere). The Inventory Allocation Context manages these risks by maintaining an accurate, real-time view of available-to-sell (ATS) inventory and enforcing allocation rules.

## Bounded Context Boundaries

**Owns**: InventoryAllocation aggregate, stock availability calculations, backorder queue, allocation rules engine

**Does not own**: Physical warehouse locations and bin management (Warehouse Operations Context), order intake and validation (Order Processing Context), purchasing and replenishment (upstream Procurement context)

**Upstream contexts**: Order Processing Context, Warehouse Operations Context (receiving events), Procurement (purchase order data)

**Downstream contexts**: Warehouse Operations Context (pick task creation), Order Processing Context (backorder notifications)

## Core Capabilities

### Stock Reservation

Stock reservation is the process of setting aside inventory for a specific order, ensuring that the same units are not promised to multiple customers.

- **Soft allocation**: A temporary, time-limited reservation that holds stock while downstream processes (payment capture, validation) complete. Soft allocations expire automatically after a configurable timeout.
- **Hard allocation**: A confirmed reservation that commits stock to an order. Hard allocations persist until the order is fulfilled, cancelled, or explicitly released.
- **Allocation confirmation**: Transition from soft to hard allocation when all preconditions are met.

### Availability Calculation

The context maintains real-time available-to-sell (ATS) quantities for every SKU at every warehouse.

- **On-hand quantity**: Physical stock present in the warehouse
- **Allocated quantity**: Stock reserved for orders (soft and hard allocations combined)
- **In-transit quantity**: Stock in transit from suppliers or transfers between warehouses
- **Available-to-sell**: On-hand minus allocated, optionally plus in-transit with a confidence factor
- **Safety stock**: Minimum quantity held in reserve, excluded from ATS calculations

### Backorder Management

When stock is insufficient to fulfill an order, the backorder process manages the waiting queue.

- Create backorder records for unfulfillable line items with estimated availability dates
- Prioritize backorder queue by order date, customer priority, and service level
- Automatically allocate incoming stock to backorders based on priority
- Notify the Order Processing Context when backorder status changes

### Multi-Warehouse Allocation

For fulfillment networks with multiple warehouses, allocation decisions consider the full network.

- Evaluate stock availability across all warehouses simultaneously
- Prefer single-warehouse fulfillment to minimize shipment count and cost
- When splitting is necessary, minimize the number of warehouses involved
- Factor in warehouse proximity to the shipping address for transit time optimization
- Consider warehouse workload and capacity constraints to balance throughput

## Key Aggregates and Entities

- **InventoryAllocation** (aggregate root): Represents a stock reservation linking an order line to a specific SKU, quantity, and warehouse
- **SKU** (value object): Stock keeping unit identifier
- **AllocatedQuantity** (value object): Quantity reserved with allocation type (soft/hard)
- **WarehouseId** (value object): Reference to the fulfilling warehouse
- **BinLocation** (value object): Specific storage location within the warehouse

## Domain Events Produced

- **InventoryAllocated**: Stock has been successfully reserved for an order
- **AllocationFailed**: Insufficient stock to fulfill one or more order lines
- **AllocationReleased**: Previously reserved stock has been returned to available inventory
- **AllocationExpired**: A soft allocation has timed out and stock is returned to ATS
- **BackorderCreated**: An order line has been placed on backorder
- **BackorderFulfilled**: Incoming stock has been allocated to a backorder

## Domain Events Consumed

- **OrderReceived** (from Order Processing Context): Triggers allocation attempt
- **OrderCancelled** (from Order Processing Context): Triggers allocation release
- **ItemReceived** (from Warehouse Operations Context): Updates on-hand quantities and may fulfill backorders
- **PickCompleted** (from Warehouse Operations Context): Confirms allocated stock has been physically picked
- **ReturnInspected** (from Returns & Exceptions Context): Restocked items increase on-hand quantities

## Concurrency and Consistency

Inventory allocation is a high-contention operation where multiple orders may compete for the same stock simultaneously. The context must address:

- **Optimistic locking**: Detect and retry conflicting allocation attempts using version numbers on inventory records
- **Idempotent operations**: Ensure that retried allocation requests do not double-allocate
- **Eventual consistency**: Accept that ATS quantities reported to upstream systems may lag by milliseconds; design for this tolerance
- **Atomic allocation**: A single order's allocation across multiple line items should succeed or fail as a unit

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) -- Inventory Allocation is one of the six core bounded contexts
- [Order Processing Context](../order-processing-context/) -- Upstream context that submits orders for allocation
- [Warehouse Operations Context](../warehouse-operations-context/) -- Downstream context that picks allocated stock
- [Domain Events](../domain-events/) -- Produces InventoryAllocated, AllocationFailed, BackorderCreated events
- [Aggregates](../aggregates/) -- Owns the InventoryAllocation aggregate
- [Domain Services](../domain-services/) -- AllocationService coordinates multi-warehouse allocation logic

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 14: Maintaining Model Integrity. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 10: Aggregates. Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 7: Modeling Bounded Contexts. O'Reilly, 2021.
- Kleppmann, Martin. "Designing Data-Intensive Applications." Chapter 7: Transactions. O'Reilly, 2017.
