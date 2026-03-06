# Context Map in Enterprise Resource Planning

## Overview

A context map documents the relationships and integration patterns between bounded contexts. In an ERP system, contexts are highly interconnected — a sales order triggers procurement, which triggers inventory, which feeds manufacturing, which flows back to finance. The context map makes these dependencies explicit.

## Context Relationships

### Sales/Order → Finance/Accounting (Customer-Supplier)

- Sales is upstream; Finance is downstream
- When an order is invoiced, Sales publishes OrderInvoiced events
- Finance creates corresponding journal entries (debit AR, credit Revenue)
- Finance depends on Sales for revenue recognition data

### Sales/Order → Inventory/Warehouse (Customer-Supplier)

- Sales is upstream; Inventory is downstream for fulfillment
- OrderPlaced events trigger stock reservation in Inventory
- Inventory confirms availability or raises backorder notifications
- Fulfillment completion in Inventory triggers shipping in Sales

### Procurement → Finance/Accounting (Customer-Supplier)

- Procurement is upstream; Finance is downstream
- PurchaseOrderApproved creates a commitment in Finance (debit Inventory/Expense, credit AP)
- GoodsReceived triggers accrual entries
- SupplierInvoiceReceived triggers AP posting

### Procurement → Inventory/Warehouse (Customer-Supplier)

- Procurement is upstream for goods receipt
- GoodsReceived events trigger Inventory to update stock levels
- Inventory publishes StockBelowReorderPoint events that trigger procurement

### Manufacturing → Inventory/Warehouse (Partnership)

- Bidirectional relationship
- Manufacturing consumes raw materials from Inventory (material issuance)
- Manufacturing outputs finished goods to Inventory (production receipt)
- Both teams coordinate on BOM changes and material availability

### Human Resources → Finance/Accounting (Customer-Supplier)

- HR is upstream; Finance is downstream for payroll
- PayrollProcessed events create journal entries (debit Salary Expense, credit Payroll Payable)
- Finance depends on HR for labor cost allocation to cost centers

### Manufacturing → Procurement (Customer-Supplier)

- Manufacturing is downstream; Procurement is upstream
- MRP runs in Manufacturing generate purchase requisitions in Procurement
- Procurement fulfills material requirements through supplier orders

### External Systems

- **Banking Systems** → Finance: ACL translates bank statement formats into domain transactions
- **Tax Authorities** → Finance: ACL formats tax filings from internal financial data
- **Logistics Providers** → Inventory: Open Host Service for shipping and tracking
- **Supplier Portals** → Procurement: ACL translates supplier catalog formats

## Integration Summary

| Upstream | Downstream | Pattern | Mechanism |
|----------|-----------|---------|-----------|
| Sales | Finance | Customer-Supplier | Domain Events |
| Sales | Inventory | Customer-Supplier | Domain Events |
| Procurement | Finance | Customer-Supplier | Domain Events |
| Procurement | Inventory | Customer-Supplier | Domain Events |
| Manufacturing | Inventory | Partnership | Domain Events + Shared Data |
| HR | Finance | Customer-Supplier | Domain Events |
| Manufacturing | Procurement | Customer-Supplier | Domain Events |
| Banking APIs | Finance | ACL | File Import / API |
| Logistics | Inventory | Open Host Service | REST API |

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) — The nodes on the context map
- [Strategic Design](../strategic-design/) — Context mapping is a strategic design activity
- [Anti-Corruption Layer](../anti-corruption-layer/) — Key pattern for external integrations
- [Integration Patterns](../integration-patterns/) — Detailed description of each pattern
- [Domain Events](../domain-events/) — Primary inter-context communication mechanism

## References

- Evans, Eric. "Domain-Driven Design." Chapter 14. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 3. Addison-Wesley, 2013.
- Vernon, Vaughn. "Domain-Driven Design Distilled." Chapter 4. Addison-Wesley, 2016.
