# Regulatory Compliance in Enterprise Resource Planning

## Overview

ERP systems must comply with financial, labor, data privacy, and industry-specific regulations. Compliance requirements directly shape domain model design — from how financial transactions are recorded to how employee data is protected.

## Financial Regulations

### SOX (Sarbanes-Oxley Act)

U.S. federal law requiring publicly traded companies to maintain internal controls over financial reporting.

**DDD implications**:

- **Audit trails**: Every journal entry, approval, and modification must be logged as immutable domain events
- **Segregation of duties**: Domain services enforce that the same person cannot create and approve a PO or journal entry
- **Access controls**: Bounded context APIs enforce role-based access — AP clerks cannot post journal entries; accountants cannot approve their own adjustments
- **Change management**: Modifications to chart of accounts, approval workflows, and financial rules require documented authorization

### GAAP / IFRS

Accounting standards governing financial statement preparation.

**DDD implications**:

- **Revenue recognition**: Finance context implements ASC 606 (GAAP) or IFRS 15 rules as domain logic — recognizing revenue when performance obligations are satisfied
- **Double-entry invariant**: JournalEntry aggregate enforces debits equal credits
- **Period-end close**: FiscalPeriodCloseService implements accruals, deferrals, and reclassifications per accounting standards
- **Depreciation**: Asset depreciation methods (straight-line, declining balance) modeled as domain strategies

### Tax Regulations

Multi-jurisdictional tax requirements: sales tax, VAT, GST, income tax, customs duties.

**DDD implications**:

- **TaxRate value object**: Captures jurisdiction, rate, effective dates
- **Tax calculation service**: Applies correct rates based on product category, origin/destination, and customer exemption status
- **Tax filing**: ACL translates internal financial data into jurisdiction-specific filing formats

## Data Privacy Regulations

### GDPR (General Data Protection Regulation)

EU regulation governing personal data processing.

**DDD implications**:

- **Right to erasure**: Employee and customer aggregates must support data anonymization or deletion
- **Data minimization**: Domain events carry only necessary personal data
- **Consent tracking**: Customer and Employee aggregates include consent status
- **Data portability**: System must export personal data in machine-readable format
- **Processing records**: All processing of personal data is logged

### CCPA (California Consumer Privacy Act)

California privacy law with requirements similar to GDPR for California residents.

## Labor Regulations

**DDD implications for HR Context**:

- **FLSA** (Fair Labor Standards Act): Overtime calculation rules modeled in PayrollCalculationService
- **FMLA** (Family and Medical Leave Act): Leave entitlement tracking in Employee aggregate
- **ACA** (Affordable Care Act): Benefits eligibility rules in BenefitsEnrollment
- **Equal Pay**: Compensation analysis domain services for pay equity reporting
- **I-9 verification**: Employment eligibility tracking

## Trade and Customs

- **Export controls**: Product classification against restricted lists
- **Customs duties**: HS code-based duty calculations
- **Country of origin**: Tracking for trade compliance
- **Sanctions screening**: Supplier and customer checks against sanctioned entity lists

## Compliance in Domain Design

### Audit Events

All significant actions produce immutable domain events:

- `JournalEntryPosted` (who, when, amounts, accounts)
- `PurchaseOrderApproved` (who, when, amount, authority level)
- `PayrollProcessed` (who, when, period, total amounts)
- `EmployeeDataAccessed` (who, when, what fields, purpose)

### Segregation of Duties

Domain services enforce that:

- PO creator ≠ PO approver
- Journal entry creator ≠ journal entry poster
- Payroll processor ≠ payroll approver
- Inventory adjuster ≠ inventory auditor

### Retention Policies

Financial records must be retained per regulation (typically 7 years for tax records). Event sourcing naturally preserves full history.

## Relationships to Other Topics

- [Domain Events](../domain-events/) — Events form the audit trail
- [Aggregates](../aggregates/) — Compliance shapes aggregate invariants
- [Finance/Accounting Context](../finance-accounting-context/) — SOX, GAAP/IFRS compliance
- [Human Resources Context](../human-resources-context/) — Labor law and GDPR compliance
- [Domain Services](../domain-services/) — Segregation of duties enforcement

## References

- U.S. Congress. "Sarbanes-Oxley Act of 2002." Public Law 107-204.
- FASB. "Accounting Standards Codification (ASC) 606: Revenue from Contracts with Customers."
- IASB. "IFRS 15: Revenue from Contracts with Customers."
- European Parliament. "General Data Protection Regulation (GDPR)." Regulation (EU) 2016/679.
- U.S. Department of Labor. "Fair Labor Standards Act (FLSA)." 29 U.S.C. § 201 et seq.
- IRS. "Record Retention Requirements." Publication 583.
