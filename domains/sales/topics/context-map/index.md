# Context Map in the Sales Domain

## Overview

A context map is a visual and conceptual representation of the relationships and interactions
between bounded contexts. In the Sales domain, the context map documents how the six bounded
contexts (Lead Generation, Opportunity Management, Sales Pipeline, Order Management, Account
Management, and Sales Analytics) communicate, share data, and depend on each other. The context
map makes integration points explicit and helps teams coordinate their work across context
boundaries.

The context map is a strategic design artifact that captures the reality of how different parts
of the Sales system interact, including the politics, dependencies, and translation mechanisms
between teams and their models.

## Key Concepts

**Upstream/Downstream Relationships**: In the Sales domain, Lead Generation is upstream of
Opportunity Management because it produces qualified leads that flow into the sales pipeline.
Order Management is downstream of Opportunity Management because it receives closed deals
for order processing.

**Relationship Patterns**: The context map uses established DDD relationship patterns to
characterize each integration:

- **Customer-Supplier**: Opportunity Management (supplier) provides closed deal data to Order
  Management (customer). The supplier commits to meeting the customer's needs.

- **Conformist**: Sales Analytics conforms to the models of all other contexts, adapting its
  internal representations to match the events and data structures published by upstream
  contexts.

- **Shared Kernel**: Opportunity Management and Sales Pipeline share a common model for
  opportunity stage definitions, jointly maintained by both teams.

- **Published Language**: All contexts publish domain events using a shared event schema,
  enabling loose coupling through a well-documented interchange format.

- **Partnership**: Lead Generation and Opportunity Management operate as partners, jointly
  defining lead qualification criteria and handoff processes.

- **Anti-Corruption Layer**: External CRM or ERP systems are integrated through
  anti-corruption layers that translate foreign concepts into the Sales domain's
  ubiquitous language.

## Integration Points in the Sales Domain

**Lead Generation to Opportunity Management**: The LeadQualified event triggers opportunity
creation. This is a partnership relationship where both teams coordinate on qualification
criteria.

**Opportunity Management to Sales Pipeline**: Opportunity stage changes update pipeline
tracking. A shared kernel maintains stage definitions and probability weightings.

**Opportunity Management to Order Management**: The DealClosedWon event initiates order
creation. This is a customer-supplier relationship where Opportunity Management provides
the data Order Management needs.

**Opportunity Management to Account Management**: New deal associations update account
records. Deal closure updates account revenue and relationship data.

**All Contexts to Sales Analytics**: Domain events flow from every context to the analytics
context for aggregation. Sales Analytics acts as a conformist, adapting to each upstream
context's event schemas.

**External Systems**: CRM platforms, marketing automation tools, and ERP systems connect
through anti-corruption layers that protect the Sales domain's model integrity.

## Relationship to Other Topics

- [Bounded Contexts](../bounded-contexts/) define the individual contexts shown on the map.
- [Strategic Design](../strategic-design/) establishes the framework for context mapping.
- [Integration Patterns](../integration-patterns/) detail the specific integration mechanisms.
- [Anti-Corruption Layer](../anti-corruption-layer/) implements translation between contexts.
- [Event-Driven Architecture](../event-driven-architecture/) enables event-based communication.
- [Domain Events](../domain-events/) define the specific events flowing between contexts.

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software."
  Addison-Wesley, 2003. Chapter 14: Maintaining Model Integrity.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
  Chapter 3: Context Maps.
- Khononov, Vlad. "Learning Domain-Driven Design." O'Reilly Media, 2021.
  Chapter 5: Context Mapping.
- Brandolini, Alberto. "Introducing EventStorming." Leanpub, 2021.
