# Repositories in the Sales Domain

## Overview

Repositories provide abstractions for storing and retrieving aggregate roots, decoupling the
domain model from persistence infrastructure. In the Sales domain, repositories enable the
business logic to focus on sales operations without concerning itself with database
technologies, query languages, or storage mechanisms. Each aggregate root in the Sales domain
has a corresponding repository that provides methods for finding, saving, and removing
aggregates using domain-meaningful queries.

Repositories are a critical tactical pattern because they maintain the purity of the domain
model. A LeadRepository, for example, provides methods like findByScore or
findQualifiedLeads that express queries in the ubiquitous language of the Lead Generation
Context, hiding the underlying storage implementation entirely.

## Key Concepts

**Collection-Oriented vs. Persistence-Oriented**: Collection-oriented repositories behave
like in-memory collections, tracking changes automatically and persisting them when the unit
of work completes. Persistence-oriented repositories require explicit save calls. The Sales
domain can use either approach, with the choice driven by infrastructure considerations
rather than domain logic.

**Domain-Centric Query Methods**: Repository methods are named using the ubiquitous language.
Rather than generic findById or findAll methods, Sales domain repositories expose
business-meaningful queries: findOpenOpportunitiesByRepresentative,
findContractsExpiringWithin, findLeadsAboveScoreThreshold. These methods encode common
access patterns in the domain's vocabulary.

**Aggregate Root Access Only**: Repositories only provide access to aggregate roots, never
to internal entities or value objects within an aggregate. To access an OrderLine, you
retrieve the Order aggregate root through the OrderRepository and navigate to the line
item through the aggregate's interface.

**Specification Pattern**: For complex queries in the Sales domain, the specification pattern
allows composable query criteria. A pipeline query might combine specifications for stage,
representative, date range, and minimum deal value without requiring a dedicated repository
method for each combination.

**No Business Logic**: Repositories contain no domain logic. They are purely responsible for
persistence and retrieval. Business rules belong in aggregates, entities, value objects,
and domain services, not in repositories.

## Sales Domain Repositories

**LeadRepository**: Provides access to Lead aggregates. Key queries include finding leads
by score threshold, by source campaign, by assignment status, and by qualification state.

**CampaignRepository**: Provides access to Campaign aggregates. Key queries include finding
campaigns by status (active, completed, scheduled), by date range, and by performance
metrics.

**OpportunityRepository**: Provides access to Opportunity aggregates. Key queries include
finding opportunities by stage, by assigned representative, by close date range, by
account, and by probability threshold.

**ProposalRepository**: Provides access to Proposal aggregates. Key queries include finding
proposals by opportunity, by status, by expiration date, and by approval state.

**OrderRepository**: Provides access to Order aggregates. Key queries include finding orders
by status, by account, by date range, and by fulfillment state.

**AccountRepository**: Provides access to Account aggregates. Key queries include finding
accounts by territory, by industry classification, by lifetime value tier, and by assigned
representative.

**ContractRepository**: Provides access to Contract aggregates. Key queries include finding
contracts by expiration date range, by renewal status, by account, and by value.

## Relationship to Other Topics

- [Aggregates](../aggregates/) are the objects that repositories persist and retrieve.
- [Entities](../entities/) serve as aggregate roots accessed through repositories.
- [Domain Services](../domain-services/) use repositories to access needed aggregates.
- [Domain Events](../domain-events/) may be stored alongside aggregates in event-sourced systems.
- [Bounded Contexts](../bounded-contexts/) each define their own repositories.
- [Value Objects](../value-objects/) are persisted as part of their containing aggregate.

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software."
  Addison-Wesley, 2003. Chapter 6: The Life Cycle of a Domain Object.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
  Chapter 12: Repositories.
- Khononov, Vlad. "Learning Domain-Driven Design." O'Reilly Media, 2021.
  Chapter 6: Tactical Design.
- Fowler, Martin. "Repository." Patterns of Enterprise Application Architecture.
  Addison-Wesley, 2002.
