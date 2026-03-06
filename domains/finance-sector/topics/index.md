# Finance DDD - Topics

## Strategic Design

- [Strategic Design](strategic-design/) - Strategic design principles applied to finance
- [Bounded Contexts](bounded-contexts/) - The six bounded contexts and their boundaries
- [Context Map](context-map/) - Relationships between bounded contexts
- [Ubiquitous Language](ubiquitous-language/) - Shared vocabulary for developers and financial experts
- [Subdomains](subdomains/) - Core, supporting, and generic subdomains
- [Anti-Corruption Layer](anti-corruption-layer/) - Protecting domain models from external systems

## Tactical Design

- [Aggregates](aggregates/) - Clusters of entities and value objects
- [Entities](entities/) - Objects with unique identity
- [Value Objects](value-objects/) - Immutable objects defined by attributes
- [Domain Events](domain-events/) - Significant occurrences in the domain
- [Repositories](repositories/) - Persistence abstraction for aggregate roots
- [Domain Services](domain-services/) - Operations spanning multiple aggregates

## Bounded Contexts

- [Accounts & Ledger Context](accounts-ledger-context/) - Chart of accounts, general ledger, journal entries, trial balance
- [Payments & Transfers Context](payments-transfers-context/) - Payment processing, wire transfers, ACH, real-time payments
- [Lending & Credit Context](lending-credit-context/) - Loan origination, underwriting, servicing, collections
- [Risk & Compliance Context](risk-compliance-context/) - Risk assessment, KYC/AML, fraud detection, regulatory reporting
- [Trading & Investments Context](trading-investments-context/) - Securities trading, portfolio management, market data, settlement
- [Customer Onboarding Context](customer-onboarding-context/) - Account opening, identity verification, KYC, product enrollment

## Cross-Cutting Concerns

- [Event-Driven Architecture](event-driven-architecture/) - Event flows, sagas, choreography vs. orchestration
- [Integration Patterns](integration-patterns/) - Shared kernel, published language, open host service
- [Finance Standards](finance-standards/) - ISO 20022, SWIFT, FIX protocol, XBRL, PCI DSS, NACHA/ACH, FpML, IBAN/BIC
- [Regulatory Compliance](regulatory-compliance/) - Basel III/IV, SOX, Dodd-Frank, PSD2, GDPR, KYC/AML, MiFID II, FATCA
