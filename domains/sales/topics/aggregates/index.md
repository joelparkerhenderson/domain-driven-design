# Aggregates in the Sales Domain

## Overview

Aggregates are clusters of related domain objects treated as a single unit for the purpose of
data consistency. Each aggregate has a root entity that controls access to all objects within
the aggregate and enforces business invariants across them. In the Sales domain, aggregates
define the transactional boundaries within each bounded context, ensuring that operations like
creating an order with multiple line items, updating an opportunity's stage, or modifying a
contract's terms maintain consistency without requiring system-wide locks.

Aggregate design is one of the most consequential tactical design decisions in the Sales
domain. Poorly designed aggregates lead to contention, performance issues, and convoluted
business logic. Well-designed aggregates encapsulate cohesive business rules and enable
scalable, maintainable systems.

## Key Concepts

**Aggregate Root**: The single entry point for all operations on the aggregate. In the Sales
domain, the Order entity is the aggregate root for the Order aggregate. All modifications to
order lines, discounts, and shipping details must go through the Order root to ensure the
total price, tax calculations, and fulfillment requirements remain consistent.

**Consistency Boundary**: An aggregate defines a transactional consistency boundary. Within the
boundary, all invariants are enforced immediately. Across aggregate boundaries, consistency
is eventual. For example, when a deal is closed in the Opportunity Management Context, the
creation of the corresponding order in the Order Management Context is eventually consistent
via domain events.

**Small Aggregates**: Following Vernon's guidance, aggregates should be kept as small as
possible while still enforcing the required business invariants. A common mistake in the
Sales domain is creating a large Account aggregate that includes all contacts, contracts,
opportunities, and orders. Instead, Account, Contract, and Opportunity should be separate
aggregates linked by identity references.

**Reference by Identity**: Aggregates reference other aggregates by their identity (such as
AccountId or OpportunityId) rather than by direct object references. This maintains loose
coupling and enables independent scaling of different aggregate types.

## Sales Domain Aggregates

**Lead Aggregate** (Lead Generation Context): Root entity is Lead. Contains the lead's
contact information, source attribution, lead score, and qualification status. Invariant:
a lead's score must be recalculated whenever engagement data changes.

**Campaign Aggregate** (Lead Generation Context): Root entity is Campaign. Contains campaign
definition, target criteria, budget allocation, and performance metrics. Invariant: campaign
spend must not exceed the allocated budget.

**Opportunity Aggregate** (Opportunity Management Context): Root entity is Opportunity.
Contains proposal references, competitor entries, and stage history. Invariant: stage
transitions must follow defined rules (no skipping stages, no backward movement without
explicit loss recording).

**Proposal Aggregate** (Opportunity Management Context): Root entity is Proposal. Contains
line items, pricing, terms, and approval status. Invariant: the proposal total must equal
the sum of its line item values minus any applicable discounts.

**Order Aggregate** (Order Management Context): Root entity is Order. Contains OrderLine
entities and a ShippingAddress value object. Invariant: order total must always equal the
sum of line item subtotals minus discounts plus taxes.

**Account Aggregate** (Account Management Context): Root entity is Account. Contains contact
references and account classification. Invariant: an account must always have at least one
primary contact.

**Contract Aggregate** (Account Management Context): Root entity is Contract. Contains
contract terms, renewal dates, and pricing schedules. Invariant: contract end date must be
after start date, and renewal terms must be set before the renewal window opens.

## Aggregate Design Guidelines

When designing aggregates in the Sales domain, favor smaller aggregates that protect only
the invariants that must be immediately consistent. Use domain events to synchronize state
across aggregates. Design aggregate boundaries to minimize contention in concurrent access
scenarios, which is important when multiple sales representatives work on related deals
simultaneously.

## Relationship to Other Topics

- [Entities](../entities/) are the identity-bearing objects within aggregates.
- [Value Objects](../value-objects/) provide immutable attributes within aggregates.
- [Repositories](../repositories/) persist and retrieve aggregate roots.
- [Domain Events](../domain-events/) communicate changes across aggregate boundaries.
- [Domain Services](../domain-services/) coordinate operations spanning multiple aggregates.
- [Bounded Contexts](../bounded-contexts/) contain one or more aggregates.

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software."
  Addison-Wesley, 2003. Chapter 6: The Life Cycle of a Domain Object.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
  Chapter 10: Aggregates.
- Vernon, Vaughn. "Effective Aggregate Design." DDD Community, 2011.
  Three-part article series on aggregate sizing.
- Khononov, Vlad. "Learning Domain-Driven Design." O'Reilly Media, 2021.
  Chapter 8: Architectural Patterns.
