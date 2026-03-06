# Bounded Contexts in Agile Management

## Overview

A bounded context is a well-defined boundary within which a particular domain model applies and terms have specific, unambiguous meaning. In agile management, bounded contexts prevent the confusion that arises when the same term (e.g., "story," "velocity," "sprint") carries different connotations in different parts of the organization or process. Each bounded context encapsulates a cohesive set of concepts, rules, and behaviors, allowing teams to reason about one aspect of agile management without being overwhelmed by the entire system.

## Relevance to Agile Management

Agile management spans multiple concerns: product strategy, sprint execution, people management, process improvement, and delivery coordination. A single unified model would accumulate every attribute and behavior for every use case -- a Product Owner's view of a story differs from the Scrum Master's impediment-tracking view, which differs again from the release manager's deployment-readiness view. Bounded contexts let each concern own a focused model that serves its specific needs while communicating with others through well-defined interfaces.

## The Five Agile Bounded Contexts

### Backlog Management Context

- **Focus**: Product backlog ownership, user story creation and splitting, epic decomposition, backlog prioritization, refinement sessions, estimation
- **Aggregate Roots**: ProductBacklog, Epic
- **Key Operations**: Create story, split story, estimate story, prioritize backlog, refine backlog, decompose epic, apply RICE/MoSCoW scoring
- **Language**: Product Backlog, User Story, Epic, Feature, Acceptance Criteria, Definition of Ready, RICE Score, MoSCoW Priority, Story Splitting, Backlog Refinement, Product Owner
- **Team Ownership**: Product Management
- **Key Invariants**:
  - Every story must have acceptance criteria before being marked as "ready"
  - Epics must decompose into at least one user story
  - Backlog items must have a unique priority rank within their product backlog

### Sprint Execution Context

- **Focus**: Sprint lifecycle management (planning, execution, review, retrospective), daily standups, burndown tracking, impediment management, sprint goal achievement
- **Aggregate Roots**: Sprint
- **Key Operations**: Plan sprint, start sprint, conduct daily standup, track burndown, raise impediment, resolve impediment, review sprint, complete sprint
- **Language**: Sprint, Sprint Goal, Sprint Backlog, Sprint Backlog Item, Burndown Chart, Daily Standup, Sprint Review, Impediment, Definition of Done, Sprint Velocity, Time-box
- **Team Ownership**: Scrum Master / Delivery Team
- **Key Invariants**:
  - A sprint must have a sprint goal before it can start
  - Sprint duration is fixed once the sprint begins (time-box integrity)
  - Items cannot be added to an active sprint without team agreement
  - Only items meeting the Definition of Done can be marked complete

### Team & Capacity Context

- **Focus**: Team composition, velocity tracking and prediction, capacity planning, skill matrix management, resource allocation across sprints, team health
- **Aggregate Roots**: Team
- **Key Operations**: Form team, add/remove team member, calculate velocity, plan capacity, update skill matrix, allocate resources, track availability
- **Language**: Team, Team Member, Velocity, Capacity, Availability, Skill Matrix, Cross-functional Team, Self-organizing Team, Sustainable Pace, Load Factor
- **Team Ownership**: Engineering Management / Scrum Master
- **Key Invariants**:
  - Velocity is calculated from completed sprints only (not in-progress work)
  - Team capacity cannot exceed the sum of individual member availability
  - A team member cannot be allocated to more than their available capacity

### Continuous Improvement Context

- **Focus**: Retrospective facilitation, action item identification and tracking, agile metrics analysis (cycle time, lead time, throughput), process evolution, Kaizen practices
- **Aggregate Roots**: Retrospective
- **Key Operations**: Facilitate retrospective, identify action items, assign action items, track improvement metrics, analyze trends, implement process changes
- **Language**: Retrospective, Action Item, Cycle Time, Lead Time, Throughput, Cumulative Flow Diagram, Process Improvement, Kaizen, What Went Well, What Could Improve, Experiment
- **Team Ownership**: Scrum Master / Agile Coach
- **Key Invariants**:
  - Every retrospective must produce at least one action item
  - Action items must have an assignee and a target date
  - Metrics are calculated from historical data and cannot be manually overridden

### Release Management Context

- **Focus**: Release planning, deployment coordination, feature flag management, semantic versioning, CI/CD integration, rollback procedures, release notes
- **Aggregate Roots**: Release
- **Key Operations**: Plan release, create release candidate, toggle feature flags, deploy release, verify deployment, rollback release, generate release notes
- **Language**: Release, Release Candidate, Feature Flag, Semantic Version, Deployment Pipeline, Rollback, Release Train, Release Notes, Go/No-Go Decision, Hotfix
- **Team Ownership**: DevOps / Release Engineering
- **Key Invariants**:
  - A release must contain at least one completed increment
  - Feature flags must have an owner and an expiration policy
  - Rollback procedures must be verified before any production deployment

## Boundary Enforcement

Each bounded context enforces its boundary by:

- Maintaining its own data store or schema, avoiding shared database coupling
- Exposing only well-defined interfaces (APIs, domain events) to other contexts
- Translating external concepts through anti-corruption layers when integrating with tools like Jira or Azure DevOps
- Owning its deployment lifecycle independently, enabling teams to release at their own cadence

## How Terms Differ Across Contexts

The same concept takes on different characteristics depending on the context:

| Term | Backlog Management | Sprint Execution | Team & Capacity |
|------|-------------------|------------------|-----------------|
| Story | A requirements artifact with acceptance criteria | A work item being actively developed | A unit of effort consuming capacity |
| Velocity | Not directly used | Sprint-level completion metric | Planning input for future capacity |
| Priority | Strategic product value ranking | Execution order within sprint | Not directly used |

## Relationships to Other Topics

- [Strategic Design](../strategic-design/) -- Bounded contexts are the primary output of strategic design
- [Context Map](../context-map/) -- Shows how the five agile bounded contexts relate and communicate
- [Ubiquitous Language](../ubiquitous-language/) -- Each bounded context defines its own language
- [Anti-Corruption Layer](../anti-corruption-layer/) -- Protects context boundaries during integration with external tools
- [Aggregates](../aggregates/) -- Each context contains its own aggregates
- [Integration Patterns](../integration-patterns/) -- Defines how contexts exchange information

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 14: Maintaining Model Integrity. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 2: Domains, Subdomains, and Bounded Contexts. Addison-Wesley, 2013.
- Schwaber, Ken and Sutherland, Jeff. "The Scrum Guide." 2020.
- Anderson, David. "Kanban: Successful Evolutionary Change for Your Technology Business." Blue Hole Press, 2010.
- Cohn, Mike. "Agile Estimating and Planning." Prentice Hall, 2005.
