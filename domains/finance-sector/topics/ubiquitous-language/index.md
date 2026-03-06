# Ubiquitous Language in Finance

## Overview

Ubiquitous language is the shared vocabulary used by developers and domain experts within a bounded context. Every term in the ubiquitous language has a precise, agreed-upon meaning, and this language is used consistently in conversations, documentation, code, and tests. The ubiquitous language eliminates translation between "business speak" and "tech speak."

## Relevance to Finance

Finance is a domain rich with specialized terminology. Terms like "settlement," "account," "position," "exposure," and "maturity" carry precise meanings that vary between contexts. Establishing a rigorous ubiquitous language is critical because:

- Miscommunication about financial terms can lead to incorrect calculations, regulatory violations, or financial losses
- Regulators expect precise terminology in reporting and audit trails
- Multiple professional disciplines (accounting, trading, risk management, compliance) each have established vocabularies that must be reconciled within each bounded context
- Industry standards like ISO 20022 define canonical term definitions that the ubiquitous language should align with where appropriate

## Key Principles

### One Term, One Meaning Per Context

Within the Accounts & Ledger context, "account" means a ledger category. Within the Customer Onboarding context, "account" means a customer relationship. Neither definition is wrong; they belong to different bounded contexts with different ubiquitous languages.

### Language Drives the Model

The ubiquitous language is not just documentation -- it shapes the code. If domain experts say "post a journal entry," the code should have a method like `journalEntry.post()`, not `journalEntry.save()` or `journalEntry.commit()`.

### Language Evolves Through Collaboration

The ubiquitous language is refined through ongoing conversations between developers and domain experts. When a trader describes how "an order gets filled," that language should appear directly in the model.

## Finance-Specific Language Challenges

### Homonyms Across Contexts

| Term | Accounts & Ledger | Payments | Trading | Lending |
|------|-------------------|----------|---------|---------|
| Account | Ledger category | Payment account | Brokerage account | Loan account |
| Settlement | Period close | Funds transfer completion | Securities delivery | Loan payoff |
| Balance | Debit/credit total | Available funds | Position value | Outstanding principal |
| Order | Not applicable | Payment order | Buy/sell instruction | Not applicable |
| Interest | Not applicable | Not applicable | Not applicable | Periodic charge on principal |

### Domain Expert Roles

Different financial professionals bring different vocabulary:

- **Accountants** speak of debits, credits, accruals, and fiscal periods
- **Traders** speak of positions, fills, lots, and market depth
- **Risk analysts** speak of exposure, VaR, stress testing, and loss given default
- **Compliance officers** speak of screening, suspicious activity, beneficial ownership, and enhanced due diligence
- **Loan officers** speak of origination, underwriting, disbursement, and delinquency

### Alignment with Standards

The finance domain benefits from aligning ubiquitous language with established standards:

- ISO 20022 defines standardized message element names
- FIX protocol provides canonical trading terminology
- Basel accords establish risk measurement terminology
- GAAP and IFRS define accounting terminology

## Building the Ubiquitous Language

1. **Conduct domain workshops** with financial experts from each bounded context
2. **Identify ambiguous terms** that mean different things to different teams
3. **Resolve ambiguity** by assigning specific definitions within each bounded context
4. **Document the glossary** in the ubiquitous-language.md file and keep it current
5. **Enforce in code** by naming classes, methods, and events using the agreed vocabulary
6. **Review regularly** as the domain model evolves and new concepts emerge

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) -- Each bounded context has its own ubiquitous language
- [Strategic Design](../strategic-design/) -- Language discovery is a key strategic design activity
- [Context Map](../context-map/) -- Published languages define inter-context communication vocabularies
- [Finance Standards](../finance-standards/) -- Standards provide canonical terminology the language should align with
- [Entities](../entities/) -- Entity names come directly from the ubiquitous language
- [Value Objects](../value-objects/) -- Value object names come directly from the ubiquitous language

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 2: Communication and the Use of Language. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 1: Getting Started with DDD. Addison-Wesley, 2013.
- Vernon, Vaughn. "Domain-Driven Design Distilled." Chapter 1: DDD for Me. Addison-Wesley, 2016.
- ISO. "ISO 20022 Data Dictionary." https://www.iso20022.org/
- FIX Trading Community. "FIX Protocol Specification." https://www.fixtrading.org/
