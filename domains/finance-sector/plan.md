# Finance DDD - Project Plan

## Goal

Create comprehensive Domain-Driven Design documentation for a finance system, covering strategic design, tactical design, bounded contexts, and cross-cutting concerns.

## Phases

### Phase 1: Scaffolding [x]

Create foundational project files:

- [x] CLAUDE.md - Domain-specific instructions and conventions
- [x] plan.md - This project plan
- [x] tasks.md - Task tracking checklist
- [x] ubiquitous-language.md - Ubiquitous language glossary
- [x] index.md - Main documentation entry point
- [x] topics/index.md - Topics directory index

### Phase 2: Strategic Design Topics [x]

Document the foundational DDD concepts applied to finance:

- [x] Strategic design principles
- [x] Bounded contexts and their boundaries
- [x] Context map showing relationships between contexts
- [x] Ubiquitous language conventions
- [x] Subdomain identification (core, supporting, generic)
- [x] Anti-corruption layers for external system integration

### Phase 3: Tactical Design Topics [x]

Document DDD tactical patterns within the finance domain:

- [x] Aggregates (Account + JournalEntry + LineItem)
- [x] Entities (Account, Loan, Trade, Payment)
- [x] Value objects (Money, IBAN, InterestRate, CreditScore)
- [x] Domain events (PaymentProcessed, TradeExecuted, LoanDisbursed)
- [x] Repositories for aggregate root persistence
- [x] Domain services spanning multiple aggregates

### Phase 4: Bounded Context Topics [x]

Document each bounded context in detail:

- [x] Accounts & Ledger context
- [x] Payments & Transfers context
- [x] Lending & Credit context
- [x] Risk & Compliance context
- [x] Trading & Investments context
- [x] Customer Onboarding context

### Phase 5: Cross-Cutting Concerns [x]

Document integration and compliance topics:

- [x] Event-driven architecture patterns
- [x] Integration patterns between contexts
- [x] Finance standards (ISO 20022, SWIFT, FIX, XBRL, PCI DSS, NACHA/ACH, FpML, IBAN/BIC)
- [x] Regulatory compliance (Basel III/IV, SOX, Dodd-Frank, PSD2, GDPR, KYC/AML, MiFID II, FATCA)

### Phase 6: Review and Finalize [x]

- [x] Update tasks.md to reflect completion
- [x] Verify all symlinks and file structure
- [x] Cross-reference topics for consistency

## References

- Evans, Eric. "Domain-Driven Design." Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
- Vernon, Vaughn. "Domain-Driven Design Distilled." Addison-Wesley, 2016.
- Millett & Tune. "Patterns, Principles, and Practices of DDD." Wrox, 2015.
