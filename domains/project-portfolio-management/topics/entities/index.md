# Entities in Project Portfolio Management

## Overview

Entities in PPM represent the core identifiable objects tracked across their lifecycle: portfolios, projects, resources, risks, and milestones.

## PPM Entities

### Portfolio

- **Identity**: PortfolioID (system-generated)
- **Attributes**: Name, description, strategic objectives, investment categories, budget ceiling, owner
- **Lifecycle**: Created → Active → Under Review → Rebalanced → Archived
- **Context**: Aggregate root in Portfolio Governance

### Project

- **Identity**: ProjectID (system-generated or business-assigned)
- **Attributes**: Name, description, sponsor, project manager, start/end dates, status, priority
- **Lifecycle**: Proposed → Approved → Planning → Executing → Closing → Completed / Cancelled
- **Context**: Aggregate root in Execution Management; referenced in Governance as ProjectProposal

### Resource

- **Identity**: ResourceID (may map to employee ID from HR)
- **Attributes**: Name, role, skills, capacity, department, cost rate, availability
- **Lifecycle**: Active → (allocated to projects) → Unavailable → Active
- **Context**: Aggregate root in Resource Management

### Risk

- **Identity**: RiskID (system-generated)
- **Attributes**: Title, description, category, probability, impact, score, owner, status, mitigation plan
- **Lifecycle**: Identified → Assessed → Mitigated → Monitoring → Closed / Materialized
- **Context**: Aggregate root in Risk Management

### Milestone

- **Identity**: MilestoneID
- **Attributes**: Name, target date, actual date, status, deliverables
- **Lifecycle**: Planned → In Progress → Completed / Missed
- **Context**: Entity within Project aggregate in Execution Management

### Stakeholder

- **Identity**: StakeholderID
- **Attributes**: Name, role, influence level, interest level, communication preference
- **Context**: Referenced across Governance and Execution contexts

## Relationships to Other Topics

- [Aggregates](../aggregates/) — Entities serve as aggregate roots
- [Value Objects](../value-objects/) — Entities contain value objects
- [Repositories](../repositories/) — Repositories persist entities
- [Ubiquitous Language](../ubiquitous-language/) — Entity names from domain vocabulary

## References

- Evans, Eric. "Domain-Driven Design." Chapter 5. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 5. Addison-Wesley, 2013.
- PMI. "PMBOK Guide." 7th ed. PMI, 2021.
