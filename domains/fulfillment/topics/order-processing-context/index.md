# Order Processing Context in Fulfillment

## Overview

The Order Processing Context is the bounded context responsible for receiving customer orders from sales channels, validating order data, splitting multi-warehouse orders, and routing validated orders into the fulfillment pipeline. It serves as the entry point to the fulfillment domain, translating external order representations into the fulfillment system's internal FulfillmentOrder model. This context owns the transition from "customer placed an order" to "order is ready for inventory allocation."

## Relevance to Fulfillment

Every fulfillment operation begins with an order. The Order Processing Context ensures that only valid, complete, and properly formatted orders enter the fulfillment pipeline. It absorbs the complexity of integrating with multiple sales channels (e-commerce platforms, ERP systems, marketplaces, EDI partners), each with its own order format, and normalizes them into a single FulfillmentOrder representation that downstream contexts can process consistently.

## Bounded Context Boundaries

**Owns**: FulfillmentOrder aggregate, order validation rules, order splitting logic, channel integration adapters

**Does not own**: Inventory availability (Inventory Allocation Context), warehouse pick/pack operations (Warehouse Operations Context), carrier selection (Shipping & Carrier Context)

**Upstream contexts**: External e-commerce platforms, ERP systems, marketplace APIs

**Downstream contexts**: Inventory Allocation Context, Warehouse Operations Context

## Core Capabilities

### Order Intake

Order intake receives orders from multiple sales channels and creates FulfillmentOrder aggregates.

- Accept orders from e-commerce platforms via webhooks or API polling
- Accept orders from ERP systems via EDI (850 Purchase Order) or API integration
- Accept orders from marketplace channels (Amazon, Shopify, BigCommerce)
- Translate channel-specific order formats into the internal FulfillmentOrder model
- Assign FulfillmentOrderId and record ChannelOrderId for cross-reference
- Deduplicate orders to prevent double-fulfillment from retry scenarios

### Order Validation

Validation ensures that each order contains complete, correct, and actionable data before it enters the fulfillment pipeline.

- **Address validation**: Verify shipping address format, confirm deliverability, standardize address components, flag PO boxes or restricted addresses
- **Item validation**: Confirm that all SKUs exist in the product catalog and are fulfillable
- **Quantity validation**: Verify that requested quantities are within allowed order limits
- **Service level validation**: Confirm that the requested service level (overnight, two-day, ground) is available for the destination
- **Payment confirmation**: Verify that payment has been captured or authorized (via upstream confirmation, not payment processing)
- **Fraud screening results**: Accept or reject based on upstream fraud check outcomes

### Order Splitting

When a single customer order cannot be fulfilled from one warehouse, the Order Processing Context splits it into multiple FulfillmentOrders.

- Evaluate item availability across warehouses (using data published by the Inventory Allocation Context)
- Split orders by warehouse assignment, creating child FulfillmentOrders linked to the original
- Optimize splits to minimize the total number of shipments and reduce shipping cost
- Maintain traceability between the original channel order and all resulting FulfillmentOrders

### Order Routing

Routing determines which warehouse or fulfillment center should process each order or order split.

- Apply proximity-based routing to minimize transit time to the customer
- Apply capacity-based routing to balance workload across warehouses
- Apply capability-based routing for special handling (hazmat, cold chain, oversized)
- Respect carrier cutoff times when routing to ensure same-day dispatch eligibility

## Key Aggregates and Entities

- **FulfillmentOrder** (aggregate root): The primary entity representing a validated order in the fulfillment pipeline
- **OrderLine** (entity within FulfillmentOrder): Individual line items with SKU, quantity, and price
- **ChannelSource** (value object): Identifies the originating sales channel
- **Address** (value object): Validated shipping address
- **OrderPriority** (value object): Processing priority level

## Domain Events Produced

- **OrderReceived**: A new order has been accepted from a sales channel
- **OrderValidated**: An order has passed all validation checks
- **OrderSplit**: A multi-warehouse order has been divided into separate FulfillmentOrders
- **OrderCancelled**: An order has been cancelled before fulfillment completion
- **OrderRoutedToWarehouse**: An order has been assigned to a specific warehouse for fulfillment

## Domain Events Consumed

- **PaymentCaptured** (from upstream Sales/Billing context): Confirms payment readiness
- **InventoryAvailabilityChanged** (from Inventory Allocation Context): Updates routing decisions based on stock levels
- **AllocationFailed** (from Inventory Allocation Context): Triggers backorder handling or customer notification

## Anti-Corruption Layer

The Order Processing Context maintains an anti-corruption layer to translate between external channel formats and the internal fulfillment model.

- Channel-specific adapters convert Shopify orders, Amazon orders, EDI 850 documents, and ERP orders into FulfillmentOrder aggregates
- External status codes and enumerations are mapped to internal domain values
- External identifiers are preserved as ChannelOrderId but do not replace the internal FulfillmentOrderId

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) -- Order Processing is one of the six core bounded contexts
- [Inventory Allocation Context](../inventory-allocation-context/) -- Downstream consumer of validated orders
- [Anti-Corruption Layer](../anti-corruption-layer/) -- Translates external channel formats
- [Domain Events](../domain-events/) -- Produces OrderReceived, OrderValidated, OrderSplit events
- [Aggregates](../aggregates/) -- Owns the FulfillmentOrder aggregate
- [Integration Patterns](../integration-patterns/) -- Channel adapters implement integration patterns

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 14: Maintaining Model Integrity. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 2: Domains, Subdomains, and Bounded Contexts. Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 7: Modeling Bounded Contexts. O'Reilly, 2021.
- Hohpe, Gregor & Woolf, Bobby. "Enterprise Integration Patterns." Addison-Wesley, 2003.
