# Event-Driven Architecture in Project Portfolio Management

## Overview

EDA enables PPM bounded contexts to communicate through domain events. Portfolio decisions trigger project execution, which generates progress data feeding back to governance. Resources and finances are tracked through events from execution and allocation.

## PPM Event Flow Patterns

### Idea-to-Value Flow

1. `ProjectProposed` → Governance evaluates
2. `ProjectApproved` → Execution creates plan, Financial creates budget, Resource allocates
3. `ProjectStarted` → Execution begins, costs accrue
4. `MilestoneCompleted` → EVM updated, Governance dashboard refreshed
5. `ProjectCompleted` → Resources released, budget closed, benefits realization tracked

### Risk Escalation Flow

1. `RiskIdentified` → Risk assessed and scored
2. `RiskEscalated` → Governance notified, project manager creates mitigation plan
3. `RiskMaterialized` → Becomes issue, financial impact assessed
4. Cost impact recorded → BudgetExceeded if over threshold → Governance review triggered

## Choreography vs. Orchestration

- **Choreography**: ProjectApproved causes Execution, Financial, and Resource to react independently
- **Orchestration**: Project Approval Saga coordinates budget creation, resource allocation, and plan creation in sequence with rollback on failure

## Saga: Project Approval

1. Create budget allocation → success
2. Reserve key resources → success
3. Create project plan template → success
4. Notify stakeholders → success

If step 2 fails (resources unavailable), compensate: release budget allocation, notify governance of resource constraint.

## Relationships to Other Topics

- [Domain Events](../domain-events/) — Events flowing through the architecture
- [Integration Patterns](../integration-patterns/) — EDA foundation for integration
- [Bounded Contexts](../bounded-contexts/) — EDA enables loose coupling

## References

- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 8. Addison-Wesley, 2013.
- Richardson, Chris. "Microservices Patterns." Chapter 4. Manning, 2018.
- Hohpe, Gregor & Woolf, Bobby. "Enterprise Integration Patterns." Addison-Wesley, 2003.
