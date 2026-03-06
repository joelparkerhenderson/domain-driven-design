# Entities in the Sales Domain

## Overview

Entities are domain objects defined by their unique identity rather than their attributes.
An entity maintains continuity through its lifecycle even as its properties change over time.
In the Sales domain, entities represent the core business objects that sales teams interact
with daily: leads that progress through qualification, opportunities that move through pipeline
stages, orders that are processed, and accounts that span years of business relationships.
Each entity has a distinct identity that distinguishes it from all other instances, regardless
of attribute values.

Entity design in the Sales domain requires careful consideration of identity generation,
lifecycle management, and the business rules that govern state transitions. A SalesOrder
entity, for example, has a unique order number, progresses through states from draft to
confirmed to fulfilled, and must enforce rules about what modifications are allowed at
each state.

## Key Concepts

**Identity**: Every entity has a unique identifier that persists throughout its lifecycle. In
the Sales domain, a Lead might be identified by a system-generated LeadId, an Opportunity by
an OpportunityId, and an Order by an OrderNumber. Identity is assigned at creation and never
changes, even if every other attribute is modified.

**Lifecycle Management**: Entities have creation, modification, and potentially archival or
deletion phases. A Lead entity is created when a prospect first engages, modified as scoring
data accumulates, and transitions out of the Lead entity lifecycle when it is qualified into
an Opportunity or disqualified.

**State Transitions**: Many Sales entities have well-defined state machines. An Opportunity
progresses through states such as Prospecting, Discovery, Proposal, Negotiation, Closed-Won,
and Closed-Lost. The entity enforces rules about valid transitions and the data required at
each state.

**Equality by Identity**: Two entities are equal if and only if they have the same identity.
Two Opportunity entities with identical deal values, close dates, and assigned representatives
are still distinct entities if they have different OpportunityIds.

## Sales Domain Entities

**Lead**: Identified by LeadId. Tracks a potential customer from initial capture through
qualification. Attributes include contact details, source, engagement history, and lead
score. State transitions: New, Contacted, Engaged, Qualified, Disqualified.

**Opportunity**: Identified by OpportunityId. Represents a qualified deal in progress.
Attributes include expected revenue, probability, close date, assigned representative, and
associated account. State transitions follow the pipeline stage definitions.

**Proposal**: Identified by ProposalId. A formal offer associated with an opportunity.
Attributes include line items, pricing, terms, validity period, and approval status.
State transitions: Draft, Submitted, Approved, Sent, Accepted, Rejected, Expired.

**Order**: Identified by OrderNumber. A confirmed purchase commitment from a customer.
Attributes include order lines, billing and shipping details, payment terms, and order
status. State transitions: Draft, Submitted, Validated, Approved, Confirmed, Fulfilled.

**Account**: Identified by AccountId. A business entity with an ongoing relationship.
Attributes include company details, industry classification, territory assignment, and
lifetime value. Accounts persist indefinitely with no terminal state.

**Contact**: Identified by ContactId. An individual person associated with one or more
accounts. Attributes include name, role, communication preferences, and decision-making
authority level.

**Contract**: Identified by ContractId. A binding agreement with an account. Attributes
include start date, end date, terms, pricing schedule, and renewal status. State
transitions: Draft, Negotiation, Executed, Active, Renewed, Expired, Terminated.

**Sales Representative**: Identified by RepresentativeId. A member of the sales team with
assigned territory, quota, and pipeline responsibilities.

## Relationship to Other Topics

- [Aggregates](../aggregates/) group entities and value objects into consistency boundaries.
- [Value Objects](../value-objects/) provide immutable attributes within entities.
- [Repositories](../repositories/) handle persistence and retrieval of entity aggregate roots.
- [Domain Events](../domain-events/) signal significant entity state changes.
- [Ubiquitous Language](../ubiquitous-language/) provides the naming conventions for entities.
- [Domain Services](../domain-services/) operate across multiple entities.

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software."
  Addison-Wesley, 2003. Chapter 5: A Model Expressed in Software.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
  Chapter 5: Entities.
- Khononov, Vlad. "Learning Domain-Driven Design." O'Reilly Media, 2021.
  Chapter 6: Tactical Design.
- Nilsson, Jimmy. "Applying Domain-Driven Design and Patterns."
  Addison-Wesley, 2006.
