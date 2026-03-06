# Context Map in Finance

## Overview

A context map is a visual and conceptual representation of the relationships and interactions between bounded contexts. It documents how models in different contexts relate to each other, the nature of their integration, and the power dynamics between teams. The context map is one of the most important strategic design tools because it makes inter-context dependencies explicit.

## Relevance to Finance

Financial systems are deeply interconnected. A single customer transaction may flow through customer onboarding, accounts and ledger, payments, risk and compliance, and trading contexts. The context map makes these integration paths visible and helps teams understand their dependencies and responsibilities.

## Context Relationships in Finance

### Accounts & Ledger <-> Payments & Transfers

- **Relationship type**: Customer-Supplier
- **Direction**: Payments is the upstream supplier of settlement data; Accounts & Ledger is the downstream consumer that posts journal entries
- **Integration**: When a payment is settled, the Payments context publishes a PaymentSettled event. The Accounts & Ledger context consumes this event and creates the corresponding journal entries
- **Potential conflict**: Timing differences between payment processing and ledger posting require reconciliation processes

### Accounts & Ledger <-> Lending & Credit

- **Relationship type**: Customer-Supplier
- **Direction**: Lending produces loan disbursement and repayment events; Accounts & Ledger consumes them for booking
- **Integration**: LoanDisbursed and RepaymentReceived events trigger journal entries in the general ledger
- **Shared concept**: Interest accrual calculations must be consistent between both contexts

### Accounts & Ledger <-> Trading & Investments

- **Relationship type**: Customer-Supplier
- **Direction**: Trading produces trade settlement data; Accounts & Ledger books the resulting positions and cash movements
- **Integration**: TradeSettled events create journal entries reflecting securities and cash transfers

### Payments & Transfers <-> Risk & Compliance

- **Relationship type**: Conformist (Payments conforms to compliance rules)
- **Direction**: Risk & Compliance sets fraud screening and AML rules; Payments must comply
- **Integration**: Payment orders are screened against sanctions lists and fraud rules before execution. Risk & Compliance publishes screening policies; Payments applies them

### Lending & Credit <-> Risk & Compliance

- **Relationship type**: Partnership
- **Direction**: Bidirectional; Lending supplies borrower data for risk assessment; Risk provides credit decisions and risk ratings
- **Integration**: Loan applications trigger credit risk assessments. Risk decisions flow back to authorize or deny origination

### Customer Onboarding <-> Risk & Compliance

- **Relationship type**: Customer-Supplier
- **Direction**: Customer Onboarding supplies customer data; Risk & Compliance performs KYC/AML screening
- **Integration**: New customer applications trigger KYC verification workflows. Compliance decisions gate account activation

### Trading & Investments <-> Risk & Compliance

- **Relationship type**: Conformist
- **Direction**: Risk & Compliance sets position limits, margin requirements, and pre-trade risk checks; Trading must comply
- **Integration**: Pre-trade risk checks validate orders against exposure limits before execution

### Customer Onboarding <-> All Downstream Contexts

- **Relationship type**: Published Language
- **Direction**: Customer Onboarding publishes verified customer profiles consumed by all other contexts
- **Integration**: CustomerOnboarded event distributes core customer identity data (CustomerID, verified name, risk rating)

## Integration Pattern Summary

| Upstream | Downstream | Pattern | Communication |
|----------|-----------|---------|---------------|
| Payments & Transfers | Accounts & Ledger | Customer-Supplier | Domain events |
| Lending & Credit | Accounts & Ledger | Customer-Supplier | Domain events |
| Trading & Investments | Accounts & Ledger | Customer-Supplier | Domain events |
| Risk & Compliance | Payments & Transfers | Conformist | Published policies |
| Risk & Compliance | Trading & Investments | Conformist | Pre-trade checks |
| Risk & Compliance | Lending & Credit | Partnership | Bidirectional events |
| Customer Onboarding | All contexts | Published Language | Domain events |

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) -- The nodes on the context map
- [Strategic Design](../strategic-design/) -- Context mapping is a core strategic design activity
- [Integration Patterns](../integration-patterns/) -- Details the integration approaches used between contexts
- [Anti-Corruption Layer](../anti-corruption-layer/) -- Used at context boundaries to translate between models
- [Event-Driven Architecture](../event-driven-architecture/) -- The primary communication mechanism between contexts

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 14: Maintaining Model Integrity. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 3: Context Maps. Addison-Wesley, 2013.
- Vernon, Vaughn. "Domain-Driven Design Distilled." Chapter 4: Strategic Design with Context Mapping. Addison-Wesley, 2016.
- Brandolini, Alberto. "Introducing EventStorming." Leanpub, 2021.
