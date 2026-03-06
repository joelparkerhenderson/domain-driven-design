# Finance/Accounting Context in Enterprise Resource Planning

## Overview

The Finance/Accounting Context manages the financial backbone of the organization: general ledger, chart of accounts, journal entries, accounts payable, accounts receivable, financial reporting, tax management, and fiscal period operations. Every business transaction eventually produces a financial entry in this context.

## Core Responsibilities

- **General Ledger**: Maintaining the chart of accounts and recording all financial transactions as journal entries
- **Accounts Payable**: Tracking money owed to suppliers, processing payments, managing cash disbursements
- **Accounts Receivable**: Tracking money owed by customers, processing payments, managing collections
- **Financial Reporting**: Generating balance sheets, income statements, cash flow statements, and trial balances
- **Tax Management**: Calculating, accruing, and reporting taxes across jurisdictions
- **Fiscal Period Management**: Opening, closing, and locking accounting periods

## Aggregates

### JournalEntry Aggregate

- **Root**: JournalEntry
- **Internal entities**: JournalLine (debit/credit entries)
- **Value objects**: Money, AccountReference, FiscalPeriod, PostingDate
- **Invariants**: Debits must equal credits; cannot post to closed periods; posted entries are immutable

### Invoice Aggregate (AR)

- **Root**: Invoice
- **Internal entities**: InvoiceLineItem
- **Value objects**: Money, TaxRate, PaymentTerms, DueDate
- **Invariants**: Invoice total equals sum of lines plus tax; issued invoices are immutable

### Payment Aggregate

- **Root**: Payment
- **Value objects**: Money, PaymentMethod, BankReference
- **Invariants**: Payment amount must be positive; payment must reference valid invoice(s)

## Domain Events

| Event | Trigger | Consumers |
|-------|---------|-----------|
| JournalEntryPosted | Entry recorded in GL | Financial reporting, Audit |
| InvoiceIssued | Customer invoice created | AR tracking, Customer notification |
| PaymentReceived | Customer payment processed | AR clearing, Cash management |
| SupplierPaymentProcessed | Supplier paid | AP clearing, Cash management |
| FiscalPeriodClosed | Period-end close | Reporting, Audit, Management |

## Integration with Other Contexts

- **Sales → Finance**: OrderShipped triggers revenue recognition; InvoiceIssued creates AR
- **Procurement → Finance**: PurchaseOrderApproved creates commitment; GoodsReceived triggers accrual; SupplierInvoiceReceived creates AP
- **Inventory → Finance**: StockReceived/Issued triggers inventory valuation entries; StockAdjusted triggers adjustment entries
- **HR → Finance**: PayrollProcessed creates salary expense entries
- **Manufacturing → Finance**: ProductionCompleted triggers WIP-to-finished-goods entries

## Accounting Standards

- **GAAP** (Generally Accepted Accounting Principles) — U.S. standard
- **IFRS** (International Financial Reporting Standards) — International standard
- **Double-entry bookkeeping** — Every transaction affects at least two accounts
- **Accrual basis** — Revenue and expenses recognized when earned/incurred, not when cash changes hands

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) — One of six ERP bounded contexts
- [Context Map](../context-map/) — Downstream consumer of Sales, Procurement, Inventory, HR, Manufacturing
- [Domain Events](../domain-events/) — Financial entries triggered by events from other contexts
- [Aggregates](../aggregates/) — JournalEntry, Invoice, Payment are aggregate roots
- [Regulatory Compliance](../regulatory-compliance/) — SOX, GAAP/IFRS compliance requirements

## References

- Evans, Eric. "Domain-Driven Design." Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
- Kieso, Weygandt, & Warfield. "Intermediate Accounting." Wiley, published periodically.
- FASB. "Accounting Standards Codification." Financial Accounting Standards Board.
- IASB. "International Financial Reporting Standards." IFRS Foundation.
