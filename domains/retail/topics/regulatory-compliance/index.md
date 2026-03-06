# Regulatory Compliance

## Overview

Regulatory compliance in retail encompasses the legal, financial, and governance requirements that shape domain model design and operational processes. Retail systems must comply with payment security standards, consumer protection laws, tax regulations, data privacy rules, and accessibility requirements. In a Domain-Driven Design context, regulatory compliance is a cross-cutting concern that influences aggregate design, value object validation, domain event content, and anti-corruption layer implementation across all bounded contexts.

Compliance requirements do not exist as a separate bounded context. Instead, they are woven into the domain models of the contexts they affect. The Point of Sale Context must comply with PCI DSS. The Customer and Loyalty Context must comply with GDPR and CCPA. The Pricing Context must comply with consumer protection pricing laws. The Inventory Context must comply with product safety regulations.

## PCI DSS for Payment Processing

The Payment Card Industry Data Security Standard (PCI DSS) governs how organizations handle, process, store, and transmit credit card data. PCI DSS compliance directly shapes the Point of Sale Context:

**Domain Model Impact**: The Transaction aggregate must never contain raw primary account numbers (PAN), CVV/CVC codes, or PIN data. Payment card data is handled exclusively by PCI-validated point-to-point encryption (P2PE) devices and payment gateways. The domain model works with payment tokens (opaque references to card data held securely by the payment processor) and authorization codes.

**Scope Reduction**: By using P2PE hardware and tokenization, the retail domain model removes itself from PCI scope as much as possible. The anti-corruption layer between the POS Context and payment processors is designed to minimize the system's PCI cardholder data environment.

**Audit Requirements**: PCI DSS requires logging of access to cardholder data, regular vulnerability assessments, and annual compliance validation. Domain events related to payment processing must be retained for audit purposes while excluding sensitive card data.

**PCI DSS v4.0**: The current version (effective 2024) introduces enhanced requirements for multi-factor authentication, encryption in transit and at rest, and targeted risk analysis. Retail systems must adapt their security architecture to meet these evolving requirements.

## Consumer Protection Laws

Consumer protection regulations affect the Pricing and Promotions Context and the Point of Sale Context:

**Price Accuracy**: Laws in many jurisdictions require that the price charged matches the price displayed. When a shelf tag shows one price and the system charges another, the customer is typically entitled to the lower price. The Pricing Context must ensure synchronization between displayed prices and system prices. Domain events like PriceChanged must trigger updates to all price display channels.

**Unit Pricing**: Many jurisdictions require retailers to display unit prices (price per ounce, price per count) alongside the total price for packaged goods. The value objects in the Pricing Context must support unit price calculation and display.

**Rain Checks**: When an advertised sale item is out of stock, some jurisdictions require the retailer to issue a rain check allowing the customer to purchase the item at the sale price when it becomes available. This creates an intersection between the Pricing Context and the Inventory Context.

**Return Policies**: Consumer protection laws in many jurisdictions mandate minimum return periods or require that return policies be clearly disclosed. The POS Context's return processing logic must enforce legal minimums even if the retailer's stated policy is more restrictive.

**Advertising Standards**: Promotional claims (comparative pricing, percentage-off claims, "free" offers) are regulated by the Federal Trade Commission (FTC) in the United States and equivalent bodies in other jurisdictions. The Promotion aggregate must validate that promotional claims comply with advertising standards.

## Tax Compliance

Tax compliance is a significant concern for the Point of Sale Context and the Omnichannel Context:

**Sales Tax (United States)**: The U.S. has over 12,000 tax jurisdictions with different rates and rules. Sales tax calculation depends on the selling location, the delivery location (for shipped orders), the product category (food, clothing, and medicine may be exempt or taxed at reduced rates), and the customer (tax-exempt organizations). The Supreme Court decision in South Dakota v. Wayfair (2018) established that states can require online retailers to collect sales tax even without physical presence, significantly expanding tax obligations for omnichannel retailers.

**Value Added Tax (International)**: VAT systems require tax to be calculated at each stage of the supply chain. VAT rates vary by country and product category. The domain model must support VAT-inclusive pricing (common in Europe) and VAT-exclusive pricing (common in B2B transactions).

**Tax Exemptions**: Certain customers (non-profit organizations, government agencies, resellers) may be exempt from sales tax. The POS Context and Omnichannel Context must support tax exemption certificates and validation.

