# Accounts & Ledger Context

## Overview

The Accounts & Ledger context is the financial record-keeping backbone of the finance domain. It manages the chart of accounts, general ledger, journal entries, and trial balance. Every financial transaction across the institution -- payments, trades, loan disbursements, fee assessments -- ultimately produces journal entries in this context. It is the system of record for the institution's financial position.

## Core Concepts

### Chart of Accounts

The chart of accounts is the structured classification system that organizes all financial accounts into categories. Each account has a unique number, a name, and a type.

- **Asset accounts** (1xxx): Cash, securities held, loans receivable, property
- **Liability accounts** (2xxx): Deposits, borrowings, accrued expenses
- **Equity accounts** (3xxx): Retained earnings, capital stock, reserves
- **Revenue accounts** (4xxx): Interest income, fee income, trading gains
- **Expense accounts** (5xxx): Interest expense, operating costs, provisions

### General Ledger

The general ledger is the master accounting record containing all financial transactions organized by chart of accounts. It maintains the complete history of every debit and credit posted to every account, enabling financial statement generation and audit.

### Journal Entries

A journal entry is the fundamental unit of financial recording. Each entry contains one or more line items (debits and credits) that must balance. Journal entries are immutable once posted -- corrections are made through reversing entries.

- **Manual entries**: Created by accountants for adjustments, accruals, and reclassifications
- **System-generated entries**: Automatically created from domain events in other contexts (e.g., PaymentSettled triggers a journal entry)

### Trial Balance

A trial balance is a report listing the balances of all general ledger accounts at a specific date, verifying that total debits equal total credits. It is the starting point for preparing financial statements.

### Fiscal Periods

The ledger organizes transactions into fiscal periods (monthly, quarterly, annually). Period close processes lock prior periods against new postings and generate financial statements.

## Aggregates in This Context

### Account Aggregate

- **Root**: Account (AccountNumber)
- **Contains**: JournalEntries, LineItems
- **Value objects**: Money, AccountType, FiscalPeriod
- **Key invariant**: Total debits must equal total credits for each journal entry

### FiscalPeriod Aggregate

- **Root**: FiscalPeriod (PeriodID)
- **Contains**: PeriodStatus, ClosingAdjustments
- **Key invariant**: A closed period cannot accept new journal entries

## Domain Events

- JournalEntryPosted -- triggers downstream reporting and reconciliation
- AccountOpened -- new account added to the chart of accounts
- AccountClosed -- account deactivated
- FiscalPeriodClosed -- period finalized, financial statements generated
- TrialBalanceGenerated -- balance verification completed

## Integration with Other Contexts

This context is the **downstream consumer** of financial events from all other contexts:

- **Payments & Transfers**: PaymentSettled events create cash movement journal entries
- **Lending & Credit**: LoanDisbursed, RepaymentReceived, and InterestAccrued events create loan-related journal entries
- **Trading & Investments**: TradeSettled events create securities and cash journal entries
- **Risk & Compliance**: Provision adjustments and write-offs are posted as journal entries

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) -- One of the six bounded contexts in the finance domain
- [Aggregates](../aggregates/) -- Account and FiscalPeriod aggregates defined here
- [Value Objects](../value-objects/) -- Money, AccountType, and FiscalPeriod are key value objects
- [Domain Events](../domain-events/) -- This context both consumes events from other contexts and produces its own
- [Finance Standards](../finance-standards/) -- XBRL for financial reporting; GAAP/IFRS for accounting rules
- [Integration Patterns](../integration-patterns/) -- Downstream consumer of events from all operational contexts

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
- Kieso, Weygandt, Warfield. "Intermediate Accounting." Wiley, 2019.
- Financial Accounting Standards Board. "Accounting Standards Codification (ASC)." https://asc.fasb.org/
- XBRL International. "Extensible Business Reporting Language." https://www.xbrl.org/
