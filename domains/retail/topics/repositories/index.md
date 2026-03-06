# Repositories

## Overview

A repository is an abstraction for storing and retrieving aggregate roots. Repositories provide a collection-like interface that hides the details of data storage, allowing the domain model to remain pure and independent of infrastructure concerns. In the retail domain, repositories manage the persistence of products, transactions, customers, inventory positions, prices, and orders across various storage technologies.

Repositories operate on aggregate roots only. To access an entity or value object within an aggregate, the consumer retrieves the aggregate root through the repository and navigates to the desired internal object. This enforces the aggregate's consistency boundaries and prevents bypassing business rules.

## Retail Repositories by Bounded Context

### Product Catalog Context

**ProductRepository**: Stores and retrieves Product aggregates by product identifier or GTIN. Supports queries for products by category, brand, attribute values, and active status. The repository must handle large catalogs (tens of thousands to millions of products) efficiently, supporting pagination and filtered retrieval for assortment management.

**CategoryRepository**: Stores and retrieves Category aggregates by category code. Supports queries for the full category hierarchy or subtrees. The repository must support efficient tree traversal for navigation and product classification.

### Pricing and Promotions Context

**PriceListRepository**: Stores and retrieves Price List aggregates by price zone and effective date. Supports queries for the current active price of a product in a specific zone. Performance is critical because price lookups occur during every transaction at every register.

**PromotionRepository**: Stores and retrieves Promotion aggregates by promotion code or date range. Supports queries for all active promotions applicable to a given product, category, or customer segment at a point in time. Must support efficient evaluation of promotion eligibility during checkout.

### Point of Sale Context

**TransactionRepository**: Stores and retrieves Transaction aggregates by transaction number (store, register, sequence). Supports queries by date range, store, register, cashier, and customer. The repository must handle high write throughput during peak selling hours and support both operational queries (current-day transactions) and analytical queries (historical reporting).

### Customer and Loyalty Context

**CustomerRepository**: Stores and retrieves Customer aggregates by customer identifier, email address, phone number, or loyalty card number. Must support deduplication queries to identify potential duplicate customer records across channels. Privacy requirements (GDPR, CCPA) mean the repository must support data access requests, portability exports, and right-to-erasure operations.

**LoyaltyAccountRepository**: Stores and retrieves Loyalty Account aggregates by account number or associated customer identifier. Supports queries for accounts approaching tier thresholds, accounts with expiring points, and accounts with recent activity.

### Inventory and Merchandising Context

**StockPositionRepository**: Stores and retrieves Stock Position aggregates by SKU and location. Supports queries for all locations carrying a given SKU, all SKUs at a given location, and SKUs below reorder point. The repository must handle high-frequency updates as sales and receipts continuously modify stock quantities.

**PurchaseOrderRepository**: Stores and retrieves Purchase Order aggregates by order number or supplier. Supports queries for open orders, overdue orders, and orders awaiting receiving.

### Omnichannel Context

**OrderRepository**: Stores and retrieves Order aggregates by order number or customer. Supports queries by status (placed, picked, shipped, delivered, cancelled), channel, and fulfillment location.

## Repository Design in Retail

Retail repositories should define interfaces in the domain layer and provide implementations in the infrastructure layer. This allows the domain model to declare its persistence needs without coupling to specific databases or ORMs.

Query capabilities vary by context. The Product Catalog Context needs rich query support for catalog browsing and search. The POS Context needs fast writes and sequential reads. The Inventory Context needs real-time consistency for stock quantities. Repository interfaces should reflect these differing needs rather than providing a one-size-fits-all abstraction.

CQRS (Command Query Responsibility Segregation) is particularly valuable in retail. The write side uses repositories to store aggregate state changes, while the read side uses optimized query models for catalog search, reporting, and dashboards. This separation allows each side to be scaled and optimized independently.

## Relationships to Other Topics

- [Aggregates](../aggregates/index.md): Repositories persist and retrieve aggregate roots.
- [Entities](../entities/index.md): Entities are accessed through their aggregate's repository.
- [Domain Events](../domain-events/index.md): Event-sourced repositories store events rather than current state.
- [Domain Services](../domain-services/index.md): Domain services use repositories to access multiple aggregates.
- [Bounded Contexts](../bounded-contexts/index.md): Each bounded context defines its own repositories.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 6: The Life Cycle of a Domain Object.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 12: Repositories.
- Fowler, Martin. *Patterns of Enterprise Application Architecture*. Addison-Wesley, 2002. Repository pattern.