**Tax Reporting**: Retailers must file regular tax returns with jurisdiction-specific requirements. Domain events from the POS Context (TransactionCompleted, ReturnProcessed) feed into tax reporting systems. The data retained in these events must include sufficient detail for accurate tax filing.

**Tax Calculation Services**: Given the complexity of multi-jurisdiction tax, most retailers delegate tax calculation to specialized services (Avalara, Vertex, Thomson Reuters ONESOURCE) through an anti-corruption layer. The domain model owns the concept of what is taxable and the resulting tax amount, while the external service determines the applicable rate.

## GDPR and CCPA for Customer Data

Data privacy regulations affect the Customer and Loyalty Context and any context that processes personally identifiable information (PII):

**GDPR (General Data Protection Regulation)**: The European Union regulation that governs the processing of personal data for EU residents. Key requirements that shape the domain model include:

- **Consent Management**: The Customer aggregate must record explicit consent for each data processing purpose (marketing, analytics, personalization). Consent is a value object with a purpose, a granted/denied status, and a timestamp.
- **Right to Access**: The system must be able to provide a customer with all data held about them. The CustomerRepository must support comprehensive data extraction.
- **Right to Erasure**: The system must be able to delete a customer's personal data upon request, while retaining anonymized transaction data needed for financial reporting. Aggregate design must support selective data deletion.
- **Right to Portability**: Customer data must be exportable in a machine-readable format.
- **Data Minimization**: The domain model should only capture and retain personal data that is necessary for a specific, documented purpose.

**CCPA (California Consumer Privacy Act)**: The California regulation with requirements similar to GDPR, including the right to know what data is collected, the right to delete personal data, the right to opt out of data sale, and requirements for privacy policy disclosures.

**Domain Design Implications**: Privacy requirements are not bolted on after the fact; they are designed into the aggregate structure. The Customer aggregate includes consent records as first-class value objects. Repositories support data access and deletion requests. Domain events that carry PII must be handled according to data retention policies.

## ADA Accessibility

The Americans with Disabilities Act (ADA) and equivalent international regulations (European Accessibility Act, EN 301 549) affect the Omnichannel Context, particularly digital channels:

**Digital Accessibility**: E-commerce websites and mobile applications must be accessible to people with disabilities, following Web Content Accessibility Guidelines (WCAG). While this primarily affects the presentation layer rather than the domain model, the domain must support accessibility by providing complete textual descriptions for products (alt text for images), structured content that can be rendered accessibly, and alternative interaction patterns.

**In-Store Accessibility**: Physical store accessibility affects store layout, signage, and POS terminal design. Planogram definitions in the Inventory and Merchandising Context should consider accessibility requirements for product placement.

## Relationships to Other Topics

- [Point of Sale Context](../point-of-sale-context/index.md): PCI DSS, sales tax, and consumer protection compliance.
- [Customer Loyalty Context](../customer-loyalty-context/index.md): GDPR, CCPA, and data privacy compliance.
- [Pricing Promotions Context](../pricing-promotions-context/index.md): Consumer protection pricing laws.
- [Omnichannel Context](../omnichannel-context/index.md): ADA accessibility and multi-jurisdiction tax for online sales.
- [Anti-Corruption Layer](../anti-corruption-layer/index.md): Isolates compliance-specific external services.
- [Retail Standards](../retail-standards/index.md): PCI DSS bridges standards and compliance.
- [Value Objects](../value-objects/index.md): Consent records, tax amounts, and tokens as value objects.
- [Domain Events](../domain-events/index.md): Events must comply with data retention and privacy policies.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- PCI Security Standards Council. *PCI Data Security Standard (PCI DSS) v4.0*. 2022.
- European Parliament. *General Data Protection Regulation (GDPR)*. Regulation (EU) 2016/679, 2016.
- State of California. *California Consumer Privacy Act (CCPA)*. California Civil Code 1798.100-199, 2018.
- U.S. Supreme Court. *South Dakota v. Wayfair, Inc.* 585 U.S. 162, 2018.
- Federal Trade Commission. *Guides Against Deceptive Pricing*. 16 CFR Part 233.
- W3C. *Web Content Accessibility Guidelines (WCAG) 2.1*. 2018.
