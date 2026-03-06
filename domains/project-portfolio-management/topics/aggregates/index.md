# Aggregates in Project Portfolio Management

## Overview

Aggregates in PPM model the core transactional units: portfolios with their projects, projects with their tasks, risks with their mitigations, and budgets with their line items.

## PPM Aggregates

### Portfolio Aggregate

- **Root**: Portfolio (identified by PortfolioID)
- **Internal entities**: ProjectProposal, InvestmentCategory
- **Value objects**: ScoringModel, StrategicObjective, Priority
- **Invariants**: Total investment cannot exceed portfolio budget ceiling; all projects must have a priority ranking; active projects must have approved business cases
- **Events**: PortfolioRebalanced, ProjectApproved, ProjectDeferred, ProjectCancelled

### Project Aggregate (Execution)

- **Root**: Project (identified by ProjectID)
- **Internal entities**: Milestone, Task, Deliverable
- **Value objects**: DateRange, RAGStatus, WBSElement, Dependency
- **Invariants**: Project end date must be after start date; milestones must fall within project date range; tasks cannot exceed milestone dates; dependencies must not create circular references
- **Events**: ProjectStarted, MilestoneCompleted, TaskCompleted, ProjectCompleted, StatusReported

### Risk Aggregate

- **Root**: Risk (identified by RiskID)
- **Value objects**: RiskScore (probability × impact), MitigationStrategy, RiskCategory, RiskOwner
- **Invariants**: Risk must have an owner; mitigation plan required for risks above threshold; closed risks cannot be modified
- **Events**: RiskIdentified, RiskAssessed, RiskMitigated, RiskEscalated, RiskClosed

### Budget Aggregate

- **Root**: Budget (identified by BudgetID, linked to ProjectID)
- **Internal entities**: BudgetLineItem, CostRecord
- **Value objects**: CurrencyAmount, FiscalPeriod, CostCategory
- **Invariants**: Budget total equals sum of line items; actuals must reference valid cost categories; approved budgets require change request for modification
- **Events**: BudgetApproved, CostRecorded, BudgetExceeded, ForecastUpdated

### Resource Aggregate

- **Root**: Resource (identified by ResourceID)
- **Value objects**: Skill, Capacity, Availability, AllocationPeriod
- **Invariants**: Total allocations cannot exceed 100% capacity; allocation periods must not overlap for the same project; skills must be from valid skill taxonomy
- **Events**: ResourceAllocated, ResourceReleased, CapacityChanged, SkillAdded

## Aggregate Design Rules

1. **Reference by identity**: Project references Portfolio by PortfolioID; Risk references Project by ProjectID
2. **Events for cross-aggregate coordination**: ProjectApproved triggers budget creation and resource allocation
3. **Small aggregates**: A Project contains its Milestones and Tasks but references Resources by ID

## Relationships to Other Topics

- [Entities](../entities/) — Aggregate roots are entities
- [Value Objects](../value-objects/) — Aggregates contain value objects
- [Domain Events](../domain-events/) — Aggregates produce events
- [Repositories](../repositories/) — One repository per aggregate root

## References

- Evans, Eric. "Domain-Driven Design." Chapter 6. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 10. Addison-Wesley, 2013.
