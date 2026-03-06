# Context Map

## Overview

A context map is a visual and conceptual representation of the relationships and interactions between bounded contexts in a system. In the retail domain, the context map shows how Product Catalog, Pricing and Promotions, Point of Sale, Customer and Loyalty, Inventory and Merchandising, and Omnichannel contexts relate to each other and to external systems such as payment processors, tax services, and supplier systems.

The context map captures not just data flows but also the nature of the relationships: which context is upstream and which is downstream, which contexts share a kernel, and where anti-corruption layers are needed to translate between incompatible models.

## Retail Context Map Relationships

**Product Catalog to Pricing and Promotions**: The Product Catalog Context is upstream, publishing product identifiers, categories, and attributes. The Pricing and Promotions Context is downstream, consuming product data to associate prices and promotions. This is a customer-supplier relationship where the catalog team must consider the needs of the pricing team.

**Product Catalog to Inventory and Merchandising**: The Product Catalog Context publishes product and SKU definitions. The Inventory Context consumes these to track stock levels. This is a conformist relationship where the Inventory Context adopts the product model defined by the catalog.

**Pricing and Promotions to Point of Sale**: The Pricing Context is upstream, providing price lookup and promotion evaluation services. The Point of Sale Context is downstream, requesting prices during transaction processing. This operates as an open host service with a published language for price requests and responses.

**Customer and Loyalty to Point of Sale**: The Customer and Loyalty Context provides customer identification and loyalty point services. The Point of Sale Context integrates these during checkout to apply member discounts and accrue points.

**Inventory and Merchandising to Omnichannel**: The Inventory Context publishes real-time stock availability. The Omnichannel Context consumes this to offer BOPIS, ship-from-store, and endless aisle capabilities.

**Omnichannel to All Contexts**: The Omnichannel Context acts as an orchestration layer that coordinates across Product Catalog (product display), Pricing (price display), Inventory (availability), and Customer and Loyalty (personalization).

**External System Integrations**: Payment processors (PCI DSS compliant), tax calculation services, supplier EDI systems (850/810/856), and GS1 product data pools connect to the retail system through anti-corruption layers that translate external models into the retail domain's language.

## Mapping Patterns in Retail

- **Shared Kernel**: Product Catalog and Inventory may share a kernel for SKU identifiers.
- **Customer-Supplier**: Catalog upstream to Pricing downstream.
- **Conformist**: Inventory conforms to the product model published by Catalog.
- **Open Host Service**: Pricing exposes a published language for price calculation.
- **Anti-Corruption Layer**: Point of Sale protects itself from external payment processor models.
- **Published Language**: GS1 standards (GTIN, UPC, EAN) serve as a published language for product identification across contexts and external systems.

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/index.md): The contexts that the map connects.
- [Strategic Design](../strategic-design/index.md): Context mapping is a strategic design activity.
- [Anti-Corruption Layer](../anti-corruption-layer/index.md): A key pattern used at context boundaries.
- [Integration Patterns](../integration-patterns/index.md): Detailed patterns for context integration.
- [Ubiquitous Language](../ubiquitous-language/index.md): Each context on the map uses its own language.
- [Event-Driven Architecture](../event-driven-architecture/index.md): Events flow along the paths defined in the context map.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 14: Maintaining Model Integrity.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 3: Context Maps.
- Brandolini, Alberto. *EventStorming*. Leanpub, 2021. Context mapping through collaborative workshops.
