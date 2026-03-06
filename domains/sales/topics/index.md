# Sales Domain - Topics

## Strategic Design

Strategic design addresses how to decompose the Sales domain into manageable bounded contexts, each with its own model and ubiquitous language.

- [Strategic Design](strategic-design/) - Overall strategic approach to modeling the Sales domain using DDD principles
- [Bounded Contexts](bounded-contexts/) - Defining logical boundaries for Lead Generation, Opportunity Management, Sales Pipeline, Order Management, Account Management, and Sales Analytics
- [Context Map](context-map/) - Mapping relationships and integration points between Sales bounded contexts
- [Ubiquitous Language](ubiquitous-language/) - Establishing shared terminology for leads, opportunities, orders, accounts, and related concepts
- [Subdomains](subdomains/) - Classifying Sales areas as core, supporting, or generic subdomains
- [Anti-Corruption Layer](anti-corruption-layer/) - Protecting Sales models from external system concepts during integration

## Tactical Design

Tactical design patterns structure business logic within each bounded context.

- [Aggregates](aggregates/) - Clusters of related objects such as Order with OrderLines, maintaining consistency within transactional boundaries
- [Entities](entities/) - Objects with unique identity and lifecycle such as SalesOrder, Contract, and Account
- [Value Objects](value-objects/) - Immutable descriptors such as Price, Currency, DiscountPercentage, and LeadScore
- [Domain Events](domain-events/) - Business occurrences such as LeadQualified, DealClosed, OrderPlaced, and ContractRenewed
- [Repositories](repositories/) - Abstractions for storing and retrieving aggregate roots such as OpportunityRepository and AccountRepository
- [Domain Services](domain-services/) - Stateless operations spanning multiple aggregates such as LeadScoringService and PipelineForecastingService

## Bounded Contexts

Detailed documentation for each of the six bounded contexts in the Sales domain.

- [Lead Generation Context](lead-generation-context/) - Prospect identification, marketing campaigns, lead scoring, and qualification
- [Opportunity Management Context](opportunity-management-context/) - Deal negotiation, proposal management, and win/loss tracking
- [Sales Pipeline Context](sales-pipeline-context/) - Stage progression, forecasting, and pipeline analytics
- [Order Management Context](order-management-context/) - Order creation, processing, and fulfillment handoff
- [Account Management Context](account-management-context/) - Customer accounts, relationships, and contract management
- [Sales Analytics Context](sales-analytics-context/) - Reporting, dashboards, performance metrics, and forecasting

## Cross-Cutting Concerns

Cross-cutting concerns address patterns and requirements that span multiple bounded contexts.

- [Event-Driven Architecture](event-driven-architecture/) - Communication between bounded contexts through domain events
- [Integration Patterns](integration-patterns/) - Shared kernel, published language, open host service, and customer-supplier patterns
- [Sales Standards](sales-standards/) - Industry standards such as CRM data models, sales methodology frameworks, and data interchange formats
- [Regulatory Compliance](regulatory-compliance/) - GDPR, CAN-SPAM, data protection, and financial reporting requirements

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." O'Reilly Media, 2021.
