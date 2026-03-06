# Retail - Domain-Driven Design

## Overview

This domain applies Domain-Driven Design (DDD) to retail systems, modeling product catalog management, pricing and promotions, point-of-sale transactions, customer loyalty programs, inventory and merchandising, and omnichannel operations spanning physical stores, e-commerce, and mobile commerce.

## Bounded Contexts

- **Product Catalog Context**: Product definitions, categories, attributes, assortments, and catalog lifecycle
- **Pricing & Promotions Context**: Price management, markdowns, promotions, coupons, and dynamic pricing
- **Point of Sale Context**: Transaction processing, tender handling, register operations, and receipt generation
- **Customer & Loyalty Context**: Customer profiles, loyalty programs, points, tiers, and personalization
- **Inventory & Merchandising Context**: Stock management, replenishment, planograms, and merchandising
- **Omnichannel Context**: Cross-channel experience, BOPIS, ship-from-store, endless aisle, and channel orchestration

## Topics

### Strategic Design

- [Strategic Design](topics/strategic-design/) — Strategic DDD patterns applied to retail
- [Bounded Contexts](topics/bounded-contexts/) — Retail bounded context definitions
- [Context Map](topics/context-map/) — Relationships between retail contexts
- [Ubiquitous Language](topics/ubiquitous-language/) — Shared retail terminology
- [Subdomains](topics/subdomains/) — Core, supporting, and generic retail subdomains
- [Anti-Corruption Layer](topics/anti-corruption-layer/) — Translation boundaries with POS hardware, ERP, and suppliers

### Tactical Design

- [Aggregates](topics/aggregates/) — Retail aggregate roots and consistency boundaries
- [Entities](topics/entities/) — Identifiable retail domain objects
- [Value Objects](topics/value-objects/) — Immutable retail descriptive objects
- [Domain Events](topics/domain-events/) — Significant retail occurrences
- [Repositories](topics/repositories/) — Persistence abstractions for retail aggregates
- [Domain Services](topics/domain-services/) — Cross-aggregate retail operations

### Bounded Contexts

- [Product Catalog Context](topics/product-catalog-context/) — Product and assortment management
- [Pricing & Promotions Context](topics/pricing-promotions-context/) — Price and promotion management
- [Point of Sale Context](topics/point-of-sale-context/) — Transaction processing
- [Customer & Loyalty Context](topics/customer-loyalty-context/) — Loyalty programs and customer data
- [Inventory & Merchandising Context](topics/inventory-merchandising-context/) — Stock and merchandising management
- [Omnichannel Context](topics/omnichannel-context/) — Cross-channel operations

### Cross-Cutting Concerns

- [Event-Driven Architecture](topics/event-driven-architecture/) — Event flows between retail contexts
- [Integration Patterns](topics/integration-patterns/) — Integration with POS, ERP, suppliers, and e-commerce
- [Retail Standards](topics/retail-standards/) — GS1, PCI DSS, EDI, ARTS standards
- [Regulatory Compliance](topics/regulatory-compliance/) — PCI DSS, consumer protection, tax, and privacy regulations

## References

- Evans, Eric. "Domain-Driven Design." Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." O'Reilly, 2021.
