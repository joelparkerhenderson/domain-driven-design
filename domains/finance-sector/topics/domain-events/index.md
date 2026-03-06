# Domain Events in Finance

## Overview

A domain event is a record of something significant that happened in the domain. Domain events are named in the past tense using the ubiquitous language (e.g., PaymentSettled, TradeExecuted, LoanDisbursed). They are immutable facts: once something has happened, the event cannot be changed or retracted.

## Relevance to Finance

Financial systems are inherently event-driven. A payment is processed, a trade is executed, a loan defaults, a suspicious transaction is detected. Each of these occurrences triggers downstream actions across multiple bounded contexts. Domain events are the mechanism for communicating these facts across context boundaries while maintaining loose coupling.

## Key Concepts

### Event as Fact

A domain event represents something that has already happened. It is not a request or a command. `PaymentSettled` means the payment has been settled -- there is no option to reject it. Other contexts can react to it, but they cannot prevent it retroactively.

### Event Payload

Events carry enough data for consumers to react without needing to query the source context. A `TradeExecuted` event includes the trade ID, security, quantity, price, counterparty, and timestamp -- not just "a trade happened."

### Event Ordering

In finance, event ordering matters. Interest must be accrued before end-of-day balance calculations. Trade executions must be processed before settlement. Event infrastructure must preserve ordering guarantees where required.

## Finance Domain Events

### Accounts & Ledger Context Events

- **JournalEntryPosted** -- A journal entry has been recorded in the general ledger with balanced debits and credits
- **AccountOpened** -- A new ledger account has been created in the chart of accounts
- **AccountClosed** -- A ledger account has been closed and will no longer accept entries
- **FiscalPeriodClosed** -- A fiscal period (month, quarter, year) has been finalized
- **TrialBalanceGenerated** -- A trial balance report has been produced showing all account balances

### Payments & Transfers Context Events

- **PaymentInitiated** -- A payment order has been created and submitted for processing
- **PaymentAuthorized** -- A payment has passed authorization checks (fraud, limits, funds availability)
- **PaymentSettled** -- Funds have been irrevocably transferred from payer to payee
- **PaymentReversed** -- A previously settled payment has been reversed
- **PaymentRejected** -- A payment has been rejected due to insufficient funds, sanctions hit, or other reasons

### Lending & Credit Context Events

- **LoanApplicationSubmitted** -- A borrower has submitted a loan application
- **UnderwritingCompleted** -- The underwriting process has produced an approval or denial decision
- **LoanDisbursed** -- Loan funds have been released to the borrower
- **RepaymentReceived** -- A scheduled or unscheduled repayment has been received
- **LoanDefaulted** -- A loan has been classified as defaulted after exceeding delinquency thresholds

### Trading & Investments Context Events

- **OrderSubmitted** -- A buy or sell order has been entered into the order management system
- **TradeExecuted** -- An order has been matched and filled at a specific price and quantity
- **TradeAllocated** -- A block trade has been allocated across client portfolios
- **TradeSettled** -- Securities and cash have been exchanged to complete the trade
- **PositionUpdated** -- A portfolio position has changed due to a trade, corporate action, or valuation

### Risk & Compliance Context Events

- **RiskAssessmentCompleted** -- A risk evaluation has been finalized with a score and recommendation
- **RiskLimitBreached** -- An exposure or position limit has been exceeded
- **SuspiciousActivityDetected** -- A transaction or pattern has been flagged for potential fraud or money laundering
- **SanctionsHitDetected** -- A party has matched against a sanctions or watchlist
- **RegulatoryReportSubmitted** -- A required regulatory report has been filed

### Customer Onboarding Context Events

- **CustomerApplicationReceived** -- A new customer application has been submitted
- **IdentityVerified** -- The customer's identity has been confirmed through verification checks
- **KYCCompleted** -- Know Your Customer due diligence has been completed
- **CustomerOnboarded** -- The customer has been fully activated and enrolled in products
- **CustomerRelationshipClosed** -- The customer relationship has been terminated

## Event Design Guidelines

1. **Name events in past tense**: PaymentSettled, not SettlePayment
2. **Include sufficient payload**: Consumers should not need to call back to the source for essential data
3. **Make events immutable**: Once published, an event is a historical fact
4. **Include metadata**: Timestamp, correlation ID, causation ID, and source context
5. **Version events**: Use schema versioning to evolve event structures without breaking consumers
6. **Ensure idempotent handling**: Consumers must handle duplicate event delivery gracefully

## Relationships to Other Topics

- [Aggregates](../aggregates/) -- Aggregates produce domain events when state changes occur
- [Event-Driven Architecture](../event-driven-architecture/) -- The infrastructure for publishing and consuming events
- [Integration Patterns](../integration-patterns/) -- Events are the primary inter-context communication mechanism
- [Entities](../entities/) -- Entity lifecycle transitions produce domain events
- [Repositories](../repositories/) -- Event sourcing stores events as the primary record of state

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 8: Domain Events. Addison-Wesley, 2013.
- Vernon, Vaughn. "Domain-Driven Design Distilled." Chapter 6: Tactical Design with Domain Events. Addison-Wesley, 2016.
- Fowler, Martin. "Domain Event." https://martinfowler.com/eaaDev/DomainEvent.html
- Young, Greg. "Event Sourcing." CQRS Documents, 2010.
