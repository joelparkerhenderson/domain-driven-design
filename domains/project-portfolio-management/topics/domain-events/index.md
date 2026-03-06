# Domain Events in Project Portfolio Management

## Overview

Domain events in PPM capture the significant decisions, milestones, and state changes that drive portfolio and project workflows.

## PPM Domain Events

### Portfolio Governance Events

| Event | Trigger | Consumers |
|-------|---------|-----------|
| ProjectProposed | New project submitted | Governance review pipeline |
| ProjectApproved | Gate review approves | Execution (create plan), Financial (create budget), Resource |
| ProjectDeferred | Gate review defers | Governance pipeline |
| ProjectCancelled | Project terminated | Execution, Financial, Resource (release) |
| PortfolioRebalanced | Priority changes | All projects affected |

### Execution Management Events

| Event | Trigger | Consumers |
|-------|---------|-----------|
| ProjectStarted | Execution begins | Resource (confirm allocations), Financial |
| MilestoneCompleted | Milestone achieved | Governance (status), Financial (earned value) |
| TaskCompleted | Task finished | Schedule update, Dependencies |
| StatusReported | Regular status update | Governance (dashboard), Financial |
| ChangeRequestApproved | Scope/schedule change | Financial (budget impact), Resource |
| ProjectCompleted | All deliverables done | Governance (benefits realization), Financial (close), Resource (release) |

### Risk Management Events

| Event | Trigger | Consumers |
|-------|---------|-----------|
| RiskIdentified | New risk logged | Risk register, Project manager |
| RiskAssessed | Probability/impact set | Risk dashboard |
| RiskEscalated | Risk exceeds threshold | Governance (portfolio risk), Project manager |
| RiskMaterialized | Risk becomes issue | Execution (impact), Financial (cost impact) |
| RiskClosed | Risk resolved | Risk register update |

### Resource Management Events

| Event | Trigger | Consumers |
|-------|---------|-----------|
| ResourceAllocated | Resource assigned | Execution (schedule), Financial (cost) |
| ResourceReleased | Resource freed | Resource pool, Other projects |
| CapacityChanged | Availability changes | Execution (reschedule), Resource planning |
| ResourceConflictDetected | Over-allocation | Governance (prioritization decision) |

### Financial Tracking Events

| Event | Trigger | Consumers |
|-------|---------|-----------|
| BudgetApproved | Budget authorized | Execution, Resource |
| CostRecorded | Actual cost posted | EVM calculation, Variance analysis |
| BudgetExceeded | Actuals exceed budget | Governance (escalation), Project manager |
| ForecastUpdated | EAC recalculated | Governance (portfolio financials) |

## Relationships to Other Topics

- [Aggregates](../aggregates/) — Aggregates produce events
- [Event-Driven Architecture](../event-driven-architecture/) — Infrastructure for events
- [Integration Patterns](../integration-patterns/) — Events enable inter-context communication

## References

- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 8. Addison-Wesley, 2013.
- Richardson, Chris. "Microservices Patterns." Chapter 4. Manning, 2018.
