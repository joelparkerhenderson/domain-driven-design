# Strategic Design in Finance

## Overview

Strategic design in Domain-Driven Design (DDD) addresses how to decompose a large, complex financial system into manageable parts that align with the organizational structure and business workflows. It focuses on identifying the most important areas of the domain, defining clear boundaries between subsystems, and establishing how those subsystems communicate.

## Relevance to Finance

Financial systems are among the most complex enterprise domains, involving accounting, payments, lending, trading, risk management, regulatory compliance, and customer management. Strategic design provides the tools to manage this complexity by:

- Aligning software models with how traders, accountants, loan officers, and compliance analysts actually think about their work
- Separating concerns so that changes in payments processing do not ripple into the general ledger or trading systems
- Identifying which parts of the system deserve the most investment and the best engineering talent
- Enabling independent teams to work on different bounded contexts without constant coordination

## Key Concepts

### Knowledge Crunching

Knowledge crunching is the collaborative process of extracting domain knowledge from financial experts and translating it into a software model. In finance, this means:

- Conducting workshops with accountants, traders, risk analysts, loan officers, and compliance professionals
- Observing financial workflows such as trade execution, loan origination, and month-end close
- Iteratively refining the model as understanding deepens
- Resolving ambiguities in terminology (e.g., "account" means different things in the general ledger vs. customer onboarding vs. trading contexts)

### Domain Vision Statement

A domain vision statement captures the core purpose and value of the system. For a finance system, this might be:

> "To provide a unified platform that supports accurate, compliant, and efficient financial operations by modeling accounting, payments, lending, trading, and risk management as first-class domain concepts, enabling financial professionals to focus on business outcomes rather than system complexity."

### Core Domain Identification

Not all parts of the financial system are equally important. Strategic design distinguishes:

- **Core Domain**: Risk modeling, trading algorithms, credit underwriting, and regulatory reporting -- the areas that differentiate the institution and directly impact revenue and compliance
- **Supporting Subdomains**: Customer onboarding, reconciliation, reporting, notifications -- necessary but not the primary differentiator
- **Generic Subdomains**: Authentication, email delivery, document storage, logging -- commoditized capabilities available as off-the-shelf solutions

### Distillation

Distillation separates the essential complexity of the core domain from the accidental complexity of supporting and generic concerns. In finance:

- Credit risk scoring and portfolio optimization are core and deserve custom modeling
- Payment message formatting is supporting and may use established standards like ISO 20022
- User authentication is generic and should use standard identity providers

## Finance Domain Decomposition

Strategic design guides the finance system into bounded contexts:

- **Accounts & Ledger Context** -- Chart of accounts, general ledger, journal entries, trial balance, financial statements
- **Payments & Transfers Context** -- Payment initiation, wire transfers, ACH processing, real-time payments, clearing
- **Lending & Credit Context** -- Loan origination, underwriting, servicing, collections, amortization
- **Risk & Compliance Context** -- Risk assessment, KYC/AML, fraud detection, regulatory reporting, capital adequacy
- **Trading & Investments Context** -- Order management, execution, portfolio management, market data, settlement
- **Customer Onboarding Context** -- Account opening, identity verification, KYC, product enrollment, document management

Each context has its own model, language, and team ownership, reducing cognitive load and enabling independent evolution.

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) -- The primary output of strategic design
- [Context Map](../context-map/) -- Visualizes relationships between bounded contexts
- [Subdomains](../subdomains/) -- Classification of domain areas by strategic importance
- [Ubiquitous Language](../ubiquitous-language/) -- Shared vocabulary within each bounded context
- [Anti-Corruption Layer](../anti-corruption-layer/) -- Protects context boundaries from external system leakage

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapters 14-16. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Part I: Strategic Design. Addison-Wesley, 2013.
- Vernon, Vaughn. "Domain-Driven Design Distilled." Addison-Wesley, 2016.
- Millett, Scott & Tune, Nick. "Patterns, Principles, and Practices of Domain-Driven Design." Part III. Wrox, 2015.
- Brandolini, Alberto. "Introducing EventStorming." Leanpub, 2021.
