# Account Management Context

## Overview

The Account Management Context is responsible for managing customer accounts, business
relationships, contact information, and the contract lifecycle. This bounded context
maintains the long-term view of customer relationships, tracking everything from initial
account creation through ongoing engagement, contract management, renewals, upsells, and
cross-sells. Account Management represents the post-sale relationship that drives recurring
revenue and customer lifetime value.

Account Management is classified as a supporting subdomain. While it follows established
CRM patterns, the specific strategies for customer retention, expansion selling, and
relationship development can provide competitive advantage. The quality of account management
directly influences churn rates, renewal rates, and expansion revenue.

## Key Concepts

**Account Hierarchy**: Business accounts may be organized hierarchically, with parent
companies and subsidiaries, divisions, or business units as child accounts. The hierarchy
affects pricing, contract terms, and relationship management strategy. Enterprise deals
often involve multiple child accounts under a single parent relationship.

**Contact Management**: Each account has one or more contacts representing individuals
involved in the business relationship. Contacts have roles (decision maker, influencer,
champion, end user, technical evaluator) that inform engagement strategy. Every account
must maintain at least one primary contact as an aggregate invariant.

**Contract Lifecycle**: Contracts move through stages from draft to negotiation, execution,
active, and expiration. The contract lifecycle includes renewal processing, which begins
before expiration with renewal risk assessment, pricing review, and negotiation of new
terms. Contract value recognition follows ASC 606 / IFRS 15 guidelines.

**Renewal Management**: A systematic process for managing contract renewals. Renewal
management includes early identification of at-risk accounts, proactive outreach, terms
renegotiation, and renewal confirmation. The RenewalRiskAssessmentService evaluates
renewal likelihood based on engagement, usage, and satisfaction indicators.

**Expansion Selling**: The identification and pursuit of upsell and cross-sell opportunities
within existing accounts. Upselling offers higher-tier products or services, while
cross-selling offers complementary products. Both activities leverage the established
relationship and account knowledge to drive additional revenue.

**Customer Lifetime Value (CLV)**: A projection of the total revenue an account will
generate over the entire business relationship. CLV calculations consider current contract
value, renewal probability, expansion potential, and expected relationship duration.
CLV informs resource allocation and account prioritization.

**Account Health Scoring**: A composite metric that evaluates the overall health of an
account relationship based on engagement frequency, support ticket volume, product usage
patterns, NPS scores, and payment history. Account health scores help identify at-risk
accounts before churn occurs.

## Aggregates and Entities

- **Account** (Aggregate Root): Contains company information, industry classification,
  territory assignment, lifetime value, and references to contacts. Enforces the
  invariant that at least one primary contact exists.

- **Contact** (Entity): An individual associated with an account, with role, communication
  preferences, and influence level.

- **Contract** (Aggregate Root): Contains terms, pricing, duration, renewal dates, and
  status. Enforces date validity invariants.

- **Money** (Value Object): Monetary values for contract amounts and lifetime value.

- **DateRange** (Value Object): Contract duration and renewal windows.

## Domain Events

- AccountCreated: A new customer account has been established.
- ContactAdded: A new contact has been associated with an account.
- ContactRoleChanged: A contact's role within the account has been updated.
- ContractCreated: A new contract has been drafted for an account.
- ContractSigned: A contract has been fully executed by all parties.
- ContractRenewed: An existing contract has been renewed for a new term.
- ContractExpired: A contract has reached its end date without renewal.
- UpsellIdentified: An upsell opportunity has been identified within an account.
- CrossSellIdentified: A cross-sell opportunity has been identified.
- ChurnRiskDetected: An account has been flagged as at risk of not renewing.

## Context Relationships

- **Upstream from Opportunity Management**: Receives DealClosedWon events to create or
  update account records.

- **Upstream from Order Management**: Receives OrderConfirmed events to update purchase
  history.

- **Downstream to Opportunity Management**: Upsell and cross-sell opportunities feed back
  as new opportunities.

- **Downstream to Sales Analytics**: Account metrics and contract data flow to analytics
  for retention and expansion reporting.

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software."
  Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." O'Reilly Media, 2021.
- Peppers, Don and Rogers, Martha. "Managing Customer Relationships." Wiley, 2011.
- Mehta, Nick and Steinman, Dan. "Customer Success." Wiley, 2016.
