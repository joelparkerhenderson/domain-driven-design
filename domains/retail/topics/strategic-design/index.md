# Strategic Design

## Overview

Strategic design is the discipline within Domain-Driven Design that addresses how to decompose a large retail system into manageable, well-defined bounded contexts. In the retail domain, strategic design determines how product catalog, pricing, point of sale, customer loyalty, inventory, and omnichannel capabilities are organized into distinct models with clear boundaries, shared language, and deliberate integration patterns.

Retail systems are among the most complex enterprise landscapes because they span physical stores, digital channels, supply chains, and financial systems. Strategic design provides the tools to manage this complexity by identifying which parts of the domain deserve the most investment (core subdomains), which parts support core capabilities (supporting subdomains), and which parts can be sourced externally (generic subdomains).

## Key Concepts in Retail Strategic Design

**Domain Decomposition**: A retail enterprise decomposes into bounded contexts such as Product Catalog, Pricing and Promotions, Point of Sale, Customer and Loyalty, Inventory and Merchandising, and Omnichannel. Each context owns its own model and ubiquitous language. For example, "product" means something different in the Product Catalog Context (rich attribute descriptions, categories, images) versus the Inventory Context (SKU-level stock quantities and locations).

**Core Domain Identification**: The core domain for a retailer depends on its competitive strategy. A fashion retailer may treat merchandising and assortment planning as its core domain. A discount retailer may treat pricing optimization as its core. A direct-to-consumer brand may treat customer experience and loyalty as core. Strategic design forces explicit decisions about where to invest the strongest engineering talent.

**Subdomain Classification**: Retail subdomains typically classify as follows. Core subdomains include the capabilities that differentiate the retailer, such as personalized pricing, curated assortments, or omnichannel fulfillment. Supporting subdomains include inventory management, customer data management, and promotion engines. Generic subdomains include payment processing, tax calculation, and address validation.

**Context Mapping**: The relationships between retail bounded contexts form a context map. The Product Catalog Context publishes product data consumed by Pricing, Inventory, and Omnichannel contexts. The Point of Sale Context integrates with Pricing for price lookups and with Inventory for stock checks. The Customer and Loyalty Context feeds personalization data to the Omnichannel Context.

**Distillation**: Strategic design uses distillation to separate the essential domain logic from supporting infrastructure. In retail, the core logic of markdown optimization or loyalty tier calculation must be isolated from the plumbing of database access, API gateways, and message brokers.

## Retail-Specific Considerations

Retail strategic design must account for the multi-channel nature of modern commerce. A customer may browse products online, check in-store availability, purchase via a mobile app, and return in a physical store. Strategic design ensures each bounded context can serve all channels without tangling channel-specific logic into domain models.

Seasonal and promotional cycles drive significant complexity. Strategic design must accommodate rapid changes in assortment, pricing, and merchandising without requiring changes across all bounded contexts simultaneously. Loose coupling through domain events and published languages enables this agility.

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/index.md): The primary structural output of strategic design.
- [Subdomains](../subdomains/index.md): Classification of domain areas by strategic importance.
- [Context Map](../context-map/index.md): Visual representation of bounded context relationships.
- [Ubiquitous Language](../ubiquitous-language/index.md): The shared vocabulary established within each bounded context.
- [Anti-Corruption Layer](../anti-corruption-layer/index.md): Protective boundaries between contexts.
- [Integration Patterns](../integration-patterns/index.md): Mechanisms for context-to-context communication.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Part IV: Strategic Design.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 2: Domains, Subdomains, and Bounded Contexts.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly, 2021. Chapters on strategic design and bounded context integration.
- National Retail Federation. *Retail Technology Standards and Frameworks*. NRF, ongoing.
