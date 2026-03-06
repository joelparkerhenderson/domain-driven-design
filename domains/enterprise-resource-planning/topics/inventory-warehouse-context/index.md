# Inventory/Warehouse Context in Enterprise Resource Planning

## Overview

The Inventory/Warehouse Context manages the physical stock of goods: storage locations, stock levels, warehouse operations, material movements, cycle counting, and logistics coordination.

## Core Responsibilities

- **Stock Management**: Tracking on-hand, reserved, available, and in-transit quantities per SKU per location
- **Warehouse Operations**: Receiving, put-away, picking, packing, shipping
- **Inventory Control**: Cycle counting, stock adjustments, reconciliation
- **Transfer Management**: Moving stock between warehouses and locations
- **Replenishment**: Monitoring reorder points and safety stock levels
- **Logistics Coordination**: Coordinating with shipping carriers for inbound and outbound deliveries

## Aggregates

### InventoryItem Aggregate

- **Root**: InventoryItem (identified by SKU + Warehouse)
- **Value objects**: Quantity (on-hand, reserved, available, in-transit), BinLocation, ReorderPoint, SafetyStock, LotNumber, ExpirationDate
- **Invariants**: On-hand cannot be negative; reserved cannot exceed on-hand; available = on-hand - reserved
- **Events**: StockReceived, StockIssued, StockReserved, StockReleased, StockAdjusted, StockBelowReorderPoint

### TransferOrder Aggregate

- **Root**: TransferOrder (identified by TransferOrderID)
- **Internal entities**: TransferLineItem
- **Value objects**: Quantity, SourceWarehouse, DestinationWarehouse
- **States**: Created → Shipped → In Transit → Received
- **Invariants**: Source warehouse must have sufficient available stock; quantities must be positive

### Warehouse Aggregate

- **Root**: Warehouse (identified by WarehouseCode)
- **Value objects**: Address, WarehouseType, Capacity, BinLayout
- **Operations**: Define bin locations, set storage rules, manage zones

## Domain Events

| Event | Trigger | Consumers |
|-------|---------|-----------|
| StockReceived | Goods receipt from procurement or production | Finance (valuation), Quality |
| StockIssued | Goods issued for sales or manufacturing | Finance (COGS/WIP) |
| StockReserved | Stock allocated for an order | Sales (confirmation) |
| StockBelowReorderPoint | Quantity drops below threshold | Procurement (auto-requisition) |
| StockAdjusted | Cycle count correction | Finance (adjustment entry) |
| TransferCompleted | Inter-warehouse transfer done | Source and destination warehouses |

## Integration with Other Contexts

- **Sales → Inventory**: OrderPlaced triggers stock reservation; fulfillment triggers stock issuance
- **Procurement → Inventory**: GoodsReceived triggers stock receipt
- **Manufacturing → Inventory**: Production consumes raw materials (issuance) and produces finished goods (receipt)
- **Inventory → Finance**: All stock movements trigger valuation entries (FIFO, LIFO, weighted average)
- **Inventory → Procurement**: StockBelowReorderPoint triggers replenishment

## Inventory Valuation Methods

- **FIFO** (First In, First Out): Oldest inventory costs used first
- **LIFO** (Last In, First Out): Newest inventory costs used first
- **Weighted Average**: Average cost across all units
- **Standard Cost**: Predetermined cost with variance tracking

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) — One of six ERP bounded contexts
- [Context Map](../context-map/) — Bidirectional partnership with Manufacturing; downstream from Sales and Procurement
- [Aggregates](../aggregates/) — InventoryItem, TransferOrder, Warehouse are aggregate roots
- [Domain Events](../domain-events/) — Inventory events drive Finance and Procurement
- [Value Objects](../value-objects/) — Quantity, BinLocation are core value objects

## References

- Evans, Eric. "Domain-Driven Design." Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
- Tompkins, White, Bozer, & Tanchoco. "Facilities Planning." Wiley, 2010.
- Richards, Gwynne. "Warehouse Management." Kogan Page, 2017.
