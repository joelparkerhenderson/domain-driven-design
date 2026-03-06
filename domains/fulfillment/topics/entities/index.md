# Entities in Fulfillment

## Overview

An entity is a domain object defined primarily by its identity rather than its attributes. Two entities with the same attributes but different identities are considered different objects. Entities have a lifecycle -- they are created, undergo state changes, and may eventually be archived or deleted. In fulfillment, entities represent the core operational objects that are tracked, modified, and referenced throughout the order-to-delivery lifecycle.

## Relevance to Fulfillment

Fulfillment operations track physical and logical objects through complex workflows. A package is not just a set of dimensions and weight -- it is a specific container that is packed at a station, labeled, staged, handed to a carrier, tracked, and delivered. Its identity matters because different systems reference it by its unique identifier throughout its journey. Entities capture this identity-centric nature of fulfillment objects.

## Key Concepts

### Identity

Every entity has a unique identifier that distinguishes it from all other entities of the same type. This identity remains stable even as the entity's attributes change over time.

### Lifecycle

Entities are created, evolve through state transitions, and are eventually completed or archived. A FulfillmentOrder progresses from Received to Validated to Allocated to Shipped to Delivered. The entity retains its identity throughout.

### Mutability

Unlike value objects, entities change over time. A Shipment's status changes from Created to Labeled to Dispatched. A Warehouse's capacity and active zones may change. The entity itself persists; its state evolves.

## Fulfillment Entities

### FulfillmentOrder

**Identity**: FulfillmentOrderId (system-generated UUID or sequential identifier)

**Purpose**: Represents a validated customer order that has entered the fulfillment pipeline. It is the primary entity around which the fulfillment process revolves.

**Key attributes**:

- FulfillmentOrderId (immutable identity)
- ChannelOrderId (the original order identifier from the sales channel)
- ChannelSource (e-commerce platform, ERP, marketplace)
- ShippingAddress (Address value object)
- OrderLines (collection of OrderLine entities)
- OrderPriority (standard, expedited, same-day)
- Status (Received, Validated, Allocated, InPicking, Packed, Shipped, Delivered, Cancelled)
- RequestedServiceLevel (ground, two-day, overnight)
- CreatedAt, UpdatedAt timestamps

**Lifecycle**: Created when an order is received from a sales channel. Transitions through validation, allocation, picking, packing, shipping, and delivery. May be cancelled at any point before dispatch.

### Shipment

**Identity**: ShipmentId (system-generated UUID)

**Purpose**: Represents a physical shipment assigned to a carrier for delivery. A single fulfillment order may produce multiple shipments (multi-package or split shipments).

**Key attributes**:

- ShipmentId (immutable identity)
- FulfillmentOrderId (reference to parent order)
- CarrierId (reference to assigned carrier)
- ServiceLevel (ground, express, overnight, freight)
- Packages (collection of Package entities)
- TrackingNumber (assigned by carrier)
- ShipDate, EstimatedDeliveryDate
- Status (Created, Labeled, Staged, Dispatched, InTransit, Delivered, Returned)
- OriginWarehouseId

**Lifecycle**: Created when packing is complete. Receives a carrier assignment and label. Is staged for carrier pickup. Dispatched to carrier. Tracked through delivery. May be returned.

### Package

**Identity**: PackageId (system-generated UUID)

**Purpose**: Represents a single physical container within a shipment. Contains specific items packed together.

**Key attributes**:

- PackageId (immutable identity)
- ShipmentId (reference to parent shipment)
- Weight (Weight value object)
- Dimensions (Dimensions value object)
- PackageType (box, envelope, pallet, tube)
- ShippingLabel (ShippingLabel value object, assigned during labeling)
- Barcode (Barcode value object for warehouse scanning)
- Contents (list of item SKUs and quantities)
- PackedAt timestamp, PackedBy (worker identifier)

**Lifecycle**: Created during the packing process. Receives a shipping label. Is staged and dispatched as part of a shipment.

### Warehouse

**Identity**: WarehouseId (system-assigned code, e.g., "WH-EAST-01")

**Purpose**: Represents a physical fulfillment center or distribution center where inventory is stored and orders are fulfilled.

**Key attributes**:

