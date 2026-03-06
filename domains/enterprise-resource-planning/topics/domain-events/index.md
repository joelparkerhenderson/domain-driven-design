# Domain Events in Enterprise Resource Planning

## Overview

A domain event records something significant that happened in the domain. In ERP, domain events drive the cross-functional workflows that connect sales, procurement, inventory, manufacturing, and finance.

## ERP Domain Events

### Sales/Order Context

| Event | Trigger | Consumers |
|-------|---------|-----------|
| OrderPlaced | Customer places order | Inventory (reserve stock), Finance (credit check) |
| OrderConfirmed | Order validated and accepted | Fulfillment, Customer notification |
| OrderShipped | Goods dispatched | Customer notification, Finance (revenue recognition) |
| OrderCancelled | Order cancelled | Inventory (release reservation), Finance |
| ReturnRequested | Customer initiates return | Inventory, Finance (credit memo) |

### Procurement Context

| Event | Trigger | Consumers |
|-------|---------|-----------|
| RequisitionCreated | Internal purchase request | Procurement approval workflow |
| PurchaseOrderApproved | PO authorized | Supplier notification, Finance (commitment) |
| GoodsReceived | Delivery accepted | Inventory (stock update), Finance (accrual), Quality |
| SupplierInvoiceReceived | Supplier sends invoice | Finance (AP posting) |

### Inventory/Warehouse Context

| Event | Trigger | Consumers |
|-------|---------|-----------|
| StockReceived | Goods added to inventory | Finance (inventory valuation) |
| StockIssued | Goods removed from inventory | Finance (COGS), Manufacturing |
| StockBelowReorderPoint | Inventory drops below threshold | Procurement (auto-requisition) |
| StockAdjusted | Cycle count correction | Finance (adjustment entry) |
| TransferCompleted | Inter-warehouse transfer done | Both warehouses |

### Finance/Accounting Context

| Event | Trigger | Consumers |
|-------|---------|-----------|
| JournalEntryPosted | Entry recorded in GL | Financial reporting |
| InvoiceIssued | Customer invoice created | AR, Customer notification |
| PaymentReceived | Customer payment posted | AR, Sales (order status) |
| FiscalPeriodClosed | Period-end closing | Reporting, Audit |

### Manufacturing Context

| Event | Trigger | Consumers |
|-------|---------|-----------|
| WorkOrderCreated | Production scheduled | Inventory (material reservation) |
| ProductionStarted | Manufacturing begins | Inventory (material issuance) |
| ProductionCompleted | Finished goods produced | Inventory (receipt), Finance (production costs) |
| QualityHoldApplied | Quality issue detected | Production hold, Investigation |

### Human Resources Context

| Event | Trigger | Consumers |
|-------|---------|-----------|
| EmployeeHired | New hire processed | Payroll, IT (account creation), Facilities |
| PayrollProcessed | Pay period completed | Finance (journal entries) |
| EmployeeTerminated | Employment ended | Payroll (final pay), IT (access revocation) |

## Event Flow Example: Order-to-Cash

1. `OrderPlaced` → Inventory reserves stock, Finance checks credit
2. `OrderConfirmed` → Warehouse creates pick list
3. `StockIssued` → Finance records COGS
4. `OrderShipped` → Customer notified, Finance recognizes revenue
5. `InvoiceIssued` → AR created
6. `PaymentReceived` → AR cleared, cash posted

## Relationships to Other Topics

- [Aggregates](../aggregates/) — Aggregates produce events
- [Event-Driven Architecture](../event-driven-architecture/) — Infrastructure for events
- [Integration Patterns](../integration-patterns/) — Events are the primary inter-context mechanism
- [Context Map](../context-map/) — Events flow along context map relationships

## References

- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 8. Addison-Wesley, 2013.
- Richardson, Chris. "Microservices Patterns." Chapter 4. Manning, 2018.
