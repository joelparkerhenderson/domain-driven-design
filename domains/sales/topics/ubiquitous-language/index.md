# Ubiquitous Language in the Sales Domain

## Overview

Ubiquitous language is the shared vocabulary that domain experts, developers, and stakeholders
use consistently within a bounded context. In the Sales domain, ubiquitous language bridges
the gap between sales professionals who think in terms of leads, deals, and quotas and the
software teams who model these concepts. Every conversation, document, and line of code within
a bounded context uses the same precise terms, reducing misunderstanding and ensuring the
software model faithfully represents the business reality.

The Sales domain has particularly rich terminology that varies by context. A "customer" in
Lead Generation is a Prospect, while in Account Management it is an Account with contracts
and revenue history. Ubiquitous language makes these distinctions explicit rather than
allowing ambiguity to create confusion across teams.

## Key Concepts

**Context-Specific Definitions**: Each bounded context defines its own precise vocabulary.
The Lead Generation Context uses terms like Lead, Prospect, Lead Score, and Qualification.
The Opportunity Management Context uses Deal, Proposal, Win, and Loss. These terms have
specific, agreed-upon meanings within their respective contexts.

**Language in Code**: The ubiquitous language appears directly in code artifacts. Classes,
methods, variables, and domain events use the same terms that sales managers and
representatives use daily. A LeadQualified event in code corresponds exactly to the
business process of qualifying a lead.

**Language Evolution**: As the business understanding deepens, the ubiquitous language evolves.
New terms may emerge (such as Sales Velocity or Customer Lifetime Value) and existing terms
may be refined. The language and the model evolve together through ongoing collaboration
between domain experts and developers.

**Glossary Maintenance**: The Sales domain maintains a formal glossary (see the domain-level
ubiquitous-language.md) that documents each term with a precise definition. This glossary
serves as the authoritative reference for all stakeholders.

## Sales Domain Terminology Examples

**Lead vs. Prospect vs. Opportunity**: A Lead is an unqualified contact. A Prospect is a
lead that meets qualification criteria. An Opportunity is a prospect with a specific deal
in the pipeline. These three terms represent distinct stages in the sales lifecycle and
must not be used interchangeably.

**Pipeline Stage**: A defined phase such as Discovery, Proposal, Negotiation, or Closed-Won.
Each stage has entry and exit criteria that govern opportunity progression.

**Quota vs. Forecast**: A Quota is a target assigned to a representative. A Forecast is a
prediction of actual revenue based on pipeline data. These terms are often confused but
represent fundamentally different concepts.

**Upsell vs. Cross-sell**: An Upsell offers a higher-tier product to an existing customer.
A Cross-sell offers a complementary product. Both occur within the Account Management
Context as expansion selling activities.

**Fulfillment Handoff**: The specific event when a confirmed order transitions from the Sales
domain to the Fulfillment domain. This term precisely captures the domain boundary crossing.

**Win Rate**: The percentage of closed opportunities that result in a sale. Distinct from
Conversion Rate, which measures progression between any two stages.

**Sales Cycle**: The average elapsed time from initial lead capture to deal closure. This
metric spans multiple bounded contexts and is tracked in the Sales Analytics Context.

## Relationship to Other Topics

- [Strategic Design](../strategic-design/) relies on ubiquitous language to define context boundaries.
- [Bounded Contexts](../bounded-contexts/) each maintain their own dialect of the ubiquitous language.
- [Entities](../entities/) and [Value Objects](../value-objects/) are named using ubiquitous language.
- [Domain Events](../domain-events/) use ubiquitous language to name significant occurrences.
- [Sales Standards](../sales-standards/) may introduce industry-standard terminology.

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software."
  Addison-Wesley, 2003. Chapter 2: Communication and the Use of Language.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
  Chapter 1: Getting Started with DDD.
- Khononov, Vlad. "Learning Domain-Driven Design." O'Reilly Media, 2021.
  Chapter 2: Discovering Domain Knowledge.
- Avram, Abel and Marinescu, Floyd. "Domain-Driven Design Quickly." InfoQ, 2007.
