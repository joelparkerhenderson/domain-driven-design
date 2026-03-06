# Aggregates

## Overview

An aggregate is a cluster of related domain objects treated as a single unit for the purpose of data changes and consistency enforcement. Each aggregate has a root entity that controls access to its internal objects and enforces invariants. In the retail domain, aggregates define the transactional boundaries within each bounded context, ensuring that business rules are consistently applied when products are defined, prices are set, transactions are processed, and inventory is managed.

## Retail Aggregates by Bounded Context

### Product Catalog Context

**Product Aggregate**: The Product aggregate is rooted at the Product entity and includes variants (size, color, style), product attributes, category assignments, and product images. The aggregate enforces invariants such as: every product must have at least one active variant, variant combinations must be unique within a product, and required attributes must be populated for the product's category. The Product aggregate does not include pricing or inventory data, which belong to their respective contexts.

**Category Aggregate**: The Category aggregate manages the product hierarchy. It enforces that category trees are acyclic, that a category has at most one parent, and that category codes are unique. Categories contain attribute definitions that govern which attributes products in that category must or may carry.

### Pricing and Promotions Context

**Price List Aggregate**: The Price List aggregate manages a collection of price entries for products within a price zone. It enforces that each product has at most one active price at any time, that markdown sequences only move downward, and that promotional prices have valid date ranges. The aggregate root is the Price List entity, containing Price Entry value objects.

**Promotion Aggregate**: The Promotion aggregate defines promotional campaigns with rules for eligibility, discount calculation, and stacking behavior. It enforces that promotion date ranges are valid, that discount percentages are within allowed bounds, and that incompatible promotions cannot be combined.

### Point of Sale Context

**Transaction Aggregate**: The Transaction aggregate is the central aggregate of the POS context. It is rooted at the Transaction entity and includes line items, tenders, tax calculations, and discounts. The aggregate enforces that the total of tenders equals or exceeds the transaction total, that voided items are properly accounted for, and that tax calculations are consistent with jurisdictional rules. The Transaction aggregate must maintain strong consistency because partial transactions are not acceptable.

### Customer and Loyalty Context

**Customer Aggregate**: The Customer aggregate manages the customer profile, contact information, communication preferences, and loyalty membership. The aggregate enforces that a customer has a unique identifier, that email addresses are validated, and that opt-in preferences are properly recorded for regulatory compliance.

**Loyalty Account Aggregate**: The Loyalty Account aggregate tracks point balances, tier status, and transaction history for a loyalty member. It enforces that point balances never go negative, that tier transitions follow defined rules, and that point expiration is applied according to program rules.

### Inventory and Merchandising Context

**Stock Position Aggregate**: The Stock Position aggregate tracks the on-hand, allocated, and available-to-sell quantities for a SKU at a location. It enforces that available-to-sell quantity equals on-hand minus allocated, that quantities never go negative, and that allocation requests are validated against available stock.

## Aggregate Design Principles in Retail

Retail aggregates should be designed to be small and focused. Large aggregates create contention in high-throughput scenarios like holiday sales periods. The Transaction aggregate in the POS context must complete quickly to avoid checkout delays. The Stock Position aggregate must handle concurrent allocation requests without deadlocking.

References between aggregates should be by identity, not by direct object reference. A Transaction line item references a product by its product identifier, not by holding a reference to the Product aggregate. This loose coupling allows aggregates in different bounded contexts to evolve independently.

## Relationships to Other Topics

- [Entities](../entities/index.md): Aggregate roots are entities with unique identities.
- [Value Objects](../value-objects/index.md): Aggregates contain value objects that carry no identity.
- [Domain Events](../domain-events/index.md): Aggregates emit domain events when significant state changes occur.
- [Repositories](../repositories/index.md): Each aggregate root has a corresponding repository for persistence.
- [Bounded Contexts](../bounded-contexts/index.md): Aggregates exist within bounded contexts.
- [Domain Services](../domain-services/index.md): Operations spanning multiple aggregates are handled by domain services.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 6: The Life Cycle of a Domain Object.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 10: Aggregates.
- Vernon, Vaughn. "Effective Aggregate Design." Three-part series, DDD Community, 2011.
