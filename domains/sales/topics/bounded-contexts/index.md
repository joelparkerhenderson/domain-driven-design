# Bounded Contexts in the Sales Domain

## Overview

Bounded contexts define the logical boundaries within which a particular domain model is
consistent and meaningful. In the Sales domain, bounded contexts separate distinct areas of
sales operations so that each area maintains its own model, its own ubiquitous language, and
its own business rules without interference from other areas. A term like "Customer" means
different things in different contexts: in Lead Generation it is a Prospect, in Account
Management it is an Account, and in Order Management it is a Buyer.

The Sales domain is decomposed into six bounded contexts, each representing a coherent area
of business capability with well-defined responsibilities and boundaries.

## Key Concepts

**Model Integrity**: Each bounded context maintains its own internally consistent model. The
Lead Generation Context models prospects and campaigns, while the Opportunity Management
Context models deals and proposals. These models are deliberately separate to avoid
conceptual confusion and coupling.

**Linguistic Boundaries**: The ubiquitous language within each bounded context is precise and
unambiguous. "Qualification" in the Lead Generation Context means assessing lead readiness,
while in the Opportunity Management Context it may refer to qualifying a deal against
specific criteria such as MEDDIC.

**Context Ownership**: Each bounded context is owned by a specific team or group responsible
for its domain logic and evolution. The Lead Generation Context might be owned by the
marketing operations team, while the Opportunity Management Context is owned by the sales
operations team.

**Boundary Identification**: Boundaries are identified through analysis of the ubiquitous
language, organizational structure, and business process flows. When different stakeholders
use the same term with different meanings, that signals a context boundary.

## The Six Bounded Contexts

**Lead Generation Context**: Handles prospect identification, marketing campaign management,
lead scoring, and lead qualification. Inputs raw market interest and outputs qualified leads
ready for sales engagement. Key aggregates: Lead, Campaign.

**Opportunity Management Context**: Manages deal negotiation, proposal creation and tracking,
competitive analysis, and win/loss recording. This is the core bounded context where
revenue-generating activities are concentrated. Key aggregates: Opportunity, Proposal.

**Sales Pipeline Context**: Tracks the progression of opportunities through defined stages,
provides forecasting based on pipeline data, and delivers analytics on pipeline health,
velocity, and conversion rates. Key aggregates: PipelineView, Forecast.

**Order Management Context**: Creates orders from closed opportunities, manages order
processing workflows, and handles the fulfillment handoff to downstream systems. Key
aggregates: Order.

**Account Management Context**: Maintains customer account records, manages business
relationships and contacts, and handles the contract lifecycle including renewals, upsells,
and cross-sells. Key aggregates: Account, Contract.

**Sales Analytics Context**: Aggregates data from all other contexts to provide reporting,
dashboards, performance metrics, and predictive forecasting for sales leadership. Key
aggregates: Dashboard, Report.

## Bounded Context Interactions

Each bounded context communicates with others through well-defined integration points.
The Lead Generation Context publishes LeadQualified events consumed by the Opportunity
Management Context. The Opportunity Management Context publishes DealClosedWon events
consumed by Order Management, Account Management, and Sales Analytics. These interactions
are documented in the context map and implemented through the event-driven architecture.

## Relationship to Other Topics

- [Strategic Design](../strategic-design/) provides the framework for identifying bounded contexts.
- [Context Map](../context-map/) documents how these six bounded contexts interact.
- [Ubiquitous Language](../ubiquitous-language/) ensures terminology is precise within each context.
- [Subdomains](../subdomains/) classifies each context area by strategic importance.
- [Aggregates](../aggregates/) define consistency boundaries within each bounded context.
- [Integration Patterns](../integration-patterns/) define the relationship types between contexts.

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software."
  Addison-Wesley, 2003. Chapter 14: Maintaining Model Integrity.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
  Chapter 2: Domains, Subdomains, and Bounded Contexts.
- Khononov, Vlad. "Learning Domain-Driven Design." O'Reilly Media, 2021.
  Chapter 4: Bounded Contexts.
- Fowler, Martin. "BoundedContext." martinfowler.com, 2014.
