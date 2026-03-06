# Repositories in Finance

## Overview

A repository is a mechanism for encapsulating the storage, retrieval, and search behavior for aggregate roots. It provides an interface that speaks the domain language, hiding the underlying persistence technology. The domain model interacts with repositories as if they were in-memory collections of domain objects.

## Relevance to Finance

Financial systems must persist vast amounts of data: millions of transactions, accounts, trades, and regulatory records. Repositories provide a clean abstraction that separates the domain logic from storage concerns, whether the underlying technology is a relational database, event store, distributed ledger, or data warehouse.

## Key Concepts

### One Repository Per Aggregate Root

Each aggregate root has exactly one repository. There is no repository for internal entities or value objects -- they are accessed through their aggregate root. For example:

- `AccountRepository` retrieves Account aggregates (which include JournalEntries)
- `LoanRepository` retrieves Loan aggregates (which include AmortizationSchedules)
- `TradeRepository` retrieves Trade aggregates (which include Allocations)

### Domain-Centric Interface

Repository methods use domain language, not database language:

- `findByAccountNumber(accountNumber)` -- not `selectByPrimaryKey(id)`
- `findOverdueLoans(asOfDate)` -- not `queryWhere("status = 'DELINQUENT' AND due_date < ?")`
- `findTradesByPortfolio(portfolioId, dateRange)` -- not `joinTradesPositions()`

### Persistence Ignorance

The domain model does not know or care how data is stored. The repository interface is defined in the domain layer; the implementation lives in the infrastructure layer.

## Finance Repositories

### AccountRepository (Accounts & Ledger Context)

- `findByAccountNumber(AccountNumber)` -- Retrieve a specific ledger account
- `findByType(AccountType)` -- Retrieve all accounts of a given type (asset, liability, equity, revenue, expense)
- `findByFiscalPeriod(FiscalPeriod)` -- Retrieve accounts with activity in a fiscal period
- `save(Account)` -- Persist an account and its journal entries

### PaymentRepository (Payments & Transfers Context)

- `findByPaymentId(PaymentID)` -- Retrieve a specific payment
- `findByStatus(PaymentStatus)` -- Retrieve payments in a given status (pending, settled, reversed)
- `findByDateRange(DateRange)` -- Retrieve payments within a date window
- `findByPayerOrPayee(AccountIdentifier)` -- Retrieve payments involving a specific party
- `save(Payment)` -- Persist a payment and its clearing records

### LoanRepository (Lending & Credit Context)

- `findByLoanId(LoanID)` -- Retrieve a specific loan
- `findByBorrower(CustomerID)` -- Retrieve all loans for a borrower
- `findOverdueLoans(Date)` -- Retrieve loans with missed payments as of a date
- `findByMaturityRange(DateRange)` -- Retrieve loans maturing within a date window
- `save(Loan)` -- Persist a loan and its amortization schedule

### TradeRepository (Trading & Investments Context)

- `findByTradeId(TradeID)` -- Retrieve a specific trade
- `findByPortfolio(PortfolioID)` -- Retrieve trades for a portfolio
- `findUnsettledTrades(Date)` -- Retrieve trades pending settlement
- `findBySecurity(SecurityIdentifier)` -- Retrieve trades for a specific security
- `save(Trade)` -- Persist a trade and its allocations

### RiskAssessmentRepository (Risk & Compliance Context)

- `findByAssessmentId(AssessmentID)` -- Retrieve a specific risk assessment
- `findBySubject(SubjectID)` -- Retrieve assessments for a customer, counterparty, or transaction
- `findActiveAssessments(Date)` -- Retrieve assessments that have not expired
- `save(RiskAssessment)` -- Persist an assessment and its risk factors

### CustomerRepository (Customer Onboarding Context)

- `findByCustomerId(CustomerID)` -- Retrieve a specific customer profile
- `findByTaxIdentifier(TaxID)` -- Retrieve a customer by tax identifier for deduplication
- `findPendingVerification()` -- Retrieve customers awaiting identity verification
- `save(Customer)` -- Persist a customer profile and verification records

## Event Sourcing Consideration

Financial systems are strong candidates for event sourcing, where the repository stores a sequence of domain events rather than current state. This approach provides a complete audit trail -- a requirement in many regulatory contexts.

- **Account ledger**: Naturally event-sourced as a sequence of journal entries
- **Payment lifecycle**: Each status transition is an event in the payment's history
- **Trade lifecycle**: Execution, allocation, and settlement are sequential events

With event sourcing, the repository reconstructs the current state by replaying events, and snapshots optimize read performance for aggregates with long event histories.

## Relationships to Other Topics

- [Aggregates](../aggregates/) -- Repositories store and retrieve aggregate roots
- [Entities](../entities/) -- Aggregate roots accessed via repositories are entities
- [Domain Events](../domain-events/) -- Event-sourced repositories store events as the primary record
- [Domain Services](../domain-services/) -- Domain services use repositories to load aggregates for coordination

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 6: The Life Cycle of a Domain Object. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 12: Repositories. Addison-Wesley, 2013.
- Millett, Scott & Tune, Nick. "Patterns, Principles, and Practices of Domain-Driven Design." Chapter 21: Repositories. Wrox, 2015.
- Fowler, Martin. "Patterns of Enterprise Application Architecture." Chapter 10: Data Source Architectural Patterns. Addison-Wesley, 2002.
- Young, Greg. "CQRS and Event Sourcing." CQRS Documents, 2010.
