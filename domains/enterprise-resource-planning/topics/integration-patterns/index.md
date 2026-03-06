# Integration Patterns in Enterprise Resource Planning

## Overview

Integration patterns define how ERP bounded contexts communicate and share data. ERP systems are highly interconnected — sales drives inventory, procurement feeds finance, manufacturing consumes from inventory and produces for sales. The choice of integration pattern for each relationship determines coupling, autonomy, and resilience.

## DDD Integration Patterns in ERP

### Customer-Supplier

The dominant pattern in ERP. The upstream context provides data that the downstream context depends on.

- **Sales → Finance**: Sales publishes OrderShipped; Finance creates revenue entries
- **Procurement → Finance**: Procurement publishes GoodsReceived; Finance creates accruals
- **HR → Finance**: HR publishes PayrollProcessed; Finance creates expense entries
- **Sales → Inventory**: Sales publishes OrderPlaced; Inventory reserves stock

### Partnership

Bidirectional relationship where both teams coordinate closely.

- **Manufacturing ↔ Inventory**: Manufacturing consumes materials and produces finished goods. Both teams coordinate on BOM changes, material availability, and production receipts.

### Shared Kernel

Small shared model maintained by multiple teams.

- **Product Master Data**: A minimal shared product identifier (ProductID, name, UOM) shared across Sales, Inventory, Procurement, and Manufacturing. Each context extends this with context-specific attributes.

### Anti-Corruption Layer

Translation boundary for external systems.

- **Banking APIs → Finance**: Bank statement formats translated to domain transactions
- **Tax Authority → Finance**: Tax rate tables and filing formats
- **Supplier Portals → Procurement**: Catalog and order confirmation formats
- **Logistics Providers → Inventory**: Shipping and tracking APIs

### Published Language

Standardized data formats for cross-context communication.

- **EDI formats**: Used for B2B transactions (purchase orders, invoices, ASNs)
- **XBRL**: Financial reporting exchange format
- **UBL**: Universal Business Language for procurement documents

### Open Host Service

A context exposes a well-defined API for multiple consumers.

- **Product Catalog Service**: Exposes product data consumed by Sales, Procurement, and external partners
- **Customer Service**: Exposes customer data consumed by Sales, Finance (AR), and external CRM

## ERP Integration Map

| Upstream | Downstream | Pattern | Mechanism |
|----------|-----------|---------|-----------|
| Sales | Finance | Customer-Supplier | Domain Events |
| Sales | Inventory | Customer-Supplier | Domain Events |
| Procurement | Finance | Customer-Supplier | Domain Events |
| Procurement | Inventory | Customer-Supplier | Domain Events |
| Manufacturing | Inventory | Partnership | Domain Events |
| HR | Finance | Customer-Supplier | Domain Events |
| Manufacturing | Procurement | Customer-Supplier | Domain Events |
| Product Master | All contexts | Shared Kernel | Shared Data |
| Banking APIs | Finance | ACL | File Import / API |
| Logistics | Inventory | Open Host Service | REST API |

## Relationships to Other Topics

- [Context Map](../context-map/) — Integration patterns are documented on the context map
- [Anti-Corruption Layer](../anti-corruption-layer/) — Detailed ACL documentation
- [Domain Events](../domain-events/) — Primary mechanism for asynchronous integration
- [Event-Driven Architecture](../event-driven-architecture/) — Infrastructure supporting event-based integration
- [ERP Standards](../erp-standards/) — EDI, XBRL, UBL serve as published languages

## References

- Evans, Eric. "Domain-Driven Design." Chapter 14. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 3. Addison-Wesley, 2013.
- Hohpe, Gregor & Woolf, Bobby. "Enterprise Integration Patterns." Addison-Wesley, 2003.
