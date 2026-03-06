# Pricing and Promotions Context

## Overview

The Pricing and Promotions Context is the bounded context responsible for price management, markdown scheduling, promotional campaigns, coupon handling, and price calculation rules. This context determines the price a customer pays for any product at any point in time, considering base prices, markdowns, promotions, coupons, and customer-specific pricing. For many retailers, pricing strategy is a core subdomain that provides competitive differentiation.

## Core Concepts

**Price List**: A collection of price entries for products within a defined scope, typically a price zone (geographic region, store cluster, or channel). Each product in a price list has a regular price and optionally a sale price with an effective date range. Price lists support multi-currency pricing for international retailers.

**Price Zone**: A grouping of stores or channels that share the same pricing. A retailer might define price zones by region (Northeast, Southeast, West), by store format (full-line store, outlet, pop-up), or by channel (in-store, online, marketplace). Zone-based pricing allows competitive positioning while managing the complexity of individual store pricing.

**Markdown**: A permanent or semi-permanent reduction in a product's price, typically driven by end-of-season clearance, slow-selling merchandise, or fashion cycle transitions. Markdowns follow a schedule: initial markdown, second markdown, final clearance. The markdown strategy is often a core business capability involving optimization algorithms that balance sell-through rate, margin preservation, and inventory clearance goals.

**Promotion**: A temporary offer that provides a benefit to the customer. Promotions include percentage-off discounts, dollar-off discounts, buy-one-get-one (BOGO) offers, buy-N-for-$X offers, gift-with-purchase, and basket-level discounts (spend $100, save $20). Promotions have eligibility rules (which products, which customers, which channels, which date range) and stacking rules (which promotions can combine with others).

**Coupon**: A mechanism that entitles the bearer to a specific discount. Coupons may be physical (printed barcodes), digital (codes entered online), or automatically applied (targeted offers). Coupons reference promotion rules and may have usage limits (one per customer, total redemption cap).

**Price Override**: A manual adjustment to a product's calculated price, typically performed by a manager at the point of sale. Price overrides are governed by authorization rules and audited for loss prevention.

## Aggregates

The **Price List Aggregate** is rooted at the Price List entity and contains PriceEntry value objects. Invariants include: one active regular price per product per zone at any time, markdown prices must be less than or equal to regular prices, and effective date ranges must not overlap for the same product.

The **Promotion Aggregate** is rooted at the Promotion entity and contains eligibility rules, discount mechanics, stacking rules, and budget limits. Invariants include: valid date ranges, discount bounds (maximum percentage, maximum dollar amount), and compatible stacking configurations.

## Domain Events

- PriceChanged: Published when a product's regular or sale price changes in a zone.
- MarkdownApplied: Published when a markdown reduces a product's price.
- PromotionActivated: Published when a promotion becomes effective.
- PromotionExpired: Published when a promotion's effective period ends.
- CouponRedeemed: Published when a coupon is successfully applied to a transaction.

## Domain Services

**PriceCalculationService**: The central service that determines the final price for a product given the price zone, applicable promotions, customer segment, and transaction context. This service resolves price conflicts (multiple promotions, best-price guarantees) and applies stacking rules.

**PromotionEvaluationService**: Evaluates a basket of items against all active promotions to determine which promotions apply and calculates the resulting discounts.

**MarkdownOptimizationService**: Determines the timing and depth of markdowns based on inventory levels, sell-through rates, and margin targets.

## Integration with Other Contexts

- **Product Catalog Context** (upstream): Provides product identifiers and categories that prices and promotions reference.
- **Point of Sale Context** (downstream): Requests price calculations during transaction processing.
- **Customer and Loyalty Context**: Provides customer segment and tier data for personalized pricing.
- **Omnichannel Context**: Requests prices for digital product display and cart calculation.
- **Inventory and Merchandising Context**: Provides inventory data for markdown optimization decisions.

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/index.md): Pricing and Promotions is a core retail bounded context.
- [Aggregates](../aggregates/index.md): Price List and Promotion are the primary aggregates.
- [Domain Services](../domain-services/index.md): Price calculation and promotion evaluation services.
- [Value Objects](../value-objects/index.md): Money, PriceEntry, DiscountAmount, and DateRange.
- [Domain Events](../domain-events/index.md): PriceChanged, PromotionActivated, and related events.
- [Point of Sale Context](../point-of-sale-context/index.md): Primary consumer of pricing services.
- [Subdomains](../subdomains/index.md): Pricing may be a core subdomain for price-competitive retailers.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Phillips, Robert L. *Pricing and Revenue Optimization*. Stanford Business Books, 2005.
- ARTS (Association for Retail Technology Standards). *ARTS Data Model: Pricing and Promotion*. NRF, ongoing.
