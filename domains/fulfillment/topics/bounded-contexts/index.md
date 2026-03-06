# Bounded Contexts in Fulfillment

## Overview

A bounded context is a linguistic and conceptual boundary within which a particular domain model is defined and consistent. Every term, every entity, and every business rule has a precise meaning within that boundary. In fulfillment, bounded contexts separate the distinct operational domains of order processing, inventory management, warehouse operations, shipping, delivery, and returns.

## Relevance to Fulfillment

Fulfillment spans multiple operational disciplines, each with its own vocabulary, rules, and concerns. Without bounded contexts, terms like "order," "item," "status," and "location" become overloaded with conflicting meanings across teams. A bounded context ensures that:

- "Order" in the Order Processing Context means a validated customer request with line items
- "Order" in the Warehouse Operations Context means a pick list with bin locations and quantities
- "Shipment" in the Shipping Context means a carrier-assigned package with a tracking number
- "Shipment" in the Delivery Context means a delivery attempt with proof-of-delivery data

## The Six Fulfillment Bounded Contexts

### 1. Order Processing Context

**Purpose**: Receiving, validating, and routing customer orders.

**Responsibilities**:

- Accepting orders from e-commerce platforms, ERP systems, and other sales channels
- Validating order data: customer identity, addresses, product availability, payment authorization
- Splitting multi-item orders across warehouses based on inventory location
- Routing orders to the appropriate fulfillment center
- Managing order holds and cancellations

**Key Models**: FulfillmentOrder, OrderLine, OrderHold, RoutingRule

**Team Ownership**: Order management team, integration engineers

### 2. Inventory Allocation Context

**Purpose**: Reserving stock and managing product availability.

**Responsibilities**:

- Real-time inventory availability checking across warehouses
- Soft allocation (reserving stock for pending orders) and hard allocation (committing stock for confirmed orders)
- Backorder management when stock is insufficient
- Multi-warehouse allocation optimization
- Safety stock and reorder point management
- Inventory reconciliation and cycle counting

**Key Models**: InventoryAllocation, StockKeepingUnit, AvailabilityRecord, Backorder, ReservationHold

**Team Ownership**: Inventory management team

### 3. Warehouse Operations Context

**Purpose**: Physical fulfillment operations within the warehouse.

**Responsibilities**:

- Inbound receiving and put-away to storage locations
- Pick wave generation and optimization
- Pick, pack, and stage workflows
- Bin and zone management
- Quality control and inspection
- Labor management and task assignment

**Key Models**: PickWave, PickList, PackStation, BinLocation, PutAway, ReceivingOrder

**Team Ownership**: Warehouse operations team, WMS engineers

### 4. Shipping & Carrier Context

**Purpose**: Carrier integration, shipping logistics, and dispatch.

**Responsibilities**:

- Carrier selection and rate shopping across UPS, FedEx, DHL, USPS, and regional carriers
- Service level determination (ground, express, overnight, freight)
- Shipping label generation and printing
- Manifest creation and carrier pickup scheduling
- Customs documentation for international shipments
- Freight and parcel cost management

**Key Models**: Shipment, Package, ShippingLabel, CarrierRate, Manifest, CustomsDeclaration

**Team Ownership**: Shipping and logistics team

### 5. Delivery & Tracking Context

**Purpose**: Last-mile delivery management and shipment tracking.

**Responsibilities**:

- Aggregating tracking events from multiple carrier APIs
- Providing unified tracking status to customers and internal systems
- Delivery attempt management and exception handling
- Proof of delivery capture (signature, photo)
- Estimated delivery date calculation and updates
- Delivery notification to customers

**Key Models**: TrackingEvent, DeliveryAttempt, ProofOfDelivery, DeliveryRoute, DeliveryException

**Team Ownership**: Customer experience team, delivery operations

### 6. Returns & Exceptions Context

**Purpose**: Managing returns, refunds, exchanges, and order exceptions.

**Responsibilities**:

- Return Merchandise Authorization (RMA) processing
- Return label generation and shipping
- Return receiving and item inspection
- Refund and exchange authorization
- Disposition of returned goods (restock, refurbish, dispose)
- Exception handling for damaged, lost, or mis-shipped items

**Key Models**: ReturnRequest, ReturnAuthorization, Inspection, Disposition, ExchangeOrder, Refund

**Team Ownership**: Returns processing team, customer service

## Context Boundary Design Principles

### Autonomy

Each bounded context should be deployable and scalable independently. The Warehouse Operations Context should not need to be redeployed when the Shipping Context adds a new carrier.

### Language Isolation

Terms are defined locally. "Package" in the Warehouse Operations Context is a packed container ready for staging. "Package" in the Shipping Context is a carrier-assigned parcel with weight, dimensions, and a tracking number.

### Data Ownership

Each context owns its data and exposes it through well-defined interfaces. The Inventory Allocation Context is the authoritative source for stock levels; other contexts query or subscribe to its events.

### Team Alignment

Context boundaries align with team boundaries (Conway's Law). The warehouse operations team owns the Warehouse Operations Context; the shipping logistics team owns the Shipping & Carrier Context.

## Relationships to Other Topics

- [Strategic Design](../strategic-design/) -- Bounded contexts are the primary output of strategic design
- [Context Map](../context-map/) -- Defines relationships between bounded contexts
- [Ubiquitous Language](../ubiquitous-language/) -- Each bounded context has its own ubiquitous language
- [Anti-Corruption Layer](../anti-corruption-layer/) -- Protects bounded context boundaries from external models
- [Aggregates](../aggregates/) -- Each bounded context contains its own aggregates
- [Event-Driven Architecture](../event-driven-architecture/) -- Events enable communication between contexts

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 14: Maintaining Model Integrity. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 2: Domains, Subdomains, and Bounded Contexts. Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 6: Bounded Contexts. O'Reilly, 2021.
- Vernon, Vaughn. "Domain-Driven Design Distilled." Chapter 4: Strategic Design with Context Mapping. Addison-Wesley, 2016.
