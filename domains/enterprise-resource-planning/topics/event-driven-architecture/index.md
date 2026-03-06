# Event-Driven Architecture in Enterprise Resource Planning

## Overview

Event-driven architecture (EDA) enables ERP bounded contexts to communicate through domain events rather than direct synchronous calls. This is essential in ERP because business transactions flow across multiple functional areas — a single sales order triggers inventory, finance, and shipping actions.

## ERP Event Flow Patterns

### Order-to-Cash Flow

1. `OrderPlaced` → Inventory reserves stock, Finance checks credit
2. `OrderConfirmed` → Warehouse generates pick list
3. `StockIssued` → Finance records COGS
4. `OrderShipped` → Customer notified
5. `InvoiceIssued` → AR created
6. `PaymentReceived` → AR cleared

### Procure-to-Pay Flow

1. `StockBelowReorderPoint` → Procurement creates requisition
2. `PurchaseOrderApproved` → Supplier notified, Finance records commitment
3. `GoodsReceived` → Inventory updated, Finance records accrual
4. `SupplierInvoiceReceived` → AP created
5. `SupplierPaymentProcessed` → AP cleared

### Plan-to-Produce Flow

1. `ProductionPlanCreated` → MRP calculates material needs
2. `WorkOrderCreated` → Materials reserved
3. `ProductionStarted` → Materials issued
4. `ProductionCompleted` → Finished goods received, costs rolled up

## Choreography vs. Orchestration

- **Choreography**: Each context reacts independently. OrderPlaced causes Inventory to reserve, Finance to check credit — all independently. Simple, loosely coupled.
- **Orchestration**: Order Fulfillment Saga coordinates: check credit → reserve stock → generate pick list → ship → invoice. Explicit, easier to monitor failures.

Use choreography for loosely coupled notifications; use orchestration for multi-step processes where failure handling matters.

## Saga Pattern in ERP

### Order Fulfillment Saga

Steps with compensating actions:

1. Check credit → compensate: release credit hold
2. Reserve inventory → compensate: release reservation
3. Generate pick list → compensate: cancel pick list
4. Ship goods → compensate: initiate return
5. Invoice customer → (no compensation needed if payment not yet received)

## Relationships to Other Topics

- [Domain Events](../domain-events/) — The domain objects flowing through the architecture
- [Integration Patterns](../integration-patterns/) — EDA is the foundation for integration
- [Bounded Contexts](../bounded-contexts/) — EDA enables loose coupling
- [Aggregates](../aggregates/) — Aggregates produce events

## References

- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 8. Addison-Wesley, 2013.
- Richardson, Chris. "Microservices Patterns." Chapter 4. Manning, 2018.
- Hohpe, Gregor & Woolf, Bobby. "Enterprise Integration Patterns." Addison-Wesley, 2003.
