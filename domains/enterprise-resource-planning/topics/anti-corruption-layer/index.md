# Anti-Corruption Layer in Enterprise Resource Planning

## Overview

An anti-corruption layer (ACL) is a translation boundary that protects a bounded context's domain model from external system concepts. In ERP, ACLs are essential because ERP systems integrate with banks, tax authorities, supplier portals, logistics providers, and legacy systems — each with its own data formats and terminology.

## ERP ACL Examples

### Banking Systems → Finance Context

Banks provide statement data in proprietary formats (MT940, CAMT.053, BAI2). The ACL:

- Parses bank-specific statement formats into domain `BankTransaction` value objects
- Maps bank transaction codes to internal transaction categories
- Reconciles bank transactions against internal ledger entries
- Shields the Finance domain from changes in individual bank APIs

### Tax Authority Systems → Finance Context

Tax filings require specific formats per jurisdiction. The ACL:

- Translates internal financial data into jurisdiction-specific tax formats
- Maps internal account categories to tax reporting categories
- Generates required forms (1099, VAT returns, GST filings) from domain objects

### Supplier Portals → Procurement Context

Suppliers provide catalogs and order confirmations in various formats (cXML, OCI, proprietary). The ACL:

- Translates supplier catalog formats into internal `Product` and `Pricing` value objects
- Converts supplier order confirmations into domain `PurchaseOrderConfirmation` events
- Maps supplier-specific status codes to internal `OrderStatus`

### Logistics Providers → Inventory Context

Shipping carriers use proprietary APIs for tracking and rate calculation. The ACL:

- Translates carrier-specific tracking data into domain `ShipmentStatus` events
- Converts rate quotes from multiple carriers into standardized `ShippingRate` value objects
- Maps carrier service levels to internal `ShippingMethod` options

### Legacy ERP Systems

During migration from a legacy ERP, the ACL:

- Translates legacy data structures into new domain models
- Maps legacy codes and identifiers to new system identifiers
- Handles data quality issues in legacy data (missing fields, inconsistent formats)

## When to Use an ACL

- External systems with significantly different models
- Systems controlled by other organizations that may change independently
- Multiple external systems serving the same purpose (multiple banks, carriers)
- Legacy system migration

## Relationships to Other Topics

- [Context Map](../context-map/) — ACLs appear as patterns on the context map
- [Bounded Contexts](../bounded-contexts/) — ACLs protect context boundaries
- [Integration Patterns](../integration-patterns/) — ACL is one of several integration patterns
- [ERP Standards](../erp-standards/) — Standards define the external formats ACLs translate

## References

- Evans, Eric. "Domain-Driven Design." Chapter 14. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 3. Addison-Wesley, 2013.
- Hohpe, Gregor & Woolf, Bobby. "Enterprise Integration Patterns." Addison-Wesley, 2003.
