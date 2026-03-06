# Domain Services in Finance

## Overview

A domain service is a stateless operation that belongs to the domain but does not naturally fit within a single entity or value object. Domain services encapsulate business logic that spans multiple aggregates, involves coordination between entities, or represents a domain concept that is inherently an action rather than a thing.

## Relevance to Finance

Financial operations frequently span multiple aggregates and contexts. Transferring funds between accounts, executing a trade with pre-trade risk checks, calculating portfolio valuations, and running end-of-day settlement processes are all examples of operations that do not belong to any single entity but are core domain logic.

## Key Concepts

### When to Use Domain Services

Use a domain service when the operation:

- Involves multiple aggregates and cannot be owned by a single aggregate root
- Represents a significant domain process that domain experts discuss as a standalone concept
- Is stateless -- it does not hold any mutable state between invocations
- Contains business logic that would be awkward to place on an entity or value object

### When Not to Use Domain Services

Do not use domain services as a dumping ground for all business logic. If an operation naturally belongs to an entity or value object, keep it there. Overuse of domain services leads to an anemic domain model where entities are mere data containers.

## Finance Domain Services

### FundsTransferService (Payments & Transfers Context)

Coordinates the movement of funds between accounts, spanning the Payments and Accounts & Ledger contexts.

- **Operations**: Transfer funds between accounts, validate sufficient balance, apply transfer limits, route through appropriate payment network
- **Inputs**: Source account, destination account, amount, payment method
- **Coordination**: Loads source and destination account aggregates, validates balances and limits, creates payment and journal entry aggregates
- **Events produced**: PaymentInitiated, JournalEntryPosted

### CurrencyConversionService

Converts monetary amounts between currencies using current exchange rates.

- **Operations**: Convert Money from one currency to another, applying the appropriate exchange rate and spread
- **Inputs**: Source Money, target currency, rate source (e.g., ECB, Reuters)
- **Domain logic**: Applies rounding rules per currency (e.g., JPY has zero decimal places), handles cross-rate calculations
- **Design note**: The conversion rate itself is a value object; the service encapsulates the conversion process

### CreditDecisionService (Lending & Credit Context)

Evaluates a loan application and produces an underwriting decision by combining data from multiple aggregates.

- **Operations**: Evaluate creditworthiness, calculate debt-to-income ratio, apply credit policy rules, produce approval or denial
- **Inputs**: LoanApplication, CreditScore, income verification, existing obligations
- **Coordination**: Loads the loan application, retrieves credit reports, applies policy rules, produces an underwriting decision
- **Events produced**: UnderwritingCompleted

### PortfolioValuationService (Trading & Investments Context)

Calculates the current market value of a portfolio by pricing all positions against current market data.

- **Operations**: Value individual positions, compute portfolio NAV, calculate unrealized P&L
- **Inputs**: Portfolio with positions, current market prices
- **Domain logic**: Applies pricing models, handles illiquid securities, computes accrued interest on bonds
- **Events produced**: PositionUpdated, PortfolioValued

### InterestAccrualService (Lending & Credit Context / Accounts & Ledger Context)

Calculates and posts interest accruals across loan portfolios and deposit accounts.

- **Operations**: Calculate daily interest accrual, apply day count conventions, post accrual journal entries
- **Inputs**: Accounts or loans, interest rates, accrual period, day count convention
- **Domain logic**: Handles compounding, applies different day count conventions (Actual/360, Actual/365, 30/360)
- **Events produced**: InterestAccrued, JournalEntryPosted

### SettlementService (Trading & Investments Context)

Coordinates the settlement of trades by matching obligations and instructing the exchange of securities and cash.

- **Operations**: Match trade obligations, generate settlement instructions, confirm settlement
- **Inputs**: Executed trades, settlement date, counterparty settlement instructions
- **Coordination**: Matches buy and sell legs, validates settlement readiness, instructs custodian
- **Events produced**: TradeSettled

### ReconciliationService (Cross-Cutting)

Compares internal records against external statements to identify discrepancies.

- **Operations**: Match internal transactions against external records, identify breaks, categorize discrepancies
- **Inputs**: Internal transaction records, external statement data, matching rules
- **Domain logic**: Applies matching algorithms (exact match, fuzzy match, tolerance-based), categorizes breaks by severity

## Relationships to Other Topics

- [Aggregates](../aggregates/) -- Domain services coordinate operations across multiple aggregates
- [Entities](../entities/) -- Domain services operate on entities but do not replace entity behavior
- [Domain Events](../domain-events/) -- Domain services produce events as outcomes of their operations
- [Repositories](../repositories/) -- Domain services use repositories to load the aggregates they coordinate
- [Event-Driven Architecture](../event-driven-architecture/) -- Domain services may be triggered by domain events

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 5: A Model Expressed in Software. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 7: Services. Addison-Wesley, 2013.
- Millett, Scott & Tune, Nick. "Patterns, Principles, and Practices of Domain-Driven Design." Chapter 22: Domain Model Services. Wrox, 2015.
