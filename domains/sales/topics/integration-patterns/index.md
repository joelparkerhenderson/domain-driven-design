# Integration Patterns in the Sales Domain

## Overview

Integration patterns define how bounded contexts within the Sales domain interact with each
other and with external systems. Domain-Driven Design provides a vocabulary of relationship
patterns that characterize the nature of each integration: who controls the interface, how
models are translated, and what degree of coupling exists. In the Sales domain, integration
patterns govern how lead data flows into opportunity management, how closed deals become
orders, how account data is shared across contexts, and how external CRM, ERP, and marketing
platforms connect to the domain.

Choosing the right integration pattern for each relationship is a strategic design decision
that affects team autonomy, system coupling, and the ability to evolve bounded contexts
independently.

## Key Concepts

**Customer-Supplier**: An upstream context (supplier) provides data or services that a
downstream context (customer) depends on. The supplier accommodates the customer's needs
within reason. In the Sales domain, Opportunity Management (supplier) provides closed deal
data to Order Management (customer). The Opportunity Management team ensures that
DealClosedWon events contain the information Order Management requires.

**Conformist**: A downstream context conforms to the upstream context's model without
expecting accommodations. In the Sales domain, Sales Analytics is a conformist to all
other contexts. It adapts its internal data structures to match the event schemas published
by Lead Generation, Opportunity Management, Sales Pipeline, Order Management, and Account
Management.

**Shared Kernel**: Two bounded contexts share a small, explicitly defined subset of the
domain model. Changes to the shared kernel require agreement from both teams. In the Sales
domain, Opportunity Management and Sales Pipeline share pipeline stage definitions and
probability weightings, jointly maintaining this shared model.

**Published Language**: A well-documented, versioned language (often expressed as event
schemas or API contracts) that bounded contexts use to communicate. All Sales domain
events follow a published language that defines event structure, field semantics, versioning
rules, and compatibility guarantees.

**Open Host Service**: A bounded context exposes a well-defined protocol or API for other
contexts to integrate with. The Account Management Context may provide an open host service
that allows other contexts to query account information through a stable, documented
interface.

**Anti-Corruption Layer**: A translation layer that protects a bounded context from foreign
models. Used when integrating with external systems (CRM platforms, ERP systems) that have
different models and terminology than the Sales domain.

**Partnership**: Two bounded contexts coordinate closely, with teams jointly planning
integration changes. In the Sales domain, Lead Generation and Opportunity Management
operate as partners, coordinating on lead qualification criteria and handoff processes.

## Sales Domain Integration Map

- Lead Generation to Opportunity Management: Partnership with published language
  (LeadQualified event schema).

- Opportunity Management to Sales Pipeline: Shared kernel for stage definitions.

- Opportunity Management to Order Management: Customer-supplier with published language.

- Opportunity Management to Account Management: Customer-supplier with published language.

- All contexts to Sales Analytics: Conformist with published language.

- External CRM to Sales domain: Anti-corruption layer.

- Sales domain to external ERP: Anti-corruption layer.

- Marketing platform to Lead Generation: Anti-corruption layer with data enrichment
  translation.

## Relationship to Other Topics

- [Context Map](../context-map/) visualizes the integration patterns between bounded contexts.
- [Anti-Corruption Layer](../anti-corruption-layer/) implements translation boundaries.
- [Event-Driven Architecture](../event-driven-architecture/) provides the communication mechanism.
- [Domain Events](../domain-events/) define the messages exchanged through integration patterns.
- [Bounded Contexts](../bounded-contexts/) are the participants in each integration.
- [Strategic Design](../strategic-design/) governs the choice of integration patterns.

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software."
  Addison-Wesley, 2003. Chapter 14: Maintaining Model Integrity.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
  Chapter 3: Context Maps.
- Khononov, Vlad. "Learning Domain-Driven Design." O'Reilly Media, 2021.
  Chapter 5: Context Mapping.
- Hohpe, Gregor and Woolf, Bobby. "Enterprise Integration Patterns."
  Addison-Wesley, 2003.
- Newman, Sam. "Building Microservices." O'Reilly Media, 2021.