- WarehouseId (immutable identity)
- Name, Address
- Zones (collection of zone identifiers)
- Capacity (storage capacity metrics)
- OperatingHours
- SupportedCarriers (list of carrier identifiers available for pickup)
- IsActive (whether the warehouse is currently accepting orders)

**Lifecycle**: Created when a new fulfillment center is onboarded. Attributes change as zones are added, capacity is expanded, or carrier relationships change. Deactivated when a facility is closed.

### Carrier

**Identity**: CarrierId (system-assigned code, e.g., "UPS", "FEDEX", "DHL")

**Purpose**: Represents a shipping carrier that transports packages from the warehouse to the delivery destination.

**Key attributes**:

- CarrierId (immutable identity)
- Name (UPS, FedEx, DHL, USPS)
- SupportedServiceLevels (ground, express, overnight, freight)
- APIConfiguration (endpoint URLs, authentication credentials)
- RateStructure (base rates, surcharges, dimensional weight factors)
- PickupSchedule (scheduled collection times per warehouse)
- CutoffTimes (latest submission time for same-day dispatch per service level)
- IsActive

**Lifecycle**: Created when a carrier relationship is established. Rate structures and API configurations are updated periodically. A carrier may be deactivated if the relationship ends.

### ReturnRequest

**Identity**: ReturnRequestId (system-generated UUID)

**Purpose**: Represents a customer's request to return one or more items from a delivered order.

**Key attributes**:

- ReturnRequestId (immutable identity)
- FulfillmentOrderId (reference to the original order)
- ReturnLines (collection of return line items with SKU, quantity, and reason)
- ReturnLabel (ReturnLabel value object, provided to customer)
- Status (Requested, Authorized, InTransit, Received, Inspected, Resolved, Closed)
- RefundAmount (calculated after inspection)
- DispositionDecisions (restock, refurbish, dispose per item)
- RequestedAt, ResolvedAt timestamps

**Lifecycle**: Created when a customer initiates a return. Authorized and a return label is generated. Items are received, inspected, and disposition is assigned. Refund or exchange is processed. Return is closed.

### PickList

**Identity**: PickListId (system-generated UUID)

**Purpose**: Represents a collection of items to be picked from specific bin locations by a warehouse worker during a pick wave.

**Key attributes**:

- PickListId (immutable identity)
- PickWaveId (reference to parent wave)
- WarehouseId (reference to warehouse)
- PickerAssignment (worker identifier)
- Items (collection of pick tasks: SKU, quantity, BinLocation)
- PickPath (optimized sequence of bin locations)
- Status (Assigned, InProgress, Completed, ShortPicked)
- StartedAt, CompletedAt timestamps

**Lifecycle**: Created as part of a pick wave. Assigned to a picker. Executed in sequence. Completed or flagged for short picks. Results reported to the packing stage.

## Entity Design Guidelines

1. **Identity is non-negotiable**: Every entity must have a clear, unique, immutable identifier.

2. **Minimize entity scope**: Include only the attributes necessary for the entity's responsibilities within its bounded context. A FulfillmentOrder does not need the customer's full profile -- just the shipping address and contact information.

3. **Use value objects for attributes**: Weight, Dimensions, Address, and TrackingNumber are value objects, not entities. They are defined by their values, not their identity.

4. **Reference other entities by identity**: A Shipment references a FulfillmentOrder by FulfillmentOrderId, not by embedding the entire order object.

5. **Define clear state machines**: Entity lifecycles should be modeled as explicit state machines with valid transitions. A FulfillmentOrder in "Shipped" status cannot transition back to "Validated."

## Relationships to Other Topics

- [Aggregates](../aggregates/) -- Entities serve as aggregate roots or internal entities within aggregates
- [Value Objects](../value-objects/) -- Entities contain value objects as attributes
- [Repositories](../repositories/) -- Entities are persisted and retrieved through repositories
- [Domain Events](../domain-events/) -- Entity state transitions produce domain events
- [Ubiquitous Language](../ubiquitous-language/) -- Entity names come from the ubiquitous language

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 5: A Model Expressed in Software. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 5: Entities. Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 10: Tactical Design Patterns. O'Reilly, 2021.
