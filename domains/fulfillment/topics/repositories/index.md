# Repositories in Fulfillment

## Overview

A repository is a mechanism for encapsulating storage, retrieval, and search behavior for aggregates. It provides a collection-like interface for accessing domain objects, hiding the details of the underlying persistence technology. Each aggregate root has exactly one repository. In fulfillment, repositories provide the abstraction layer between the domain model and the database, enabling the domain logic to remain independent of storage concerns.

## Relevance to Fulfillment

Fulfillment systems manage large volumes of operational data with varying access patterns. FulfillmentOrders are queried by status and warehouse; Shipments are looked up by tracking number; Inventory is queried by SKU and warehouse location. Repositories encapsulate these access patterns, providing domain-centric query interfaces while allowing the persistence layer to optimize for performance, scalability, and reliability.

## Key Concepts

### One Repository per Aggregate Root

Repositories are defined at the aggregate root level, not for individual entities or value objects within an aggregate. To access a Package, you go through the ShipmentRepository, which loads the Shipment aggregate including its Packages.

### Collection-Like Interface

A repository presents itself as an in-memory collection of aggregates. It supports adding, removing, and querying aggregates without exposing SQL, API calls, or storage details.

### Persistence Ignorance

The domain model does not know or care how data is stored. The same domain model could be backed by a relational database, a document store, or an event store. The repository interface is defined in the domain layer; the implementation resides in the infrastructure layer.

## Fulfillment Repositories

### FulfillmentOrderRepository

**Aggregate**: FulfillmentOrder (with OrderLines)

**Core operations**:

- `save(fulfillmentOrder)`: Persist a new or updated FulfillmentOrder
- `findById(fulfillmentOrderId)`: Retrieve a FulfillmentOrder by its identity
- `findByChannelOrderId(channelOrderId)`: Retrieve by the original sales channel order ID
- `findByStatus(status, warehouseId)`: Retrieve orders in a given status for a specific warehouse
- `findPendingAllocation(warehouseId)`: Retrieve orders awaiting inventory allocation
- `findByDateRange(startDate, endDate)`: Retrieve orders created within a date range
- `findByPriority(priority, status)`: Retrieve orders by priority and status for wave planning
- `delete(fulfillmentOrderId)`: Remove a cancelled order (or mark as archived)

**Query considerations**:

- High read volume for status-based queries (operations dashboards)
- Indexed by status, warehouseId, priority, and createdAt for efficient retrieval
- Pagination required for large result sets

### ShipmentRepository

**Aggregate**: Shipment (with Packages, ShippingLabels, TrackingNumbers)

**Core operations**:

- `save(shipment)`: Persist a new or updated Shipment
- `findById(shipmentId)`: Retrieve a Shipment by its identity
- `findByTrackingNumber(trackingNumber)`: Retrieve by carrier-assigned tracking number
- `findByFulfillmentOrderId(fulfillmentOrderId)`: Retrieve all shipments for a given order
- `findByStatus(status)`: Retrieve shipments in a given status
- `findByCarrierAndDispatchDate(carrierId, date)`: Retrieve shipments for manifest generation
- `findPendingDispatch(warehouseId)`: Retrieve labeled shipments awaiting carrier pickup
- `findByEstimatedDeliveryDate(date)`: Retrieve shipments expected to deliver on a given date

**Query considerations**:

- Tracking number lookups must be fast (customer-facing)
- Carrier-based queries support manifest and reconciliation workflows
- Status-based queries support staging and dispatch operations

### InventoryRepository

**Aggregate**: InventoryAllocation

**Core operations**:

- `save(allocation)`: Persist a new or updated InventoryAllocation
- `findById(allocationId)`: Retrieve an allocation by its identity
- `findByFulfillmentOrderId(fulfillmentOrderId)`: Retrieve all allocations for an order
- `findBySku(sku, warehouseId)`: Retrieve allocations for a SKU at a specific warehouse
- `findAvailableQuantity(sku, warehouseId)`: Query current available-to-sell quantity
- `findExpiredSoftAllocations()`: Retrieve soft allocations that have exceeded their timeout
- `findByStatus(status, warehouseId)`: Retrieve allocations in a given state

