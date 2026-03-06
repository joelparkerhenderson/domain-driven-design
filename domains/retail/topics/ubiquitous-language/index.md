# Ubiquitous Language

## Overview

Ubiquitous language is the shared vocabulary used by domain experts, developers, and stakeholders within a bounded context. In the retail domain, ubiquitous language ensures that terms like "SKU," "markdown," "planogram," "tender," and "loyalty tier" carry precise, agreed-upon meanings within their respective contexts. This shared language is embedded in the code, in conversations, in documentation, and in the domain model itself.

The importance of ubiquitous language in retail cannot be overstated. Retail systems involve many specialized terms from merchandising, supply chain, store operations, marketing, and finance. Misunderstandings about what "assortment" means, or whether "discount" refers to a promotional discount or an employee discount, can lead to costly software defects and business errors.

## Retail Language by Bounded Context

**Product Catalog Context**: Product, SKU (Stock Keeping Unit), category, attribute, variant, assortment, product hierarchy, product lifecycle, item master, product family, brand, style, color, size.

**Pricing and Promotions Context**: List price, sale price, markdown, clearance, promotional price, coupon, discount, buy-one-get-one (BOGO), price zone, price override, price point, margin, markup, price sensitivity, price ladder.

**Point of Sale Context**: Transaction, line item, tender, payment, receipt, register, cash drawer, void, return, exchange, refund, cashier, barcode scan, subtotal, tax, total, change due, end-of-day reconciliation.

**Customer and Loyalty Context**: Customer, member, loyalty program, points, tier (bronze, silver, gold, platinum), earn rate, burn rate, redemption, reward, customer segment, lifetime value, enrollment, opt-in, preference.

**Inventory and Merchandising Context**: On-hand quantity, allocated quantity, available-to-sell, reorder point, safety stock, replenishment, purchase order, receiving, transfer, shrinkage, planogram, shelf space, facing, fixture, merchandise hierarchy.

**Omnichannel Context**: Channel, touchpoint, buy-online-pick-up-in-store (BOPIS), ship-from-store, endless aisle, unified cart, order orchestration, fulfillment, click-and-collect, curbside pickup, same-day delivery, channel attribution.

## Building and Maintaining Ubiquitous Language

Building ubiquitous language in retail requires ongoing collaboration between business and technology teams. Merchandisers define catalog terminology. Store operations teams define POS terminology. Marketing teams define promotional terminology. The DDD practice ensures these terms are codified in the domain model and enforced in code reviews, tests, and documentation.

Language conflicts between contexts are expected and healthy. The word "item" means a product definition in the Catalog Context but means a line on a transaction in the POS Context. Bounded contexts resolve these conflicts by giving each context ownership of its own language.

Glossaries maintained per bounded context serve as living reference documents. These glossaries evolve as the business evolves, incorporating new terms for emerging capabilities like social commerce, live shopping, or recommerce.

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/index.md): Each bounded context owns its own ubiquitous language.
- [Strategic Design](../strategic-design/index.md): Language alignment is a key strategic design concern.
- [Context Map](../context-map/index.md): Language translation occurs at context boundaries shown on the map.
- [Anti-Corruption Layer](../anti-corruption-layer/index.md): Translates between the languages of different contexts.
- [Entities](../entities/index.md): Named using ubiquitous language terms.
- [Value Objects](../value-objects/index.md): Named using ubiquitous language terms.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 2: Communication and the Use of Language.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 1: Getting Started with DDD, section on Ubiquitous Language.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly, 2021. Chapter 2: Discovering Domain Knowledge.
- ARTS (Association for Retail Technology Standards). *Retail Data Model and Terminology*. NRF, ongoing.
