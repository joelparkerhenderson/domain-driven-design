# Customer and Loyalty Context

## Overview

The Customer and Loyalty Context is the bounded context responsible for customer profiles, loyalty program management, point accrual and redemption, tier progression, and customer segmentation. This context serves as the system of record for who the retailer's customers are, what their preferences are, and how their loyalty is recognized and rewarded. For customer-centric retailers, this context encompasses core subdomain capabilities that drive competitive differentiation through personalized experiences and retention programs.

## Core Concepts

**Customer Profile**: The central entity representing a customer across all channels. A customer profile includes demographic information (name, addresses, contact details), communication preferences (email opt-in, SMS opt-in, push notifications), and channel identifiers (online account, loyalty card number, mobile app ID). Customer profiles support multi-channel identification, allowing the same customer to be recognized whether shopping in-store, online, or through a mobile app.

**Loyalty Program**: The set of rules that define how customers earn and redeem rewards. A loyalty program includes earn rules (points per dollar spent, bonus point promotions, category multipliers), redemption rules (minimum point thresholds, point-to-dollar conversion rates), tier definitions, and program terms and conditions. A retailer may operate multiple loyalty programs or program tiers simultaneously.

**Loyalty Account**: The entity that tracks an individual member's standing in the loyalty program. The loyalty account records the current point balance, point transaction history (earn and burn), current tier, qualifying spend or points for tier evaluation, and account status (active, suspended, closed). Points may have expiration dates and may be categorized by type (base points, bonus points, promotional points).

**Tier Structure**: The hierarchical levels within a loyalty program (e.g., Bronze, Silver, Gold, Platinum). Each tier provides specific benefits such as earn rate multipliers, exclusive promotions, free shipping, early access to sales, or dedicated customer service. Tier qualification is based on spending thresholds or point accumulation over an evaluation period. Tier status is evaluated periodically (monthly, quarterly, annually) and may include both upgrade and downgrade paths.

**Customer Segment**: A grouping of customers based on shared characteristics used for targeted marketing, personalized pricing, and tailored experiences. Segments may be defined by behavior (high spenders, frequent shoppers, lapsed customers), demographics (age, location), preferences (categories, brands), or lifecycle stage (new, loyal, at-risk, churned).

## Aggregates

The **Customer Aggregate** is rooted at the Customer entity and contains addresses, contact details, communication preferences, and channel identifiers. Invariants include: unique customer identifier, validated email addresses and phone numbers, and properly recorded consent for data processing and communications.

The **Loyalty Account Aggregate** is rooted at the Loyalty Account entity and contains the point ledger, tier status, and qualification tracking. Invariants include: point balance must never go negative, tier transitions must follow defined rules, and point transactions must be traceable to source transactions.

## Domain Events

- CustomerRegistered: A new customer has enrolled in the system.
- CustomerProfileUpdated: Customer attributes have been modified.
- LoyaltyPointsEarned: Points have been accrued from a qualifying purchase.
- LoyaltyPointsRedeemed: Points have been spent as payment toward a purchase.
- LoyaltyPointsExpired: Points have expired based on program rules.
- TierChanged: A member has moved to a different loyalty tier (upgrade or downgrade).
- CustomerPreferenceUpdated: Communication or privacy preferences have changed.

## Domain Services

**PointAccrualService**: Calculates points earned for a transaction based on spend amount, product categories, tier multipliers, and active bonus point promotions.

**TierEvaluationService**: Evaluates whether a member qualifies for tier upgrade or is subject to tier downgrade based on qualifying activity over the evaluation period.

**CustomerDeduplicationService**: Identifies and merges duplicate customer records that may arise from multi-channel enrollment.

## Integration with Other Contexts

- **Point of Sale Context**: POS identifies customers during checkout and reports completed transactions for point accrual. Loyalty points may be redeemed as a tender type.
- **Pricing and Promotions Context**: Customer tier and segment data enables personalized pricing and targeted promotions.
- **Omnichannel Context**: Customer profile data enables personalized experiences across channels. Order history feeds customer segmentation.
- **Product Catalog Context**: Product category data informs category-specific earn rates and customer preference analysis.

## Privacy and Data Protection

The Customer and Loyalty Context handles personally identifiable information (PII) and must comply with privacy regulations including GDPR, CCPA, and other jurisdictional data protection laws. The domain model must support data subject access requests (right to know), data portability (right to export), data deletion (right to erasure), consent management, and purpose limitation. These regulatory requirements shape the aggregate design, particularly around how consent is recorded and how data retention and deletion are managed.

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/index.md): Customer and Loyalty is a core retail bounded context.
- [Aggregates](../aggregates/index.md): Customer and Loyalty Account are the primary aggregates.
- [Entities](../entities/index.md): Customer and Loyalty Account are key entities.
- [Value Objects](../value-objects/index.md): EmailAddress, PhoneNumber, LoyaltyPoints, and TierLevel.
- [Domain Events](../domain-events/index.md): CustomerRegistered, PointsEarned, TierChanged events.
- [Regulatory Compliance](../regulatory-compliance/index.md): GDPR, CCPA, and data protection requirements.
- [Point of Sale Context](../point-of-sale-context/index.md): Loyalty integration during checkout.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Program, Loyalty. *The Loyalty Guide*. Wise Marketer, ongoing.
- European Parliament. *General Data Protection Regulation (GDPR)*. Regulation (EU) 2016/679, 2016.
- State of California. *California Consumer Privacy Act (CCPA)*. California Civil Code 1798.100-199, 2018.