**Query considerations**:

- Available quantity queries must be highly performant (called during order acceptance)
- Concurrent allocation requires optimistic or pessimistic locking strategies
- Expiration queries run on a scheduled basis

### WarehouseRepository

**Aggregate**: Warehouse (treated as a reference entity, not a full aggregate in most cases)

**Core operations**:

- `save(warehouse)`: Persist a new or updated Warehouse
- `findById(warehouseId)`: Retrieve a Warehouse by its identity
- `findActive()`: Retrieve all active warehouses
- `findByRegion(region)`: Retrieve warehouses in a geographic region
- `findByCarrierSupport(carrierId)`: Retrieve warehouses that support a specific carrier
- `findNearestTo(address)`: Retrieve warehouses sorted by proximity to a shipping address

**Query considerations**:

- Warehouse data changes infrequently; aggressive caching is appropriate
- Proximity queries may require geospatial indexing

### ReturnRepository

**Aggregate**: ReturnRequest (with ReturnLineItems, Inspections)

**Core operations**:

- `save(returnRequest)`: Persist a new or updated ReturnRequest
- `findById(returnRequestId)`: Retrieve a ReturnRequest by its identity
- `findByFulfillmentOrderId(fulfillmentOrderId)`: Retrieve all returns for a given order
- `findByStatus(status)`: Retrieve returns in a given status (e.g., awaiting inspection)
- `findPendingInspection(warehouseId)`: Retrieve returns received but not yet inspected
- `findByDateRange(startDate, endDate)`: Retrieve returns within a date range
- `findByDisposition(disposition)`: Retrieve returns by disposition type for bulk processing

**Query considerations**:

- Inspection queues are frequently queried by warehouse staff
- Return rate analytics require date-range queries

### PickWaveRepository

**Aggregate**: PickWave (with PickLists)

**Core operations**:

- `save(pickWave)`: Persist a new or updated PickWave
- `findById(pickWaveId)`: Retrieve a PickWave by its identity
- `findByStatus(status, warehouseId)`: Retrieve waves in a given status for a warehouse
- `findActiveWaves(warehouseId)`: Retrieve currently executing waves
- `findByCarrierCutoff(carrierId, cutoffTime)`: Retrieve waves targeting a specific carrier cutoff

**Query considerations**:

- Active wave queries support real-time warehouse monitoring
- Cutoff-based queries support priority wave scheduling

## Repository Implementation Patterns

### Unit of Work

Track all changes to aggregates within a business operation and commit them as a single database transaction. This ensures consistency within the aggregate boundary.

### Optimistic Concurrency

Use version numbers or timestamps to detect conflicting updates to the same aggregate. This is critical for inventory allocation, where multiple orders may compete for the same stock.

### Specification Pattern

Encapsulate complex query criteria in specification objects. A `PendingAllocationSpecification` combines status, warehouse, and priority criteria into a reusable, composable query.

### Read Model Separation

For high-performance read scenarios (dashboards, tracking lookups), consider separate read models (CQRS) that are optimized for query patterns without loading full aggregate graphs.

## Relationships to Other Topics

- [Aggregates](../aggregates/) -- One repository per aggregate root
- [Entities](../entities/) -- Repositories persist and retrieve entities
- [Domain Events](../domain-events/) -- Event-sourced repositories reconstruct aggregates from event streams
- [Domain Services](../domain-services/) -- Services use repositories to load and save aggregates
- [Event-Driven Architecture](../event-driven-architecture/) -- CQRS read models are built from event streams

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 6: The Life Cycle of a Domain Object. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 12: Repositories. Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 10: Tactical Design Patterns. O'Reilly, 2021.
- Fowler, Martin. "Repository Pattern." martinfowler.com.
- Evans, Eric. "Domain-Driven Design Reference." Domain Language, Inc., 2015.
