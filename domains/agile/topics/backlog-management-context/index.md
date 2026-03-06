# Backlog Management Context in Agile Management

## Overview

The Backlog Management Context is the bounded context responsible for product backlog ownership, user story creation and refinement, epic decomposition, estimation, and prioritization. This context represents the Product Owner's domain -- the strategic decisions about what to build and in what order. It is concerned with requirements articulation, value assessment, and ensuring that work items are ready for sprint execution, not with how items are actually implemented or delivered.

## Relevance to Agile Management

The product backlog is the single source of truth for all work a team might undertake. Without a well-managed backlog, sprint planning devolves into ad hoc negotiation, teams commit to poorly understood stories, and strategic priorities are lost in operational noise. The Backlog Management Context provides the discipline to ensure that every item entering a sprint has been refined, estimated, prioritized, and validated against acceptance criteria.

## Core Responsibilities

### Product Backlog Ownership

The product backlog is a living, ordered list of everything that might be needed in the product. It is never complete -- new items are continuously added as the product evolves and market conditions change. The Product Owner is the sole authority for backlog content and ordering.

### User Story Writing

User stories capture requirements in the format: "As a [role], I want [capability], so that [benefit]." Stories follow the INVEST criteria -- Independent, Negotiable, Valuable, Estimable, Small, Testable. Each story must have acceptance criteria that define when the story is done.

### Epic Decomposition

Epics are large bodies of work that span multiple sprints. Decomposition breaks an epic into smaller user stories that can be completed within a single sprint. Decomposition techniques include splitting by workflow step, by business rule variation, by data variation, or by user role. An epic must decompose into at least one user story before it can be considered refined.

### Backlog Refinement

Refinement (formerly called "grooming") is the ongoing process of reviewing, clarifying, estimating, and ordering backlog items. Refinement sessions typically consume 5-10% of the team's sprint capacity. The output is a pool of "ready" items -- stories that have acceptance criteria, estimates, and sufficient clarity for sprint planning.

### Prioritization Frameworks

**RICE Scoring**: Quantitative framework that scores items by Reach (how many users), Impact (how much value per user), Confidence (certainty in estimates), and Effort (team resources required). Score = (Reach x Impact x Confidence) / Effort.

**MoSCoW Method**: Categorization framework that classifies items as Must have (non-negotiable), Should have (important but not critical), Could have (desirable if capacity permits), or Won't have (explicitly excluded from current scope). Items are rank-ordered within each category.

## Aggregate Roots

- **ProductBacklog**: The primary aggregate root, containing all backlog items with their priorities, estimates, and readiness states
- **Epic**: May serve as its own aggregate root for large-scale planning, containing child stories and tracking decomposition progress

## Key Invariants

- Every backlog item must have a unique priority rank within its product backlog
- Items marked as "ready" must have acceptance criteria and a size estimate
- Epics must decompose into at least one user story before they can be marked as refined
- The Product Owner is the sole authority for priority ordering
- Backlog items must have a clear value proposition (the "so that" clause) before refinement is complete

## Domain Events Produced

- BacklogItemCreated -- A new story, bug, spike, or technical debt item is added
- BacklogItemEstimated -- A team assigns a size estimate to an item
- BacklogItemPrioritized -- An item's priority rank is changed
- BacklogRefined -- A refinement session concludes with updated items
- EpicDecomposed -- An epic is split into child stories
- StoryReady -- A story meets all Definition of Ready criteria

## Domain Events Consumed

- StoryCompleted (from Sprint Execution) -- Updates epic progress tracking
- VelocityCalculated (from Team & Capacity) -- Informs epic timeline forecasting
- ReleaseDeployed (from Release Management) -- Closes delivered items and epics

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) -- Backlog Management is one of the five agile bounded contexts
- [Sprint Execution Context](../sprint-execution-context/) -- Consumes ready items from the backlog for sprint planning
- [Aggregates](../aggregates/) -- ProductBacklog and Epic are aggregate roots in this context
- [Entities](../entities/) -- UserStory, Epic, and BacklogItem are entities within this context
- [Value Objects](../value-objects/) -- Priority, AcceptanceCriteria, StoryPoint, and Estimate are value objects
- [Anti-Corruption Layer](../anti-corruption-layer/) -- Translates external tool concepts (Jira epics, Azure DevOps work items) into domain models

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 14: Maintaining Model Integrity. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 2: Domains, Subdomains, and Bounded Contexts. Addison-Wesley, 2013.
- Schwaber, Ken and Sutherland, Jeff. "The Scrum Guide." Section: Product Backlog. 2020.
- Cohn, Mike. "Agile Estimating and Planning." Chapters 4-8: User Stories, Estimation, and Prioritization. Prentice Hall, 2005.
- Cohn, Mike. "User Stories Applied: For Agile Software Development." Addison-Wesley, 2004.
