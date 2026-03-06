# Subdomains

## Overview

Subdomains classify areas of a domain by their strategic importance to the business. Domain-Driven Design identifies three types: core subdomains that provide competitive advantage, supporting subdomains that are necessary but not differentiating, and generic subdomains that are common across industries and can be sourced externally. In the retail domain, subdomain classification guides investment decisions, build-versus-buy choices, and talent allocation.

## Retail Subdomain Classification

### Core Subdomains

Core subdomains are the capabilities that differentiate a retailer from its competitors. These deserve the strongest engineering talent and the most careful domain modeling. The specific core subdomains vary by retailer type.

**Assortment and Merchandising Strategy**: For fashion and specialty retailers, the ability to curate assortments, predict trends, and optimize product mix is a core differentiator. The algorithms and rules for assortment planning are proprietary and competitively valuable.

**Pricing Optimization**: For discount and value retailers, sophisticated markdown optimization, competitive price matching, and dynamic pricing are core capabilities. The pricing engine encodes the retailer's unique strategy for maximizing margin while maintaining price perception.

**Customer Experience and Loyalty**: For customer-centric retailers, the loyalty program design, personalization engine, and customer journey orchestration are core. The rules for tier progression, point earn and burn, and personalized offers represent strategic intellectual property.

**Omnichannel Fulfillment**: For retailers competing on convenience, the ability to orchestrate BOPIS, ship-from-store, same-day delivery, and endless aisle is a core differentiator. The fulfillment optimization logic that decides which location fulfills which order is competitively significant.

### Supporting Subdomains

Supporting subdomains are necessary for the business to operate but do not provide competitive differentiation on their own. They are typically built in-house with adequate but not exceptional investment.

**Inventory Management**: Tracking stock levels, managing replenishment, and handling receiving and transfers. While essential, basic inventory management follows well-understood patterns.

**Point of Sale Operations**: Processing transactions, handling tenders, and managing register workflows. POS operations follow industry-standard patterns, though they must integrate tightly with core subdomains.

**Product Catalog Management**: Maintaining product data, categories, and attributes. Catalog management is essential infrastructure but the data model follows common retail patterns.

### Generic Subdomains

Generic subdomains solve problems common across industries and are best addressed with off-the-shelf solutions, third-party services, or open-source components.

**Payment Processing**: Credit card processing, tokenization, and settlement follow PCI DSS standards and are best handled by specialized payment service providers.

**Tax Calculation**: Sales tax, VAT, and multi-jurisdiction tax rules are complex but generic. Tax engines from providers like Avalara or Vertex handle this domain.

**Address Validation and Geocoding**: Verifying and standardizing addresses is a generic capability handled by specialized services.

**Identity and Authentication**: User authentication, authorization, and session management follow generic security patterns.

## Relationships to Other Topics

- [Strategic Design](../strategic-design/index.md): Subdomain classification is a key strategic design activity.
- [Bounded Contexts](../bounded-contexts/index.md): Subdomains inform but do not dictate bounded context boundaries.
- [Context Map](../context-map/index.md): Subdomain classification influences integration patterns on the context map.
- [Anti-Corruption Layer](../anti-corruption-layer/index.md): Generic subdomains often connect through anti-corruption layers.
- [Domain Services](../domain-services/index.md): Core subdomain logic is often expressed as domain services.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 15: Distillation.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 2: Domains, Subdomains, and Bounded Contexts.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly, 2021. Chapter 1: Analyzing Business Domains.
