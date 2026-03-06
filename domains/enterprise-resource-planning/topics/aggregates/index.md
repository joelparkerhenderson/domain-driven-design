# Aggregates in Enterprise Resource Planning

## Overview

An aggregate is a cluster of domain objects treated as a single unit for data changes and consistency enforcement. In ERP, aggregates model the core transactional documents and records that drive business operations.

## ERP Aggregates

### SalesOrder Aggregate

- **Root**: SalesOrder (identified by OrderID)
- **Internal entities**: LineItem
- **Value objects**: Money, Quantity, ShippingAddress, DiscountRate, TaxRate
- **Invariants**: Order total equals sum of line items; line items cannot exceed available credit limit; cancelled orders cannot be modified
- **Events**: OrderPlaced, OrderConfirmed, OrderShipped, OrderCancelled

### PurchaseOrder Aggregate

- **Root**: PurchaseOrder (identified by PO Number)
- **Internal entities**: PurchaseLineItem
- **Value objects**: Money, Quantity, DeliveryTerms, PaymentTerms
- **Invariants**: PO total must be within approval authority; all line items must reference valid products; approved POs cannot have lines added without re-approval
- **Events**: PurchaseOrderCreated, PurchaseOrderApproved, GoodsReceived

### JournalEntry Aggregate

- **Root**: JournalEntry (identified by EntryID)
- **Internal entities**: JournalLine
- **Value objects**: Money, AccountReference, FiscalPeriod
- **Invariants**: Total debits must equal total credits; entries can only post to open fiscal periods; posted entries are immutable
- **Events**: JournalEntryCreated, JournalEntryPosted, JournalEntryReversed

### InventoryItem Aggregate

- **Root**: InventoryItem (identified by SKU + Warehouse)
- **Value objects**: Quantity, BinLocation, ReorderPoint, SafetyStock
- **Invariants**: On-hand quantity cannot be negative; reserved quantity cannot exceed on-hand; reorder point must be non-negative
- **Events**: StockReceived, StockIssued, StockAdjusted, StockBelowReorderPoint

### WorkOrder Aggregate (Manufacturing)

- **Root**: WorkOrder (identified by WorkOrderID)
- **Internal entities**: OperationStep
- **Value objects**: Quantity, BOMReference, RoutingReference
- **Invariants**: Material consumption cannot exceed allocated quantities; completed quantity cannot exceed planned quantity without override; quality hold prevents release
- **Events**: WorkOrderCreated, ProductionStarted, ProductionCompleted, QualityHoldApplied

### Employee Aggregate (HR)

- **Root**: Employee (identified by EmployeeID)
- **Value objects**: Address, Compensation, BenefitsEnrollment, TaxWithholding
- **Invariants**: Active employment dates cannot overlap; compensation changes require effective date; terminated employees cannot accrue PTO
- **Events**: EmployeeHired, CompensationChanged, EmployeeTerminated

## Aggregate Design Rules

1. **Keep aggregates small** — A SalesOrder contains its LineItems but references Customer by ID, not by embedding the Customer object
2. **Reference other aggregates by identity** — PurchaseOrder references Supplier by SupplierID
3. **Use domain events for cross-aggregate coordination** — OrderPlaced event triggers inventory reservation in a separate transaction
4. **Design for eventual consistency between aggregates** — Finance learns about a new order milliseconds after it is placed

## Relationships to Other Topics

- [Entities](../entities/) — Aggregate roots are entities
- [Value Objects](../value-objects/) — Aggregates contain value objects
- [Domain Events](../domain-events/) — Aggregates produce events
- [Repositories](../repositories/) — One repository per aggregate root
- [Domain Services](../domain-services/) — Coordinate across aggregates

## References

- Evans, Eric. "Domain-Driven Design." Chapter 6. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 10. Addison-Wesley, 2013.
