# Lending & Credit Context

## Overview

The Lending & Credit context manages the full lifecycle of loans from origination through servicing to payoff or collection. It handles credit applications, underwriting decisions, loan disbursement, repayment scheduling, interest accrual, delinquency management, and collections. This context encapsulates the core business logic of credit risk assessment and loan portfolio management.

## Core Concepts

### Loan Origination

The end-to-end process of creating a new loan, from the initial application through credit evaluation, underwriting, approval, and disbursement. Origination involves:

- **Application intake**: Capturing borrower information, requested amount, purpose, and term
- **Credit evaluation**: Pulling credit reports, calculating debt-to-income ratios, assessing collateral
- **Underwriting**: Applying credit policy rules to produce an approval, conditional approval, or denial
- **Closing**: Finalizing loan terms, executing documents, and establishing the loan account

### Underwriting

The process of evaluating and assuming the risk of a loan. Underwriting combines quantitative analysis (credit scores, financial ratios) with qualitative judgment (employment stability, industry outlook). The underwriting decision determines:

- Whether to approve, conditionally approve, or deny the loan
- The interest rate and pricing tier
- Required collateral or guarantees
- Covenants and conditions

### Amortization

The systematic repayment of loan principal over time according to a defined schedule. Common amortization methods:

- **Fixed-rate fully amortizing**: Equal payments of principal and interest over the loan term
- **Adjustable-rate**: Interest rate resets periodically based on a benchmark index plus margin
- **Interest-only**: Borrower pays only interest for a period, then begins principal amortization
- **Balloon**: Small periodic payments with a large final payment

### Servicing

Ongoing management of the loan after disbursement:

- Collecting and applying payments
- Managing escrow accounts (taxes, insurance)
- Sending statements and payment reminders
- Processing prepayments and payoffs
- Handling modifications and forbearance

### Collections

Managing delinquent loans through a structured process:

- **Early-stage delinquency** (1-30 days): Automated reminders and outreach
- **Mid-stage delinquency** (31-90 days): Active collection efforts, workout negotiations
- **Late-stage delinquency** (90+ days): Legal action, foreclosure, charge-off determination
- **Recovery**: Pursuing recovery on charged-off loans through legal means or sale

## Aggregates in This Context

### Loan Aggregate

- **Root**: Loan (LoanID)
- **Contains**: AmortizationSchedule, RepaymentRecord, CollateralItem
- **Value objects**: Money, InterestRate, LoanTerm, CreditScore
- **Key invariant**: Amortization schedule must cover the full principal; prepayments reduce outstanding balance

### LoanApplication Aggregate

- **Root**: LoanApplication (ApplicationID)
- **Contains**: ApplicantProfile, IncomeVerification, UnderwritingDecision
- **Value objects**: Money, CreditScore, DebtToIncomeRatio
- **Key invariant**: Application cannot proceed to closing without an approved underwriting decision

### CollectionCase Aggregate

- **Root**: CollectionCase (CaseID)
- **Contains**: CollectionAction, WorkoutAgreement
- **Key invariant**: Actions must follow the escalation sequence; legal action requires prior outreach attempts

## Domain Events

- LoanApplicationSubmitted -- triggers credit evaluation and underwriting workflow
- UnderwritingCompleted -- approval or denial decision produced
- LoanDisbursed -- funds released to borrower; triggers journal entry in Accounts & Ledger
- RepaymentReceived -- scheduled or unscheduled payment applied to the loan
- LoanDefaulted -- loan classified as defaulted; triggers provisioning in Risk & Compliance
- LoanPaidOff -- loan fully repaid and closed
- CollectionCaseOpened -- delinquent loan escalated to collections

## Integration with Other Contexts

- **Accounts & Ledger** (downstream): Loan disbursements, repayments, and interest accruals produce journal entries
- **Risk & Compliance** (partner): Credit risk assessments inform underwriting decisions; default events trigger risk recalculation
- **Payments & Transfers**: Loan disbursements and repayment collections flow through the payment infrastructure
- **Customer Onboarding** (upstream): Borrower identity and KYC data used in credit evaluation

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) -- One of the six bounded contexts in the finance domain
- [Aggregates](../aggregates/) -- Loan, LoanApplication, and CollectionCase aggregates defined here
- [Domain Events](../domain-events/) -- Origination, servicing, and collections events drive cross-context communication
- [Domain Services](../domain-services/) -- CreditDecisionService and InterestAccrualService operate in this context
- [Risk & Compliance Context](../risk-compliance-context/) -- Partnership relationship for credit risk assessment
- [Regulatory Compliance](../regulatory-compliance/) -- Truth in Lending (TILA), ECOA, Fair Lending regulations

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
- Saunders, Anthony & Cornett, Marcia. "Financial Institutions Management: A Risk Management Approach." McGraw-Hill, 2018.
- Office of the Comptroller of the Currency. "Comptroller's Handbook: Commercial Lending." OCC, 2017.
- Consumer Financial Protection Bureau. "Truth in Lending Act (TILA)." https://www.consumerfinance.gov/
