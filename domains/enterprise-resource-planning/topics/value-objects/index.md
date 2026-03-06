# Value Objects in Enterprise Resource Planning

## Overview

A value object is an immutable domain object defined by its attributes rather than identity. Two value objects with the same attributes are equal. In ERP, value objects represent the quantities, amounts, codes, and descriptive data that flow through every transaction.

## ERP Value Objects

### Money

- **Attributes**: Amount (decimal), currency code (ISO 4217)
- **Behavior**: add(), subtract(), multiply(), allocate() — all return new Money instances
- **Usage**: Prices, totals, payments, account balances, tax amounts
- **Example**: `Money(1500.00, "USD")`

### Quantity

- **Attributes**: Amount (decimal), unit of measure (each, kg, liter, meter)
- **Behavior**: add(), subtract(), convert() — unit-safe arithmetic
- **Usage**: Order line quantities, inventory levels, BOM quantities
- **Example**: `Quantity(100, "kg")`

### Address

- **Attributes**: Street, city, state/province, postal code, country
- **Usage**: Customer shipping/billing, supplier locations, warehouse addresses
- **Example**: `Address("456 Commerce Blvd", "Chicago", "IL", "60601", "US")`

### TaxRate

- **Attributes**: Rate (percentage), jurisdiction, tax type (sales, VAT, GST), effective date range
- **Usage**: Tax calculation on orders and invoices
- **Example**: `TaxRate(8.25, "TX", "sales", DateRange("2026-01-01", "2026-12-31"))`

### DateRange

- **Attributes**: Start date, end date
- **Validation**: Start must not be after end
- **Usage**: Fiscal periods, contract terms, coverage periods, price validity

### PaymentTerms

- **Attributes**: Due days, discount percentage, discount days, description
- **Usage**: Customer and supplier payment conditions
- **Example**: `PaymentTerms(30, 2.0, 10, "2/10 Net 30")` — 2% discount if paid within 10 days, net due in 30

### DeliveryTerms (Incoterms)

- **Attributes**: Incoterm code (FOB, CIF, EXW, DDP), named place
- **Usage**: Purchase orders, sales orders
- **Example**: `DeliveryTerms("FOB", "Shanghai Port")`

### AccountReference

- **Attributes**: Account number, cost center (optional), project code (optional)
- **Usage**: Journal entry line items, cost allocation
- **Example**: `AccountReference("5100", "CC-200", "PRJ-42")`

### BOMReference

- **Attributes**: Product ID, version, effective date
- **Usage**: Manufacturing work orders referencing a specific BOM version

## Design Benefits

- **Type safety**: `Money` prevents adding dollars to kilograms
- **Self-validating**: Invalid values are rejected at construction
- **Immutable**: Thread-safe, cacheable, no accidental mutations
- **Expressive**: `PaymentTerms(30, 2.0, 10, "2/10 Net 30")` is clearer than four loose fields

## Relationships to Other Topics

- [Entities](../entities/) — Entities contain value objects
- [Aggregates](../aggregates/) — Value objects are part of aggregates
- [Ubiquitous Language](../ubiquitous-language/) — Value object names come from domain vocabulary
- [ERP Standards](../erp-standards/) — Standard codes (ISO 4217, Incoterms) modeled as value objects

## References

- Evans, Eric. "Domain-Driven Design." Chapter 5. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 6. Addison-Wesley, 2013.
