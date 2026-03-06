# Value Objects

## Overview

A value object is an immutable domain object defined by its attributes rather than by a unique identity. Two value objects with the same attributes are considered equal. In the retail domain, value objects represent the measurements, quantities, monetary amounts, codes, and descriptive elements that enrich entities and aggregates. Value objects encapsulate validation logic and business rules, ensuring that invalid data cannot exist in the domain model.

## Retail Value Objects by Bounded Context

### Product Catalog Context

**GTIN (Global Trade Item Number)**: Encapsulates a product's global identifier, validating the check digit according to GS1 standards. Supports GTIN-8, GTIN-12 (UPC-A), GTIN-13 (EAN), and GTIN-14 formats. A GTIN is immutable and equality is based on the numeric value.

**ProductAttribute**: Represents a named attribute with a typed value, such as "Color: Navy Blue" or "Material: 100% Cotton." Attributes are immutable and defined by their name-value pair.

**Dimensions**: Encapsulates length, width, height, and weight with their respective units of measure. Used for shipping calculations and planogram fitting. Validated to ensure positive values and consistent unit systems.

**Barcode**: Encapsulates a scannable code (UPC-A, EAN-13, Code 128, QR) with its encoding format. Validated for format correctness and check digit integrity.

### Pricing and Promotions Context

**Money**: Represents a monetary amount with a currency code. Enforces precision rules appropriate for the currency (e.g., two decimal places for USD, zero for JPY). Supports arithmetic operations that maintain currency consistency and prevent mixed-currency calculations.

**PriceEntry**: Encapsulates a regular price, sale price, and effective date range for a product in a price zone. Validated to ensure the sale price does not exceed the regular price and that date ranges are coherent.

**DiscountAmount**: Represents either a percentage discount or a fixed-amount discount. Validated to ensure percentages are between 0 and 100, and fixed amounts are non-negative.

**DateRange**: Encapsulates a start date and end date pair. Validated to ensure the start does not follow the end. Used extensively for promotion periods, price effective dates, and seasonal assortment windows.

### Point of Sale Context

**TenderAmount**: Represents the amount tendered in a specific payment method (cash, credit, debit, gift card, mobile payment). Includes the tender type and the amount. Validated to ensure the amount is non-negative.

**TaxAmount**: Encapsulates a tax jurisdiction, tax rate, taxable amount, and calculated tax. Immutable once calculated. Equality is based on all component values.

**ReceiptLine**: Represents a formatted line on a customer receipt, including item description, quantity, unit price, and extended price.

### Customer and Loyalty Context

**EmailAddress**: Encapsulates a validated email address. Equality is case-insensitive. The value object rejects syntactically invalid email formats at construction time.

**PhoneNumber**: Encapsulates a phone number with country code, area code, and subscriber number. Validated for format and length according to the country's numbering plan.

**LoyaltyPoints**: Represents a points quantity with the point type (base, bonus, promotional). Supports addition and subtraction operations. Validated to ensure non-negative results.

**TierLevel**: Represents a loyalty tier (e.g., Bronze, Silver, Gold, Platinum) with its associated benefits. Equality is based on the tier name.

### Inventory and Merchandising Context

**Quantity**: Represents a count of items with a unit of measure (each, case, pallet). Validated for non-negative values. Supports arithmetic operations that maintain unit consistency.

**LocationCode**: Encapsulates a store number, warehouse code, or bin location. Validated for format correctness according to the retailer's location coding scheme.

**PlanogramPosition**: Represents a shelf, section, and position within a planogram. Immutable and defined by its spatial coordinates within the fixture.

## Design Principles

Value objects in retail should be truly immutable. Operations that would modify a value object return a new instance. For example, adding a DiscountAmount to a Money value returns a new Money value; neither input is modified.

Value objects enforce their own validity. A Money value object with a negative amount or an invalid currency code cannot be constructed. This pushes validation to the boundary where data enters the system and guarantees that the domain model always operates on valid data.

## Relationships to Other Topics

- [Entities](../entities/index.md): Value objects are attributes of entities.
- [Aggregates](../aggregates/index.md): Value objects exist within aggregates.
- [Domain Events](../domain-events/index.md): Domain events carry value objects as payload data.
- [Ubiquitous Language](../ubiquitous-language/index.md): Value object names reflect the ubiquitous language.
- [Retail Standards](../retail-standards/index.md): Many value objects encode retail standards like GTIN and currency codes.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 5: A Model Expressed in Software.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 6: Value Objects.
- GS1. *GS1 General Specifications*. GS1 AISBL, ongoing. Standards for GTIN, barcode encoding, and product data.
