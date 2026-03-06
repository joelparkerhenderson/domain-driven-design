# Domain Events in the Sales Domain

## Overview

Domain events represent significant occurrences within the domain that domain experts care
about. In the Sales domain, events capture the key moments in the sales lifecycle: a lead
being qualified, a deal being closed, an order being placed, or a contract being renewed.
Domain events enable bounded contexts to communicate without direct coupling, allowing each
context to react to changes in other contexts while maintaining its own model integrity.

Domain events are central to the Sales domain's architecture because the sales process is
inherently event-driven. Each stage transition, status change, and milestone completion is a
meaningful business event that may trigger actions in one or more bounded contexts. The
LeadQualified event, for example, triggers opportunity creation in the Opportunity Management
Context and updates metrics in the Sales Analytics Context.

## Key Concepts

**Event Naming**: Domain events are named in the past tense using ubiquitous language,
reflecting something that has already happened. LeadCaptured, OpportunityCreated, ProposalSent,
DealClosedWon, OrderPlaced, and ContractRenewed are examples. The past tense emphasizes that
the event is an immutable fact that cannot be undone.

**Event Payload**: Each event carries the data necessary for consumers to react appropriately.
A DealClosedWon event includes the opportunity identifier, account identifier, final deal
value, closing date, and the sales representative identifier. The payload should contain
enough information for consumers to process the event without needing to query the source
context.

**Event Ordering and Idempotency**: In the Sales domain, event ordering matters. A
DealClosedWon event must logically follow an OpportunityCreated event for the same
opportunity. Consumers must handle duplicate events gracefully through idempotent processing
to ensure correctness under retry scenarios.

**Event Sourcing**: Some Sales domain contexts benefit from event sourcing, where the state
of an aggregate is reconstructed from its event history. The Opportunity aggregate's full
history of stage changes, proposal submissions, and negotiations can be derived from its
events, providing a complete audit trail.

**Event Versioning**: As the Sales domain evolves, event schemas may change. Event versioning
strategies (such as additive-only changes or explicit version numbers) ensure backward
compatibility so that existing consumers continue to function as new event fields are added.

## Sales Domain Events

**Lead Generation Events**: LeadCaptured, LeadScored, LeadQualified, LeadDisqualified,
CampaignLaunched, CampaignCompleted.

**Opportunity Management Events**: OpportunityCreated, OpportunityStageChanged,
ProposalCreated, ProposalSent, ProposalAccepted, ProposalRejected, DealClosedWon,
DealClosedLost, CompetitorIdentified.

**Sales Pipeline Events**: PipelineStageUpdated, ForecastGenerated,
PipelineReviewCompleted, VelocityAlertTriggered.

**Order Management Events**: OrderCreated, OrderLineAdded, OrderSubmitted, OrderValidated,
OrderApproved, OrderConfirmed, FulfillmentHandoffRequested, FulfillmentHandoffCompleted.

**Account Management Events**: AccountCreated, ContactAdded, ContactRoleChanged,
ContractCreated, ContractSigned, ContractRenewed, ContractExpired, UpsellIdentified,
CrossSellIdentified, ChurnRiskDetected.

**Sales Analytics Events**: ReportGenerated, DashboardRefreshed,
QuotaAttainmentCalculated, PerformanceReviewCompleted, ForecastAccuracyAssessed.

## Event Flow Across Contexts

Events flow through the Sales domain following the natural business process. LeadQualified
flows from Lead Generation to Opportunity Management. OpportunityStageChanged flows from
Opportunity Management to Sales Pipeline. DealClosedWon flows from Opportunity Management
to Order Management, Account Management, and Sales Analytics. ContractRenewed flows from
Account Management to Sales Analytics. This event-driven flow enables each context to
operate independently while maintaining system-wide coherence.

## Relationship to Other Topics

- [Event-Driven Architecture](../event-driven-architecture/) defines the infrastructure for events.
- [Aggregates](../aggregates/) emit domain events when their state changes.
- [Entities](../entities/) undergo state transitions that produce domain events.
- [Integration Patterns](../integration-patterns/) use domain events for inter-context communication.
- [Context Map](../context-map/) shows the event flows between bounded contexts.
- [Repositories](../repositories/) may store domain events alongside aggregate state.

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software."
  Addison-Wesley, 2003. (Domain events were later formalized in the DDD community.)
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
  Chapter 8: Domain Events.
- Khononov, Vlad. "Learning Domain-Driven Design." O'Reilly Media, 2021.
  Chapter 9: Communication Patterns.
- Young, Greg. "CQRS and Event Sourcing." CQRS.nu, 2010.
- Brandolini, Alberto. "Introducing EventStorming." Leanpub, 2021.
