# Inventory and Merchandising Context

## Overview

The Inventory and Merchandising Context is the bounded context responsible for stock management, replenishment, receiving, transfers, shrinkage tracking, and planogram management. This context tracks the physical reality of merchandise: how much is on hand, where it is located, when it needs to be reordered, and how it is displayed. While inventory management is often classified as a supporting subdomain (following well-understood patterns), the sophistication of a retailer's replenishment and allocation strategies can elevate certain capabilities to core subdomain status.

## Core Concepts

**Stock Position**: The fundamental concept representing the quantity of a SKU at a specific location. A stock position tracks three key quantities: on-hand (physically present), allocated (reserved for orders or transfers), and available-to-sell (on-hand minus allocated). The available-to-sell quantity is what the customer can purchase. Stock positions exist for every SKU-location combination across stores, warehouses, and distribution centers.

**Location**: A physical site where inventory is held. Locations include retail stores, distribution centers, warehouses, and potentially third-party fulfillment centers. Each location has attributes such as address, type (store, DC, vendor direct), operating hours, and fulfillment capabilities (can it ship to customers, does it support BOPIS).

**Replenishment**: The process of reordering merchandise to maintain target stock levels. Replenishment is triggered when available-to-sell quantity falls below a reorder point. The reorder point is determined by demand forecasts, lead times, and safety stock requirements. Replenishment may be automatic (system-generated purchase orders) or manual (buyer-approved).

**Purchase Order**: A formal request to a supplier for merchandise. Purchase orders specify products, quantities, expected delivery dates, and agreed prices. The purchase order lifecycle moves through creation, submission, supplier acknowledgment, shipment notification (via EDI 856 Advance Ship Notice), receiving, and closure.

**Receiving**: The process of accepting merchandise at a location. Receiving verifies that the delivered merchandise matches the purchase order or transfer in terms of products, quantities, and condition. Received quantities increase the on-hand stock position.

**Transfer**: The movement of merchandise between locations. Transfers are used to redistribute inventory from overstocked locations to understocked locations, or from distribution centers to stores. Transfers decrease stock at the origin and increase stock at the destination.

**Shrinkage**: Loss of inventory through theft, damage, administrative error, or vendor fraud. Shrinkage is identified through cycle counts and annual physical inventory counts that reveal discrepancies between system-recorded quantities and actual quantities.

**Planogram**: A visual diagram that defines how products should be displayed on shelves and fixtures within a store. Planograms specify which products go where, how many facings each product receives, and the vertical and horizontal placement on the fixture. Planograms connect inventory to the in-store customer experience.

## Aggregates

The **Stock Position Aggregate** tracks on-hand, allocated, and available-to-sell for a SKU at a location. Invariants: available-to-sell equals on-hand minus allocated, quantities are non-negative, and allocation requests are validated against available stock.

The **Purchase Order Aggregate** manages the order lifecycle. Invariants: quantities must be positive, the order must reference valid products and a valid supplier, and received quantities cannot exceed ordered quantities without explicit over-receipt authorization.

## Domain Events

- StockReceived: Merchandise has arrived and been counted at a location.
- StockAllocated: Inventory has been reserved for an order or transfer.
- StockDepleted: Available-to-sell has reached zero for a SKU at a location.
- ReplenishmentTriggered: Stock has fallen below reorder point.
- ShrinkageRecorded: An inventory discrepancy has been identified and recorded.
- TransferShipped: Merchandise has left the origin location.
- TransferReceived: Merchandise has arrived at the destination location.

## Domain Services

**InventoryAllocationService**: Allocates available stock to competing demand sources (customer orders, transfers, holds) based on priority rules.

**ReplenishmentCalculationService**: Calculates reorder quantities based on demand forecasts, lead times, safety stock, and vendor minimums.

## Integration with Other Contexts

- **Product Catalog Context** (upstream): Provides SKU definitions that inventory tracks.
- **Point of Sale Context**: Completed transactions decrement store inventory. POS may query availability during scanning.
- **Omnichannel Context**: Provides real-time availability for BOPIS, ship-from-store, and endless aisle.
- **Pricing and Promotions Context**: Inventory levels inform markdown decisions. High inventory may trigger promotional activity.
- **External Suppliers**: Purchase orders and receiving integrate with supplier systems via EDI (850, 810, 856).

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/index.md): Inventory and Merchandising is a core retail bounded context.
- [Aggregates](../aggregates/index.md): Stock Position and Purchase Order are primary aggregates.
- [Entities](../entities/index.md): Location and Purchase Order are key entities.
- [Value Objects](../value-objects/index.md): Quantity, LocationCode, and PlanogramPosition.
- [Domain Events](../domain-events/index.md): StockReceived, StockDepleted, ReplenishmentTriggered events.
- [Retail Standards](../retail-standards/index.md): EDI standards for supplier integration.
- [Anti-Corruption Layer](../anti-corruption-layer/index.md): Supplier EDI integration through ACLs.
- [Omnichannel Context](../omnichannel-context/index.md): Real-time inventory feeds omnichannel capabilities.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Hugos, Michael H. *Essentials of Supply Chain Management*. Wiley, 2018.
- GS1. *GS1 Standards for Supply Chain*. GS1 AISBL, ongoing. EDI message standards and product identification.
