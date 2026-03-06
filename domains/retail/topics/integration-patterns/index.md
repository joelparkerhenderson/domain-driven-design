# Integration Patterns

## Overview

Integration patterns define how bounded contexts communicate, share data, and coordinate behavior. In the retail domain, integration patterns govern how the Product Catalog, Pricing, Point of Sale, Customer and Loyalty, Inventory, and Omnichannel contexts interact with each other and with external systems such as payment processors, suppliers, tax engines, and marketplace platforms. The choice of integration pattern at each boundary has profound implications for coupling, autonomy, and system resilience.

## DDD Integration Patterns in Retail

### Shared Kernel

Two bounded contexts share a subset of the domain model, typically a small set of value objects or identifiers. In retail, the Product Catalog Context and Inventory Context may share a kernel consisting of the SKU identifier and basic product classification. Changes to the shared kernel require coordination between both teams. This pattern is appropriate when two contexts are closely related and maintained by collaborating teams.

**Retail Example**: The SKU identifier is shared between Product Catalog and Inventory. Both contexts agree on the format, validation rules, and lifecycle of SKU codes. Changes to the SKU scheme require coordinated updates.

### Customer-Supplier

One context (supplier/upstream) provides data or services that another context (customer/downstream) depends on. The upstream context considers the downstream context's needs but maintains its own model. In retail, the Product Catalog Context is upstream to the Pricing Context. The catalog team publishes product data, and the pricing team consumes it. The catalog team should consider the pricing team's data needs when designing its published events.

**Retail Example**: Product Catalog publishes ProductCreated and ProductUpdated events with category and attribute data that the Pricing Context needs to associate prices. The catalog team ensures these events contain the data the pricing team requires.

### Conformist

The downstream context adopts the upstream context's model without translation. The downstream team has no influence over the upstream model. This pattern is common when integrating with external systems or third-party services where the retailer has no negotiating power.

**Retail Example**: A retailer integrating with a major marketplace (Amazon, Walmart) typically conforms to the marketplace's product listing model, category taxonomy, and order format. The retailer adapts its data to the marketplace's requirements.

### Anti-Corruption Layer

The downstream context builds a translation layer to protect its own model from the upstream context's model. This is the most defensive integration pattern and is appropriate when the upstream model is complex, unstable, or conceptually incompatible.

**Retail Example**: The POS Context integrates with external payment processors through an anti-corruption layer. The payment processor's model of authorizations, captures, reversals, and settlement is translated into the POS domain's simpler model of tenders and payment confirmations.

### Open Host Service

The upstream context provides a well-defined service interface using a published language that any downstream context can consume. The service interface is stable and versioned, designed for broad consumption.

**Retail Example**: The Pricing Context exposes a PriceCalculation service as an open host service. Any context that needs a price (POS, Omnichannel, mobile app) can call this service using the published request/response format.

### Published Language

A shared, well-documented data format used for communication between contexts or with external systems. Published languages are often based on industry standards.

**Retail Examples**:
- GS1 standards (GTIN, GLN, SSCC) serve as a published language for product and location identification across the retail supply chain.
- EDI document standards (ANSI X12 or EDIFACT) serve as a published language for supplier integration: 850 (Purchase Order), 810 (Invoice), 856 (Advance Ship Notice).
- ARTS (Association for Retail Technology Standards) data models serve as a published language for retail-specific data exchange.

## Synchronous vs. Asynchronous Integration

**Synchronous Integration**: Request-response patterns where the caller waits for a response. Used when the result is needed immediately, such as price lookups during checkout or payment authorization during tendering. Synchronous integration creates temporal coupling; the caller is blocked if the service is slow or unavailable.

**Asynchronous Integration**: Event-based or message-based patterns where the sender does not wait for a response. Used for operations where eventual consistency is acceptable, such as inventory updates after a sale, loyalty point accrual after a transaction, or catalog data synchronization across channels. Asynchronous integration reduces coupling but requires handling of eventual consistency, ordering, and idempotency.

**Retail Guidance**: Prefer asynchronous integration between bounded contexts. Use synchronous integration only when the business requires an immediate response (price lookup, payment authorization, real-time inventory check for BOPIS).

## Integration with External Systems

Retail systems integrate with numerous external parties. Each integration point requires an explicit integration pattern choice:

- **Payment Service Providers**: Anti-corruption layer with synchronous calls for authorization and capture.
- **Tax Calculation Services**: Anti-corruption layer with synchronous calls during transaction processing.
- **Suppliers**: Published language (EDI 850/810/856) with asynchronous message exchange.
- **Marketplaces**: Conformist or anti-corruption layer depending on the retailer's leverage.
- **GS1 Data Pools**: Published language for product data synchronization.

## Relationships to Other Topics

- [Anti-Corruption Layer](../anti-corruption-layer/index.md): A specific defensive integration pattern.
- [Context Map](../context-map/index.md): Integration patterns are documented on the context map.
- [Bounded Contexts](../bounded-contexts/index.md): Integration occurs at context boundaries.
- [Event-Driven Architecture](../event-driven-architecture/index.md): Asynchronous integration through events.
- [Domain Events](../domain-events/index.md): Events are the medium for asynchronous integration.
- [Retail Standards](../retail-standards/index.md): Standards often serve as published languages.
- [Ubiquitous Language](../ubiquitous-language/index.md): Integration requires language translation at boundaries.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 14: Maintaining Model Integrity.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 13: Integrating Bounded Contexts.
- Hohpe, Gregor, and Bobby Woolf. *Enterprise Integration Patterns*. Addison-Wesley, 2003.
- GS1. *GS1 General Specifications*. GS1 AISBL, ongoing.
