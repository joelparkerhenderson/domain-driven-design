# Entities in Enterprise Resource Planning

## Overview

An entity is a domain object defined by its unique identity rather than its attributes. Entities persist over time — their attributes change but their identity remains constant.

## ERP Entities

### Customer

- **Identity**: Customer Number (system-assigned or business-assigned)
- **Attributes**: Name, billing address, shipping address, credit limit, payment terms, tax status
- **Lifecycle**: Prospect → Active → Inactive / Suspended
- **Context**: Aggregate root in Sales/Order Context

### Supplier

- **Identity**: Supplier Number
- **Attributes**: Name, address, payment terms, lead times, quality rating, certifications
- **Lifecycle**: Evaluated → Approved → Active → Inactive / Blocked
- **Context**: Aggregate root in Procurement Context

### Product

- **Identity**: Product Number / SKU
- **Attributes**: Description, unit of measure, weight, dimensions, product category, status
- **Lifecycle**: Designed → Active → Discontinued → Obsolete
- **Context**: Referenced across Sales, Inventory, Procurement, and Manufacturing with context-specific views

### Employee

- **Identity**: Employee ID
- **Attributes**: Name, department, position, hire date, compensation, benefits, tax withholding
- **Lifecycle**: Hired → Active → (transfers, promotions) → Terminated / Retired
- **Context**: Aggregate root in HR Context

### Account (General Ledger)

- **Identity**: Account Number
- **Attributes**: Name, type (asset/liability/equity/revenue/expense), balance, status, cost center
- **Lifecycle**: Created → Active → Frozen → Closed
- **Context**: Entity in Finance/Accounting Context

### Warehouse

- **Identity**: Warehouse Code
- **Attributes**: Name, address, type (raw materials, finished goods, distribution), capacity
- **Lifecycle**: Established → Active → Decommissioned
- **Context**: Entity in Inventory/Warehouse Context

## Identity Strategies

- **Business-meaningful**: Customer numbers, supplier codes, account numbers — carry domain meaning
- **System-generated**: UUIDs or sequential IDs for orders, journal entries — no inherent business meaning
- **External**: Tax IDs, DUNS numbers — assigned by external authorities

## Relationships to Other Topics

- [Aggregates](../aggregates/) — Entities serve as aggregate roots
- [Value Objects](../value-objects/) — Entities contain value objects as attributes
- [Repositories](../repositories/) — Repositories retrieve entities
- [Ubiquitous Language](../ubiquitous-language/) — Entity names come from domain vocabulary

## References

- Evans, Eric. "Domain-Driven Design." Chapter 5. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 5. Addison-Wesley, 2013.
