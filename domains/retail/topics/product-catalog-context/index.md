# Product Catalog Context

## Overview

The Product Catalog Context is the bounded context responsible for product definitions, categories, attributes, variants, and assortment management. It serves as the system of record for what products exist, how they are described, and how they are organized. Other bounded contexts in the retail domain depend on the Product Catalog Context for product identity and descriptive data, making it a foundational upstream context in the retail context map.

## Core Concepts

**Product**: The central entity representing a sellable item. A product has a unique identifier (often aligned with a GTIN), a name, a description, a brand, and a lifecycle status (draft, active, discontinued). Products are organized within a category hierarchy and carry attributes defined by their category.

**Variant (SKU)**: A specific sellable configuration of a product. For apparel, variants represent size and color combinations. For electronics, variants represent configurations (storage capacity, color). Each variant has its own SKU code, barcode, dimensions, and availability status. Variants are entities within the Product aggregate.

**Category Hierarchy**: A tree structure organizing products into departments, classes, subclasses, and categories. Examples include Department: Apparel, Class: Women's, Subclass: Dresses, Category: Casual Dresses. The category hierarchy drives attribute requirements, reporting structures, and merchandising views.

**Product Attributes**: Typed key-value pairs that describe product characteristics. Attributes may be defined at the category level (required attributes for the "Footwear" category might include Material, Sole Type, and Heel Height) or at the product level. Attributes are value objects.

**Assortment**: The selection of products available in a particular store, channel, or region. Assortment planning determines which products from the master catalog are active in which locations. An assortment is not a simple filter; it includes decisions about depth (how many variants) and breadth (how many products).

## Aggregates

The **Product Aggregate** is rooted at the Product entity and contains Variant entities and ProductAttribute value objects. The aggregate enforces invariants: variant SKU uniqueness within the product, required attribute completeness for the product's category, and lifecycle state transitions (a discontinued product cannot have new variants added).

The **Category Aggregate** manages the hierarchy structure, attribute definitions for the category, and validation rules. It enforces that the hierarchy is acyclic and that attribute names are unique within a category.

## Domain Events

- ProductCreated: Published when a new product is added to the catalog.
- ProductUpdated: Published when product attributes or descriptions change.
- ProductDiscontinued: Published when a product is removed from active selling.
- VariantAdded: Published when a new SKU is added to an existing product.
- AssortmentChanged: Published when the product assortment for a location or channel changes.

## Integration with Other Contexts

The Product Catalog Context is upstream to most other retail contexts. It publishes product data consumed by:

- **Pricing and Promotions Context**: Consumes product identifiers and categories to associate prices and promotions.
- **Inventory and Merchandising Context**: Consumes SKU definitions to track stock levels.
- **Omnichannel Context**: Consumes product descriptions, images, and attributes for digital display.
- **Point of Sale Context**: Consumes product identifiers and barcodes for scanning and lookup.

Data synchronization with external systems uses GS1 standards. The GTIN serves as a universal product identifier. GS1 Global Data Synchronisation Network (GDSN) data pools provide standardized supplier product data that the catalog context ingests through an anti-corruption layer.

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/index.md): The Product Catalog Context is one of six core retail bounded contexts.
- [Aggregates](../aggregates/index.md): Product and Category are the primary aggregates in this context.
- [Entities](../entities/index.md): Product, Variant, and Category are key entities.
- [Value Objects](../value-objects/index.md): GTIN, Barcode, ProductAttribute, and Dimensions are value objects.
- [Domain Events](../domain-events/index.md): ProductCreated, ProductUpdated, and related events.
- [Retail Standards](../retail-standards/index.md): GS1 standards for product identification.
- [Context Map](../context-map/index.md): Product Catalog is an upstream context.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- GS1. *GS1 General Specifications*. GS1 AISBL, ongoing. Product identification, GTIN allocation rules, and data synchronisation.
- ARTS (Association for Retail Technology Standards). *ARTS Data Model*. NRF, ongoing. Retail product data model standards.
