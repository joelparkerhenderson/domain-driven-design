# Ubiquitous Language in Enterprise Resource Planning

## Overview

Ubiquitous language is the shared vocabulary that developers and business domain experts use consistently in conversation, documentation, and code. In ERP, this means that accountants, warehouse managers, procurement specialists, and software engineers all use the same terms with the same meaning within each bounded context.

## Relevance to ERP

ERP systems span multiple business disciplines, each with precise terminology:

- Accountants speak of debits, credits, accruals, and fiscal periods
- Warehouse staff speak of SKUs, bin locations, pick lists, and cycle counts
- Procurement uses requisitions, RFQs, purchase orders, and lead times
- Manufacturing uses BOMs, work orders, routings, and work centers

The ubiquitous language respects these existing vocabularies and embeds them in the software model.

## One Language per Context

The term "Order" means different things in different contexts:

- **Sales Context**: SalesOrder — a customer's request to purchase goods
- **Procurement Context**: PurchaseOrder — an authorization to buy from a supplier
- **Manufacturing Context**: WorkOrder — an instruction to produce goods
- **Inventory Context**: TransferOrder — an instruction to move stock between locations

This is not a problem — it reflects reality. Each context uses the term that is natural to its domain experts.

## Language in Code

| Domain Term | Code Representation | Context |
|------------|-------------------|---------|
| General Ledger | `GeneralLedger` class | Finance |
| Journal Entry | `JournalEntry` aggregate | Finance |
| Purchase Order | `PurchaseOrder` aggregate | Procurement |
| Sales Order | `SalesOrder` aggregate | Sales |
| Bill of Materials | `BillOfMaterials` aggregate | Manufacturing |
| Stock Keeping Unit | `SKU` value object | Inventory |
| Employee | `Employee` aggregate | HR |

Methods reflect domain operations: `journalEntry.post()`, `purchaseOrder.approve()`, `salesOrder.fulfill()`, `workOrder.complete()`.

Events use past-tense domain language: `OrderPlaced`, `InvoiceIssued`, `GoodsReceived`, `PayrollProcessed`.

See [language.md](../../language.md) for the full glossary.

## Relationships to Other Topics

- [Strategic Design](../strategic-design/) — Language is developed during strategic design
- [Bounded Contexts](../bounded-contexts/) — Each context has its own language dialect
- [Entities](../entities/) — Named using ubiquitous language terms
- [Domain Events](../domain-events/) — Named using past-tense domain language

## References

- Evans, Eric. "Domain-Driven Design." Chapter 2. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 1. Addison-Wesley, 2013.
