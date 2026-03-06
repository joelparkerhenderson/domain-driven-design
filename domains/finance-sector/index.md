# Finance - Domain-Driven Design

Domain-Driven Design (DDD) for finance creates a modular and scalable system by modeling software directly after complex financial workflows using a shared "ubiquitous language" between developers and financial domain experts. It breaks the system into bounded contexts such as accounts and ledger, payments, lending, risk, trading, and customer onboarding to manage complexity.

## Bounded Contexts

- **[Accounts & Ledger Context](topics/accounts-ledger-context/)** - Chart of accounts, general ledger, journal entries, trial balance
- **[Payments & Transfers Context](topics/payments-transfers-context/)** - Payment processing, wire transfers, ACH, real-time payments
- **[Lending & Credit Context](topics/lending-credit-context/)** - Loan origination, underwriting, servicing, collections
- **[Risk & Compliance Context](topics/risk-compliance-context/)** - Risk assessment, KYC/AML, fraud detection, regulatory reporting
- **[Trading & Investments Context](topics/trading-investments-context/)** - Securities trading, portfolio management, market data, settlement
- **[Customer Onboarding Context](topics/customer-onboarding-context/)** - Account opening, identity verification, KYC, product enrollment

## Strategic Design

- **[Strategic Design](topics/strategic-design/)** - Principles for structuring the finance domain
- **[Bounded Contexts](topics/bounded-contexts/)** - Defining model boundaries
- **[Context Map](topics/context-map/)** - Relationships between bounded contexts
- **[Ubiquitous Language](topics/ubiquitous-language/)** - Shared vocabulary for developers and financial experts
- **[Subdomains](topics/subdomains/)** - Core, supporting, and generic subdomains
- **[Anti-Corruption Layer](topics/anti-corruption-layer/)** - Protecting domain models from external systems

## Tactical Design

- **[Aggregates](topics/aggregates/)** - Clusters of entities and value objects
- **[Entities](topics/entities/)** - Objects with unique identity
- **[Value Objects](topics/value-objects/)** - Immutable objects defined by attributes
- **[Domain Events](topics/domain-events/)** - Significant occurrences in the domain
- **[Repositories](topics/repositories/)** - Persistence abstraction for aggregate roots
- **[Domain Services](topics/domain-services/)** - Operations spanning multiple aggregates

## Cross-Cutting Concerns

- **[Event-Driven Architecture](topics/event-driven-architecture/)** - Event flows, sagas, choreography vs. orchestration
- **[Integration Patterns](topics/integration-patterns/)** - Shared kernel, published language, open host service
- **[Finance Standards](topics/finance-standards/)** - ISO 20022, SWIFT, FIX protocol, XBRL, PCI DSS, NACHA/ACH, FpML, IBAN/BIC
- **[Regulatory Compliance](topics/regulatory-compliance/)** - Basel III/IV, SOX, Dodd-Frank, PSD2, GDPR, KYC/AML, MiFID II, FATCA

## Key Files

- [Ubiquitous Language Glossary](ubiquitous-language.md) - Canonical list of domain terms
- [Project Plan](plan.md) - Implementation plan and phases
- [Tasks](tasks.md) - Task tracking checklist
- [All Topics](topics/) - Complete topic listing

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
- Vernon, Vaughn. "Domain-Driven Design Distilled." Addison-Wesley, 2016.
- Millett, Scott & Tune, Nick. "Patterns, Principles, and Practices of Domain-Driven Design." Wrox, 2015.
- ISO. "ISO 20022: Universal Financial Industry Message Scheme." https://www.iso20022.org/
- Basel Committee on Banking Supervision. "Basel III: International Regulatory Framework for Banks." BIS, 2017.
