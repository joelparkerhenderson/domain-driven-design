# Bounded Contexts in Finance

## Overview

A bounded context is a well-defined boundary within which a particular domain model applies and terms have specific, unambiguous meaning. Within a bounded context, the ubiquitous language is consistent: every term has exactly one definition, and the model reflects the mental model of the domain experts working within that boundary.

## Relevance to Finance

Finance is a domain where the same word carries different meanings depending on context. "Account" in the general ledger means a category in the chart of accounts; in customer onboarding, it means a client relationship; in trading, it may refer to a brokerage account. Bounded contexts resolve these ambiguities by giving each term a precise definition within its boundary.

## The Six Bounded Contexts

### Accounts & Ledger Context

Manages the financial record-keeping backbone. This context owns the chart of accounts, general ledger, journal entries, and trial balance. Every financial transaction across the institution ultimately produces journal entries here.

- **Core concepts**: Account, JournalEntry, LineItem, TrialBalance, FiscalPeriod
- **Language specifics**: "Account" means a ledger category (asset, liability, equity, revenue, expense)
- **Team**: Accounting and financial reporting specialists

### Payments & Transfers Context

Handles the movement of money between parties. This context manages payment initiation, routing, clearing, settlement, and reconciliation across multiple payment rails including wire transfers, ACH, and real-time payment networks.

- **Core concepts**: Payment, Transfer, PaymentOrder, ClearingInstruction, SettlementBatch
- **Language specifics**: "Settlement" means the final irrevocable transfer of funds
- **Team**: Payment operations and treasury specialists

### Lending & Credit Context

Manages the full lifecycle of loans from origination through servicing to payoff or collection. This context handles credit applications, underwriting decisions, disbursement, repayment scheduling, and delinquency management.

- **Core concepts**: LoanApplication, Loan, AmortizationSchedule, Underwriting, CollectionCase
- **Language specifics**: "Account" means a specific loan account with a repayment schedule
- **Team**: Loan officers, credit analysts, and collections specialists

### Risk & Compliance Context

Evaluates and monitors financial risks and regulatory obligations. This context manages credit risk, market risk, operational risk, KYC/AML screening, fraud detection, and regulatory reporting.

- **Core concepts**: RiskAssessment, KYCProfile, FraudAlert, RegulatoryReport, CapitalRequirement
- **Language specifics**: "Exposure" means the total amount at risk from a counterparty or position
- **Team**: Risk analysts, compliance officers, and regulatory specialists

### Trading & Investments Context

Manages the buying and selling of financial instruments. This context handles order management, trade execution, portfolio management, market data consumption, and trade settlement.

- **Core concepts**: Order, Trade, Position, Portfolio, MarketQuote, SecurityMaster
- **Language specifics**: "Settlement" means the exchange of securities and cash on the settlement date (T+1 or T+2)
- **Team**: Traders, portfolio managers, and operations specialists

### Customer Onboarding Context

Manages the process of establishing new customer relationships. This context handles identity verification, KYC data collection, account opening, product enrollment, and document management.

- **Core concepts**: Application, CustomerProfile, IdentityVerification, ProductEnrollment, Document
- **Language specifics**: "Account" means the customer's overall relationship and product enrollment
- **Team**: Client services, compliance, and operations specialists

## Boundary Design Principles

1. **Linguistic boundaries**: When the same term means different things to different teams, those teams likely belong in different bounded contexts
2. **Organizational alignment**: Bounded contexts should align with team structures to minimize cross-team coordination
3. **Autonomy**: Each context should be independently deployable and evolvable
4. **Consistency boundaries**: Transactional consistency is maintained within a bounded context; eventual consistency applies across contexts

## Relationships to Other Topics

- [Strategic Design](../strategic-design/) -- Bounded contexts are the primary output of strategic design
- [Context Map](../context-map/) -- Documents the relationships between bounded contexts
- [Ubiquitous Language](../ubiquitous-language/) -- Each bounded context has its own consistent language
- [Anti-Corruption Layer](../anti-corruption-layer/) -- Protects bounded contexts from external model contamination
- [Aggregates](../aggregates/) -- Define the consistency boundaries within a bounded context

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 14: Maintaining Model Integrity. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 2: Domains, Subdomains, and Bounded Contexts. Addison-Wesley, 2013.
- Vernon, Vaughn. "Domain-Driven Design Distilled." Chapter 4: Strategic Design with Context Mapping. Addison-Wesley, 2016.
- Millett, Scott & Tune, Nick. "Patterns, Principles, and Practices of Domain-Driven Design." Chapter 6: Maintaining the Integrity of Domain Models with Bounded Contexts. Wrox, 2015.
