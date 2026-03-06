# Enterprise Resource Planning DDD - Topics

## Strategic Design

- [Strategic Design](strategic-design/) - Strategic design principles applied to ERP
- [Bounded Contexts](bounded-contexts/) - The six bounded contexts and their boundaries
- [Context Map](context-map/) - Relationships between bounded contexts
- [Ubiquitous Language](ubiquitous-language/) - Shared vocabulary between developers and domain experts
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

- [Finance/Accounting Context](finance-accounting-context/) - General ledger, AP/AR, financial reporting
- [Procurement Context](procurement-context/) - Purchase orders, supplier management, sourcing
- [Inventory/Warehouse Context](inventory-warehouse-context/) - Stock management, warehouse operations, logistics
- [Sales/Order Context](sales-order-context/) - Sales orders, customer management, pricing, fulfillment
- [Human Resources Context](human-resources-context/) - Employee management, payroll, benefits, recruitment
- [Manufacturing Context](manufacturing-context/) - Production planning, BOM, work orders, quality control

## Cross-Cutting Concerns

- [Event-Driven Architecture](event-driven-architecture/) - Event flows, sagas, choreography vs. orchestration
- [Integration Patterns](integration-patterns/) - Shared kernel, published language, open host service
- [ERP Standards](erp-standards/) - EDI, XBRL, UBL, ISO 20022
- [Regulatory Compliance](regulatory-compliance/) - SOX, GAAP/IFRS, GDPR, tax regulations
