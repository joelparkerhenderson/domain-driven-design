# Opportunity Management Context

## Overview

The Opportunity Management Context is the core bounded context of the Sales domain,
responsible for deal negotiation, proposal management, competitive analysis, and win/loss
tracking. This context manages the high-value activities where sales representatives engage
with qualified prospects to structure, negotiate, and close deals. As the core subdomain,
Opportunity Management is where the organization's competitive differentiation in sales
execution is most pronounced, warranting the deepest investment in domain modeling.

This context receives qualified leads from the Lead Generation Context and produces closed
deals that flow to the Order Management Context and Account Management Context. The business
rules governing deal progression, discount authorization, competitive positioning, and
approval workflows are central to the organization's revenue generation capability.

## Key Concepts

**Opportunity Lifecycle**: An opportunity progresses through defined stages from creation to
closure. Typical stages include Prospecting, Discovery, Proposal, Negotiation, Closed-Won,
and Closed-Lost. Each stage has entry criteria, required activities, and exit conditions
that enforce sales process discipline.

**Proposal Management**: The creation, revision, and tracking of formal proposals presented
to prospects. A proposal includes product or service line items, pricing, discount
structures, terms and conditions, and a validity period. Proposals may go through internal
approval workflows before being sent to the prospect. Multiple proposal revisions may
occur during negotiation.

**Deal Negotiation**: The interactive process of refining terms between the sales
representative and the prospect. Negotiations may involve multiple rounds of proposal
revision, competitive price matching, scope adjustments, and executive-level engagement.
Discount authorization rules govern how much flexibility representatives have.

**Win/Loss Analysis**: The systematic recording and analysis of deal outcomes. Won deals
capture the success factors and competitive advantages. Lost deals record the reasons for
loss (price, product fit, competition, timing, no decision) to inform future strategy and
product development.

**Competitive Tracking**: Recording which competitors are involved in a deal, their
positioning, and strengths and weaknesses relative to the company's offering. This
intelligence feeds into sales strategy, battlecard development, and product roadmap
decisions.

**Discount Authorization**: Rules governing the discount levels that sales representatives
can offer independently versus those requiring management approval. Discount thresholds
may vary by deal size, product line, and customer tier.

## Aggregates and Entities

- **Opportunity** (Aggregate Root): Contains stage, expected revenue, probability, close
  date, assigned representative, and competitors. Enforces stage transition rules.

- **Proposal** (Aggregate Root): Contains line items, pricing, terms, validity period, and
  approval status. Enforces pricing and discount invariants.

- **Money** (Value Object): Deal value with currency.

- **StageTransition** (Value Object): Records a stage change with timestamp and reason.

- **CompetitorEntry** (Value Object): Records a competitor's involvement, positioning, and
  threat assessment for a specific opportunity.

## Domain Events

- OpportunityCreated: A new opportunity has been established from a qualified lead.
- OpportunityStageChanged: An opportunity has moved to a new pipeline stage.
- ProposalCreated: A formal proposal has been drafted for an opportunity.
- ProposalSent: A proposal has been delivered to the prospect.
- ProposalAccepted: The prospect has accepted the proposal terms.
- ProposalRejected: The prospect has declined the proposal.
- DealClosedWon: The opportunity has been successfully closed with a commitment to purchase.
- DealClosedLost: The opportunity has been closed without a purchase.
- CompetitorIdentified: A competitor has been identified in a deal.

## Context Relationships

- **Upstream from Lead Generation**: Receives LeadQualified events to create new
  opportunities from qualified leads.

- **Downstream to Order Management**: DealClosedWon events trigger order creation in the
  Order Management Context.

- **Downstream to Account Management**: Deal closure updates account records with new
  revenue and relationship data.

- **Downstream to Sales Pipeline**: Stage changes update the Sales Pipeline Context for
  tracking and forecasting.

- **Downstream to Sales Analytics**: All opportunity events flow to analytics for
  performance reporting.

- **Shared Kernel with Sales Pipeline**: Stage definitions and probability weightings are
  jointly maintained.

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software."
  Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." O'Reilly Media, 2021.
- Rackham, Neil. "SPIN Selling." McGraw-Hill, 1988.
- Thull, Jeff. "Mastering the Complex Sale." Wiley, 2010.
