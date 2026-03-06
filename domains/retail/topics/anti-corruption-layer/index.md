# Anti-Corruption Layer

## Overview

An anti-corruption layer (ACL) is a translation boundary that protects a bounded context from the models, concepts, and language of external systems or other bounded contexts. In the retail domain, anti-corruption layers are critical because retail systems integrate with numerous external services including payment processors, tax engines, supplier EDI systems, GS1 product data pools, and legacy point-of-sale systems. Without anti-corruption layers, external concepts leak into the domain model and corrupt its clarity.

## Retail Anti-Corruption Layer Patterns

### Payment Processor Integration

The Point of Sale Context communicates with external payment processors (e.g., Worldpay, Adyen, Stripe) that use their own models for authorization, capture, settlement, and chargebacks. The ACL translates between the POS domain's concept of a "tender" and the payment processor's concept of a "transaction." The POS domain model never directly references processor-specific fields like acquirer reference numbers or card scheme identifiers. Instead, the ACL maps these into the POS domain's own value objects.

### Tax Engine Integration

Retail systems integrate with external tax calculation services (e.g., Avalara, Vertex) that model tax jurisdictions, tax rates, product taxability, and exemptions in their own terms. The ACL translates between the retail domain's concept of a "line item with a tax amount" and the tax engine's concept of "tax jurisdiction nexus" and "tax category codes." The domain model knows about tax amounts but not about the internal workings of tax jurisdiction determination.

### Supplier EDI Integration

Suppliers communicate through Electronic Data Interchange (EDI) using standard document types: 850 (Purchase Order), 810 (Invoice), 856 (Advance Ship Notice). The ACL translates between the retail domain's Purchase Order aggregate and the EDI 850 document structure. Similarly, it translates incoming EDI 856 documents into the domain's Receiving events. The domain model uses retail terminology, not EDI segment and element codes.

### GS1 Product Data Integration

Product data from GS1 data pools uses GTIN (Global Trade Item Number), GPC (Global Product Classification), and standardized attribute schemas. The ACL translates between the external GS1 product data model and the internal Product Catalog Context's product model. The catalog context may enrich, transform, or selectively adopt GS1 attributes based on its own category and attribute definitions.

### Legacy System Integration

Many retailers operate legacy POS systems, mainframe-based inventory systems, or older ERP platforms. The ACL insulates new bounded contexts from the legacy models. For example, a legacy inventory system may represent products using internal codes and proprietary data formats. The ACL translates these into the modern Inventory Context's SKU and location model.

## Design Considerations

Anti-corruption layers in retail should be implemented as a distinct layer with clear interfaces. The ACL typically includes adapters that handle protocol translation, mappers that convert between external and internal models, and facades that present a simplified interface to the domain. The ACL should be testable independently, with contracts that verify the translation logic against known external data formats.

Performance is a concern in retail ACLs, particularly for real-time integrations like price lookups and payment authorizations. Caching strategies, circuit breakers, and fallback mechanisms ensure that ACL failures do not cascade into the domain.

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/index.md): ACLs protect bounded context boundaries.
- [Context Map](../context-map/index.md): ACLs appear on the context map at integration points.
- [Strategic Design](../strategic-design/index.md): ACL placement is a strategic design decision.
- [Integration Patterns](../integration-patterns/index.md): ACLs work alongside other integration patterns.
- [Retail Standards](../retail-standards/index.md): ACLs translate between retail standards and domain models.
- [Regulatory Compliance](../regulatory-compliance/index.md): ACLs help isolate compliance-specific logic from domain logic.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 14: Maintaining Model Integrity.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 3: Context Maps, section on Anti-Corruption Layer.
- Hohpe, Gregor, and Bobby Woolf. *Enterprise Integration Patterns*. Addison-Wesley, 2003.
