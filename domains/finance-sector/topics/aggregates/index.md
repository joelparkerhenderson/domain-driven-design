# Aggregates in Finance

## Overview

An aggregate is a cluster of domain objects that are treated as a single unit for the purpose of data changes and consistency enforcement. Each aggregate has a root entity (the aggregate root) through which all external access is managed. The aggregate root enforces invariants -- business rules that must always be true -- across all objects within the aggregate boundary.

## Relevance to Finance

Financial data involves complex relationships: an account has journal entries with line items; a loan has an amortization schedule with payment records; a trade has legs, allocations, and settlement instructions. Aggregates define which objects must be consistent together in a single transaction and which can be eventually consistent across transactions.

## Key Concepts

### Aggregate Root

The single entity through which all modifications to the aggregate pass. External code never directly modifies objects inside the aggregate -- it always goes through the root.

### Invariants

Business rules that must hold at all times within the aggregate boundary. For example:

- A journal entry must have balanced debits and credits before it can be posted
- A loan cannot be disbursed without an approved underwriting decision
- A trade order cannot exceed the available margin or position limits

### Transactional Consistency

All changes within an aggregate are committed or rolled back as a single transaction. Changes between aggregates are eventually consistent, communicated via domain events.

## Finance Aggregates

### Account Aggregate (Accounts & Ledger Context)

- **Root**: Account (identified by AccountNumber)
- **Internal entities**: JournalEntry, LineItem
- **Internal value objects**: Money, FiscalPeriod, AccountType
- **Invariants**:
  - Total debits must equal total credits for each journal entry
  - Account balance must be recalculated after each posted entry
  - Closed accounts cannot accept new journal entries
- **Events produced**: JournalEntryPosted, AccountClosed, TrialBalanceGenerated

### Payment Aggregate (Payments & Transfers Context)

- **Root**: Payment (identified by PaymentID)
- **Internal entities**: PaymentInstruction, ClearingRecord
- **Internal value objects**: Money, IBAN, BIC, PaymentMethod, PaymentStatus
- **Invariants**:
  - Payment amount must be positive and in a supported currency
  - Payment cannot be executed without passing fraud and sanctions screening
  - Settled payments cannot be modified (only reversed via new transactions)
- **Events produced**: PaymentInitiated, PaymentAuthorized, PaymentSettled, PaymentReversed

### Loan Aggregate (Lending & Credit Context)

- **Root**: Loan (identified by LoanID)
- **Internal entities**: AmortizationSchedule, RepaymentRecord, CollateralItem
- **Internal value objects**: Money, InterestRate, LoanTerm, CreditScore
- **Invariants**:
  - Loan cannot be disbursed without approved underwriting
  - Amortization schedule must cover the full principal over the loan term
  - Prepayment must be applied against outstanding principal according to loan terms
- **Events produced**: LoanOriginated, LoanDisbursed, RepaymentReceived, LoanDefaulted

### Trade Aggregate (Trading & Investments Context)

- **Root**: Trade (identified by TradeID)
- **Internal entities**: TradeLeg, Allocation
- **Internal value objects**: Money, Quantity, Price, SecurityIdentifier, SettlementDate
- **Invariants**:
  - Trade quantity and price must be positive
  - All allocations must sum to the total trade quantity
  - Settlement date must be a valid business day
- **Events produced**: TradeExecuted, TradeAllocated, TradeSettled, TradeCancelled

### RiskAssessment Aggregate (Risk & Compliance Context)

- **Root**: RiskAssessment (identified by AssessmentID)
- **Internal entities**: RiskFactor, MitigationAction
- **Internal value objects**: RiskScore, RiskCategory, ExposureAmount
- **Invariants**:
  - Risk assessment must have at least one evaluated risk factor
  - Assessment must be completed before it can be approved
  - Expired assessments cannot be used for new decisions
- **Events produced**: RiskAssessmentCompleted, RiskLimitBreached, RiskMitigated

## Aggregate Design Rules

1. **Keep aggregates small**: Prefer smaller aggregates with fewer internal objects. Large aggregates create contention and performance problems in high-throughput financial systems.
2. **Reference other aggregates by identity**: A Trade references a Portfolio by PortfolioID, not by containing the Portfolio object.
3. **Use domain events for cross-aggregate coordination**: When a PaymentSettled event occurs, the Accounts & Ledger context creates corresponding journal entries -- but this happens asynchronously, not in the same transaction.
4. **Design for eventual consistency between aggregates**: Accept that the ledger may reflect a settled payment milliseconds after the settlement occurs, not at the exact same instant.

## Relationships to Other Topics

- [Entities](../entities/) -- Aggregate roots are entities; aggregates contain entities
- [Value Objects](../value-objects/) -- Aggregates contain value objects for immutable domain concepts
- [Domain Events](../domain-events/) -- Aggregates produce events to communicate changes
- [Repositories](../repositories/) -- One repository per aggregate root
- [Domain Services](../domain-services/) -- Coordinate operations across multiple aggregates

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 6: The Life Cycle of a Domain Object. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 10: Aggregates. Addison-Wesley, 2013.
- Vernon, Vaughn. "Domain-Driven Design Distilled." Chapter 5: Tactical Design with Aggregates. Addison-Wesley, 2016.
- Millett, Scott & Tune, Nick. "Patterns, Principles, and Practices of Domain-Driven Design." Chapter 19: Aggregates. Wrox, 2015.
