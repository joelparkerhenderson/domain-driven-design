# Bounded Contexts

## Overview

A bounded context is a logical boundary within which a particular domain model is defined and applicable. In the retail domain, bounded contexts separate the system into areas such as Product Catalog, Pricing and Promotions, Point of Sale, Customer and Loyalty, Inventory and Merchandising, and Omnichannel. Each bounded context has its own ubiquitous language, its own model, and its own rules for consistency.

The concept of bounded contexts is essential in retail because the same real-world concept carries different meanings across different parts of the business. A "product" in the Product Catalog Context is a richly described item with images, descriptions, categories, and attributes. In the Inventory Context, a "product" is a SKU with a quantity at a location. In the Pricing Context, a "product" is an identifier associated with price points, markdown schedules, and promotional rules.

## Retail Bounded Contexts

**Product Catalog Context**: Owns product definitions, hierarchical categories, product attributes, variant management (size, color, style), and assortment planning. This context defines the canonical product identity and publishes product data to other contexts.

**Pricing and Promotions Context**: Owns price lists, markdown schedules, promotional campaigns, coupon definitions, and price calculation rules. This context consumes product identifiers from the Product Catalog Context but maintains its own pricing model independent of product descriptions.

**Point of Sale Context**: Owns transaction processing, tender handling (cash, card, gift card, mobile payment), receipt generation, register operations, and cashier workflows. This context integrates with Pricing for price lookups and with Inventory for stock validation.

**Customer and Loyalty Context**: Owns customer profiles, loyalty program definitions, point accrual and redemption rules, tier management, and customer segmentation. This context provides customer data to the Omnichannel Context for personalization.

**Inventory and Merchandising Context**: Owns stock levels, warehouse and store locations, replenishment rules, purchase orders, receiving, and planogram definitions. This context tracks the physical reality of merchandise across the supply chain.

**Omnichannel Context**: Owns channel orchestration including buy-online-pick-up-in-store (BOPIS), ship-from-store, endless aisle, and unified cart experiences. This context coordinates across other contexts to deliver seamless cross-channel customer experiences.

## Defining Boundaries

Bounded context boundaries in retail are determined by analyzing where language diverges, where teams operate independently, and where consistency requirements differ. The Product Catalog Context requires strong consistency for product data publishing. The Point of Sale Context requires strong consistency for transaction processing. But cross-context consistency can be eventual, mediated through domain events.

Boundaries also align with organizational structure. A merchandising team owns the Product Catalog and Inventory contexts. A marketing team owns the Pricing and Promotions context. A store operations team owns the Point of Sale context. A digital team owns the Omnichannel context. These organizational boundaries naturally reinforce bounded context boundaries, following Conway's Law.

## Relationships to Other Topics

- [Strategic Design](../strategic-design/index.md): Bounded contexts are the primary output of strategic design.
- [Context Map](../context-map/index.md): Maps the relationships between bounded contexts.
- [Ubiquitous Language](../ubiquitous-language/index.md): Each bounded context defines its own ubiquitous language.
- [Anti-Corruption Layer](../anti-corruption-layer/index.md): Protects bounded contexts from external model leakage.
- [Aggregates](../aggregates/index.md): The tactical building blocks within each bounded context.
- [Domain Events](../domain-events/index.md): The mechanism for cross-context communication.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 14: Maintaining Model Integrity.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 2: Domains, Subdomains, and Bounded Contexts.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly, 2021. Chapter 6: Bounded Contexts.
