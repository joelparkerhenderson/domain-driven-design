# Entities in Finance

## Overview

An entity is a domain object defined by its unique identity that persists over time, even as its attributes change. Two entities with the same attributes but different identities are different objects. Entities are mutable: their state evolves through business operations, but their identity remains constant.

## Relevance to Finance

Finance is built on trackable, auditable records. Every account, loan, trade, and payment must be uniquely identifiable and traceable throughout its lifecycle. An account opened ten years ago with a different balance, different address, and different interest rate is still the same account because its identity (account number) has not changed.

## Key Concepts

### Identity

Each entity has a unique identifier that distinguishes it from all other entities of the same type. In finance, identifiers often follow industry standards:

- Account numbers (internal or IBAN format)
- Loan IDs
- Trade IDs
- Payment reference numbers
- Customer IDs
- Security identifiers (ISIN, CUSIP, SEDOL)

### Lifecycle

Entities have lifecycles that the domain model must track:

- An Account is opened, active, dormant, frozen, or closed
- A Loan is applied for, approved, disbursed, performing, delinquent, defaulted, or paid off
- A Trade is submitted, executed, allocated, settled, or cancelled
- A Payment is initiated, authorized, cleared, settled, or reversed

### Equality

Two entities are equal if and only if they have the same identity. An Account with AccountNumber "1234" is the same Account regardless of its current balance or status.

## Finance Entities

### Account (Accounts & Ledger Context)

- **Identity**: AccountNumber
- **Attributes**: AccountType, AccountName, Currency, Balance, Status, OpenDate
- **Lifecycle**: Opened -> Active -> (Dormant | Frozen) -> Closed
- **Behavior**: Post journal entries, calculate balance, close account, freeze for investigation

### Payment (Payments & Transfers Context)

- **Identity**: PaymentID
- **Attributes**: Amount, Currency, PayerAccount, PayeeAccount, PaymentMethod, Status, InitiatedDate, SettledDate
- **Lifecycle**: Initiated -> Authorized -> Submitted -> Cleared -> Settled (or Rejected/Reversed)
- **Behavior**: Authorize, submit for clearing, settle, reverse

### Loan (Lending & Credit Context)

- **Identity**: LoanID
- **Attributes**: Principal, InterestRate, Term, StartDate, MaturityDate, OutstandingBalance, Status
- **Lifecycle**: Applied -> Underwritten -> Approved -> Disbursed -> Performing -> (Delinquent -> Defaulted | Paid Off)
- **Behavior**: Disburse, receive repayment, apply prepayment, charge interest, default

### Trade (Trading & Investments Context)

- **Identity**: TradeID
- **Attributes**: SecurityIdentifier, Quantity, Price, Side (Buy/Sell), TradeDate, SettlementDate, Status
- **Lifecycle**: Submitted -> Executed -> Allocated -> Settled (or Cancelled)
- **Behavior**: Execute, allocate to portfolios, settle, cancel

### Customer (Customer Onboarding Context)

- **Identity**: CustomerID
- **Attributes**: LegalName, DateOfBirth, TaxIdentifier, RiskRating, OnboardingStatus
- **Lifecycle**: Applied -> Verified -> Active -> (Suspended | Closed)
- **Behavior**: Verify identity, complete KYC, activate, suspend, close relationship

### RiskAssessment (Risk & Compliance Context)

- **Identity**: AssessmentID
- **Attributes**: SubjectID, RiskType, RiskScore, AssessmentDate, ExpiryDate, Status
- **Lifecycle**: Initiated -> Evaluated -> Completed -> (Approved | Escalated) -> Expired
- **Behavior**: Evaluate risk factors, compute score, approve, escalate, expire

## Entity Design Guidelines

1. **Assign identity early**: Financial entities should receive their unique identifier at creation time
2. **Model lifecycle explicitly**: Use status fields or state machines to track entity lifecycle transitions
3. **Protect invariants**: Entities enforce business rules through their methods, never allowing invalid state
4. **Audit all changes**: Financial entities should track who changed what and when for regulatory compliance
5. **Separate identity from external identifiers**: An internal LoanID may differ from the customer-facing loan reference number

## Relationships to Other Topics

- [Aggregates](../aggregates/) -- Entities serve as aggregate roots or internal members of aggregates
- [Value Objects](../value-objects/) -- Entities contain value objects for immutable attribute groups
- [Domain Events](../domain-events/) -- Entity state transitions produce domain events
- [Repositories](../repositories/) -- Repositories store and retrieve entity instances (aggregate roots)
- [Ubiquitous Language](../ubiquitous-language/) -- Entity names come directly from the ubiquitous language

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 5: A Model Expressed in Software. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 5: Entities. Addison-Wesley, 2013.
- Millett, Scott & Tune, Nick. "Patterns, Principles, and Practices of Domain-Driven Design." Chapter 17: Domain Model Building Blocks. Wrox, 2015.
