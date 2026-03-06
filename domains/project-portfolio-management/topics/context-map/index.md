# Context Map in Project Portfolio Management

## Overview

The context map documents relationships between PPM bounded contexts. Portfolio decisions flow downstream to execution, which generates data that feeds back to governance. Resources are shared across projects. Risks and finances are tracked in dedicated contexts that consume data from execution.

## Context Relationships

### Portfolio Governance → Execution Management (Customer-Supplier)

- Governance is upstream; Execution is downstream
- When a project is approved (ProjectApproved), Execution creates a project plan
- Governance provides strategic objectives and priority; Execution provides status and progress
- Governance consumes StatusReported events for portfolio-level dashboards

### Portfolio Governance → Financial Tracking (Customer-Supplier)

- Governance is upstream; Financial is downstream
- Approved project budgets flow to Financial Tracking for cost monitoring
- Financial provides BudgetExceeded and ForecastUpdated events back to Governance

### Execution Management → Risk Management (Customer-Supplier)

- Execution is upstream; Risk is downstream for project-level risks
- Project activities and milestones are the source of risk identification
- Risk context publishes RiskEscalated events that Execution must address

### Execution Management → Resource Management (Customer-Supplier)

- Execution is downstream consumer of resource allocations
- Resource Management provides availability; Execution requests allocations
- ResourceAllocationChanged events affect project schedules

### Execution Management → Financial Tracking (Customer-Supplier)

- Execution publishes TaskCompleted, MilestoneReached, and ActualCostRecorded events
- Financial Tracking uses these for earned value calculations and variance analysis

### Resource Management → Financial Tracking (Customer-Supplier)

- Resource costs (labor rates, utilization) flow to Financial for cost tracking
- Financial provides budget constraints that limit resource allocation

### External Systems

- **HR System** → Resource Management: ACL for employee data and skills
- **ERP Finance** → Financial Tracking: ACL for actuals from accounting system
- **PMO Tools** → Execution Management: ACL for data from MS Project, Jira, etc.

## Integration Summary

| Upstream | Downstream | Pattern | Mechanism |
|----------|-----------|---------|-----------|
| Governance | Execution | Customer-Supplier | Domain Events |
| Governance | Financial | Customer-Supplier | Domain Events |
| Execution | Risk | Customer-Supplier | Domain Events |
| Execution | Financial | Customer-Supplier | Domain Events |
| Resource | Execution | Customer-Supplier | Domain Events |
| Resource | Financial | Customer-Supplier | Domain Events |
| HR System | Resource | ACL | API Integration |
| ERP Finance | Financial | ACL | API/File Import |

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) — Nodes on the context map
- [Integration Patterns](../integration-patterns/) — Detailed pattern descriptions
- [Anti-Corruption Layer](../anti-corruption-layer/) — For external system integration
- [Domain Events](../domain-events/) — Primary communication mechanism

## References

- Evans, Eric. "Domain-Driven Design." Chapter 14. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 3. Addison-Wesley, 2013.
