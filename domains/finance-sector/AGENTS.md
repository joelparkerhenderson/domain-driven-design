# Finance - Domain-Driven Design

This domain applies Domain-Driven Design (DDD) to finance, creating a modular and scalable system by modeling software directly after complex financial workflows.

## Bounded Contexts

- **Accounts & Ledger Context** - Chart of accounts, general ledger, journal entries, trial balance
- **Payments & Transfers Context** - Payment processing, wire transfers, ACH, real-time payments
- **Lending & Credit Context** - Loan origination, underwriting, servicing, collections
- **Risk & Compliance Context** - Risk assessment, KYC/AML, fraud detection, regulatory reporting
- **Trading & Investments Context** - Securities trading, portfolio management, market data, settlement
- **Customer Onboarding Context** - Account opening, identity verification, KYC, product enrollment

## Ubiquitous Language

The shared vocabulary between developers and financial domain experts is defined in `ubiquitous-language.md`.

## Directory Structure

- `index.md` - Main documentation entry point
- `plan.md` - Project plan
- `tasks.md` - Task tracking
- `ubiquitous-language.md` - Ubiquitous language glossary
- `topics/` - Detailed topic documentation

## Conventions

- Every directory has `index.md` with a symlink `README.md` -> `index.md`
- No images, diagrams, web apps, front-end, or back-end code
- Comprehensive documentation with references and citations
- Ubiquitous language terms: one per line, one-sentence explanation

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
- Vernon, Vaughn. "Domain-Driven Design Distilled." Addison-Wesley, 2016.
- Millett, Scott & Tune, Nick. "Patterns, Principles, and Practices of Domain-Driven Design." Wrox, 2015.
