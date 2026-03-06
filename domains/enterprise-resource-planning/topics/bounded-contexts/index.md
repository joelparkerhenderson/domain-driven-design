# Bounded Contexts in Enterprise Resource Planning

## Overview

A bounded context defines a boundary within which a particular domain model is consistent and terms have unambiguous meaning. In ERP, the same business concept — such as "order," "product," or "customer" — means different things in different functional areas. Bounded contexts make these differences explicit rather than forcing a single universal model.

## The Six ERP Bounded Contexts

### Finance/Accounting Context

- **Focus**: General ledger, chart of accounts, journal entries, accounts payable, accounts receivable, financial statements, tax calculations, fiscal period management
- **Aggregate Roots**: JournalEntry, Invoice, Payment, Account
- **Key Operations**: Post journal entry, issue invoice, record payment, close fiscal period, generate financial statements
- **Language**: Debit, Credit, Ledger, Fiscal Period, Accrual, Depreciation, Reconciliation

### Procurement Context

- **Focus**: Purchase requisitions, purchase orders, supplier management, sourcing, goods receipt, supplier evaluation
- **Aggregate Roots**: PurchaseOrder, Supplier, PurchaseRequisition
- **Key Operations**: Create requisition, approve requisition, issue PO, receive goods, evaluate supplier
- **Language**: Requisition, Purchase Order, RFQ, Supplier, Lead Time, Terms, Receiving

### Inventory/Warehouse Context

- **Focus**: Stock levels, warehouse locations, bin management, stock movements, cycle counting, logistics, transfer orders
- **Aggregate Roots**: InventoryItem, Warehouse, TransferOrder
- **Key Operations**: Receive stock, issue stock, transfer between warehouses, count inventory, reserve stock
- **Language**: SKU, Bin Location, Stock Level, Safety Stock, Reorder Point, Pick List

### Sales/Order Context

- **Focus**: Customer management, sales orders, pricing, discounts, fulfillment, shipping, returns
- **Aggregate Roots**: SalesOrder, Customer, PriceList
- **Key Operations**: Create order, apply pricing, fulfill order, ship order, process return
- **Language**: Sales Order, Line Item, Discount, Fulfillment, Backorder, RMA, Customer

### Human Resources Context

- **Focus**: Employee records, payroll processing, benefits administration, recruitment, time tracking, compliance
- **Aggregate Roots**: Employee, PayrollRun, Position
- **Key Operations**: Hire employee, process payroll, enroll in benefits, track time, terminate
- **Language**: Employee, Payroll, Benefits, PTO, Recruitment, Position, Compensation

### Manufacturing Context

- **Focus**: Production planning, bill of materials, work orders, routing, quality control, capacity planning
- **Aggregate Roots**: WorkOrder, BillOfMaterials, ProductionPlan
- **Key Operations**: Create work order, consume materials, report production, inspect quality, plan capacity
- **Language**: BOM, Work Order, Routing, Work Center, Yield, Scrap, Quality Hold

## Boundary Enforcement

Each context maintains its own data model and communicates with other contexts only through well-defined interfaces (APIs, domain events). A "Product" in the Sales context (with pricing, descriptions, categories) is a different model than a "Product" in the Inventory context (with SKU, bin location, quantity on hand).

## Relationships to Other Topics

- [Strategic Design](../strategic-design/) — Bounded contexts are the primary output of strategic design
- [Context Map](../context-map/) — Shows relationships between contexts
- [Ubiquitous Language](../ubiquitous-language/) — Each context defines its own language
- [Integration Patterns](../integration-patterns/) — How contexts exchange information

## References

- Evans, Eric. "Domain-Driven Design." Chapter 14. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 2. Addison-Wesley, 2013.
- Millett, Scott & Tune, Nick. "Patterns, Principles, and Practices of Domain-Driven Design." Chapter 6. Wrox, 2015.
