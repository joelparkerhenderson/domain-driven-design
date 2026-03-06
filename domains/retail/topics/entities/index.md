# Entities

## Overview

An entity is a domain object that is defined by its unique identity rather than by its attributes. Entities persist over time, and their attributes may change while their identity remains constant. In the retail domain, entities represent the core business objects that the system tracks across their lifecycle: products that are created, updated, and discontinued; transactions that are opened, modified, and settled; customers who register, purchase, and evolve over time.

## Retail Entities by Bounded Context

### Product Catalog Context

**Product**: Identified by a unique product identifier (often aligned with GTIN or internal item number). A product has a lifecycle from creation through active selling to discontinuation. Its attributes (description, images, categories) change over time, but its identity persists. The Product entity serves as an aggregate root.

**Category**: Identified by a unique category code. Categories define the hierarchical organization of the product assortment. A category's position in the hierarchy and its associated attributes may change, but its identity is stable.

**Variant**: Identified by a SKU (Stock Keeping Unit) code. A variant represents a specific sellable configuration of a product, such as a particular size and color combination. The variant is an entity within the Product aggregate.

### Pricing and Promotions Context

**Price List**: Identified by a price list identifier that typically corresponds to a price zone or channel. Price lists have effective date ranges and contain price entries. The Price List entity manages the collection of prices for a zone.

**Promotion**: Identified by a unique promotion code. Promotions have defined start and end dates, eligibility rules, and discount mechanics. The Promotion entity manages the lifecycle of a promotional campaign.

### Point of Sale Context

**Transaction**: Identified by a unique transaction number, typically composed of store number, register number, and sequence number. The Transaction entity tracks the complete lifecycle of a sale from initiation through tender to completion. It is the aggregate root of the POS context's central aggregate.

**Register**: Identified by a register number within a store. The Register entity tracks the operational state of a point-of-sale terminal, including its current status (open, closed, suspended) and the cashier currently signed on.

### Customer and Loyalty Context

**Customer**: Identified by a unique customer identifier. The Customer entity tracks the evolution of a customer's relationship with the retailer across all channels. Attributes like contact information, preferences, and segments change over time while the customer identity persists.

**Loyalty Account**: Identified by a loyalty account number or membership identifier. The Loyalty Account entity tracks the member's current tier, point balance, and account status throughout their membership lifecycle.

### Inventory and Merchandising Context

**Location**: Identified by a unique location code (store number, warehouse code, or distribution center identifier). The Location entity represents a physical site where inventory is held. Its attributes (address, operating hours, capabilities) may change, but its identity is stable.

**Purchase Order**: Identified by a unique purchase order number. The Purchase Order entity tracks the lifecycle of an order placed with a supplier, from creation through submission, acknowledgment, shipment, and receiving.

## Entity Design in Retail

Retail entities must be designed with careful attention to identity generation. Product identifiers often align with GS1 standards (GTIN-12, GTIN-13, GTIN-14). Transaction identifiers must be unique across a distributed POS estate. Customer identifiers must support deduplication and merge operations when the same customer is identified across channels.

Entities in retail carry behavior, not just data. A Transaction entity knows how to add a line item, apply a discount, and validate that tenders cover the total. A Product entity knows how to activate a new variant and enforce attribute requirements for its category.

## Relationships to Other Topics

- [Aggregates](../aggregates/index.md): Entities serve as aggregate roots or exist within aggregates.
- [Value Objects](../value-objects/index.md): Entities contain value objects as attributes.
- [Repositories](../repositories/index.md): Entities are persisted and retrieved through repositories.
- [Domain Events](../domain-events/index.md): Entities emit domain events on significant state changes.
- [Ubiquitous Language](../ubiquitous-language/index.md): Entity names come from the ubiquitous language.
- [Bounded Contexts](../bounded-contexts/index.md): Entity definitions are scoped to their bounded context.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 5: A Model Expressed in Software.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 5: Entities.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly, 2021. Chapter 5: Implementing Simple Business Logic.
