# Domain Services

## Overview

A domain service is a stateless operation that encapsulates domain logic which does not naturally belong to a single entity or value object. Domain services express significant business processes that span multiple aggregates or require coordination that goes beyond what any single aggregate should manage. In the retail domain, domain services handle operations like price calculation, promotion evaluation, loyalty point accrual, inventory allocation, and fulfillment routing.

Domain services are part of the domain layer, not the application or infrastructure layers. They express business rules in the ubiquitous language and operate on domain objects. They differ from application services, which orchestrate use cases and coordinate infrastructure concerns.

## Retail Domain Services by Bounded Context

### Product Catalog Context

**AssortmentPlanningService**: Determines which products should be available in which stores or channels based on store attributes (format, region, climate), historical sales data, and merchandising strategy. This service spans multiple Product aggregates and Location references, making it unsuitable for a single aggregate to own.

**ProductMatchingService**: Identifies potential duplicate products or matches products across supplier catalogs using GTIN, description similarity, and attribute matching. This cross-aggregate operation naturally belongs in a domain service.

### Pricing and Promotions Context

**PriceCalculationService**: Calculates the effective price for a product given a price zone, applicable promotions, customer segment, and date. This service consults the Price List aggregate for base prices and the Promotion aggregate for applicable discounts, combining them according to stacking rules and priority order. The price calculation logic is the core of the pricing domain and must be pure business logic without infrastructure dependencies.

**PromotionEvaluationService**: Evaluates which promotions apply to a given basket of items based on eligibility rules (minimum purchase, product category, customer tier) and calculates the discount for each qualifying promotion. Handles complex scenarios like buy-one-get-one, buy-two-get-third-at-half-price, and basket-level discounts.

**MarkdownOptimizationService**: Determines optimal markdown schedules for products based on selling velocity, inventory levels, season progression, and target margins. This service encapsulates the retailer's markdown strategy as a domain service.

### Point of Sale Context

**TaxCalculationService**: Calculates taxes for a transaction based on the selling location's tax jurisdiction, the product categories (some products may be tax-exempt), and applicable exemptions. While the actual tax rate lookup may be delegated to an external service through an anti-corruption layer, the domain service owns the logic of what is taxable and how tax is applied to the transaction.

**TenderValidationService**: Validates that a set of tenders (cash, credit card, gift card, loyalty points) correctly covers the transaction total, enforces tender-specific rules (e.g., maximum cash back, gift card balance check), and determines change due.

### Customer and Loyalty Context

**PointAccrualService**: Calculates the loyalty points earned for a transaction based on the customer's tier, the products purchased, any bonus point promotions, and the program's earn rules. This service spans the Loyalty Account aggregate and references to Transaction data from the POS context.

**TierEvaluationService**: Evaluates whether a loyalty member qualifies for tier advancement or is subject to tier demotion based on qualifying spend or points over the evaluation period. This service enforces the tier rules defined by the loyalty program.

**CustomerDeduplicationService**: Identifies and merges duplicate customer records based on matching rules (email, phone, name and address similarity). This cross-aggregate operation requires a domain service to manage the merge logic and preserve data integrity.

### Inventory and Merchandising Context

**InventoryAllocationService**: Allocates available stock to customer orders, transfers, or holds based on priority rules and allocation strategies. This service manages the contention between multiple demand sources for limited stock.

**ReplenishmentCalculationService**: Calculates reorder quantities based on safety stock levels, demand forecasts, lead times, and pack sizes. The calculation spans multiple Stock Position aggregates and supplier lead time data.

### Omnichannel Context

**FulfillmentRoutingService**: Determines the optimal fulfillment location and method (ship from warehouse, ship from store, pick up in store) for an order based on inventory availability, proximity to the customer, shipping costs, and service-level commitments. This is often a core domain service that encapsulates a retailer's competitive fulfillment strategy.

**ChannelReconciliationService**: Reconciles inventory, pricing, and order data across channels to ensure consistency. Handles conflicts when the same item is sold simultaneously through different channels.

## Relationships to Other Topics

- [Aggregates](../aggregates/index.md): Domain services operate on multiple aggregates.
- [Entities](../entities/index.md): Domain services use entities as inputs and outputs.
- [Value Objects](../value-objects/index.md): Domain services return value objects as results.
- [Repositories](../repositories/index.md): Domain services use repositories to access aggregates.
- [Bounded Contexts](../bounded-contexts/index.md): Domain services are scoped to a single bounded context.
- [Domain Events](../domain-events/index.md): Domain services may trigger domain events.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 5: A Model Expressed in Software.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 7: Services.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly, 2021. Chapter 8: Architectural Patterns.
