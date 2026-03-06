# Subdomains in Finance

## Overview

Subdomains classify areas of the domain by their strategic importance to the organization. Domain-Driven Design identifies three types: core, supporting, and generic. This classification determines where to invest the most effort, talent, and custom engineering versus where to use commodity solutions.

## Relevance to Finance

Financial institutions must allocate engineering resources across dozens of capabilities. Not all capabilities are equally differentiating. A bank's competitive advantage may come from its risk models or trading algorithms, not from its email system or document storage. Subdomain classification drives these investment decisions.

## Subdomain Types

### Core Subdomains

Core subdomains are the capabilities that differentiate the institution and directly impact competitive advantage. They deserve the best talent, custom engineering, and continuous refinement.

In finance, core subdomains include:

- **Credit risk modeling** -- Proprietary algorithms for assessing borrower creditworthiness and pricing loans
- **Trading and execution** -- Algorithms for optimal order routing, execution strategy, and market making
- **Portfolio optimization** -- Models for asset allocation, risk-return optimization, and rebalancing
- **Fraud detection** -- Machine learning models for identifying suspicious patterns in real-time transaction streams
- **Regulatory capital calculation** -- Internal models for computing capital adequacy under Basel III/IV

### Supporting Subdomains

Supporting subdomains are necessary for the business to operate but are not the primary source of competitive advantage. They may use established patterns and frameworks.

In finance, supporting subdomains include:

- **Customer onboarding** -- KYC verification, account opening, product enrollment
- **Payment processing** -- Routing payments through established networks (SWIFT, ACH, Fedwire)
- **Reconciliation** -- Matching internal records with external statements and counterparty confirmations
- **Reporting and analytics** -- Financial statements, management reporting, business intelligence
- **Loan servicing** -- Payment processing, statement generation, escrow management
- **General ledger** -- Double-entry bookkeeping, journal posting, trial balance

### Generic Subdomains

Generic subdomains are not specific to finance and can be addressed with off-the-shelf solutions.

In finance, generic subdomains include:

- **Identity and access management** -- User authentication, authorization, single sign-on
- **Email and notifications** -- Transactional emails, push notifications, SMS alerts
- **Document management** -- Storage, retrieval, and versioning of documents
- **Audit logging** -- System activity recording for compliance and debugging
- **Scheduling and job management** -- Batch processing, cron jobs, task queues

## Classification Decisions

The classification of a subdomain depends on the institution's strategy:

- A fintech specializing in payments may treat payment processing as a core domain, while a traditional bank may treat it as supporting
- A quantitative hedge fund treats trading algorithms as core, while a retail bank may treat basic securities trading as supporting
- A compliance-focused institution may elevate regulatory reporting to core status

## Relationships to Other Topics

- [Strategic Design](../strategic-design/) -- Subdomain identification is a core strategic design activity
- [Bounded Contexts](../bounded-contexts/) -- Subdomains often align with bounded context boundaries
- [Context Map](../context-map/) -- Subdomain classification influences integration strategy
- [Domain Services](../domain-services/) -- Core subdomain logic often lives in domain services

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 15: Distillation. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 2: Domains, Subdomains, and Bounded Contexts. Addison-Wesley, 2013.
- Vernon, Vaughn. "Domain-Driven Design Distilled." Chapter 2: Strategic Design with Bounded Contexts and the Ubiquitous Language. Addison-Wesley, 2016.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 1: Analyzing Business Domains. O'Reilly, 2021.
