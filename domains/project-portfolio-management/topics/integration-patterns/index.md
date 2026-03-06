# Integration Patterns in Project Portfolio Management

## Overview

Integration patterns define how PPM bounded contexts communicate. PPM has a clear hierarchical flow: governance decisions drive execution, which generates data consumed by financial tracking and risk management.

## DDD Integration Patterns in PPM

### Customer-Supplier

The dominant pattern. Clear upstream-downstream relationships:

- **Governance → Execution**: Approved projects flow downstream
- **Governance → Financial**: Budget allocations flow downstream
- **Execution → Financial**: Progress and cost events flow downstream
- **Execution → Risk**: Project activities inform risk identification
- **Resource → Execution**: Allocation decisions affect project schedules

### Shared Kernel

Minimal shared model for cross-context concepts:

- **Project Identity**: ProjectID, project name, and status shared across all contexts
- **Resource Identity**: ResourceID and name shared between Resource and Execution contexts

### Anti-Corruption Layer

For external system integration:

- **HR System → Resource**: Employee data translated to Resource entities
- **ERP Finance → Financial**: Accounting actuals translated to CostRecord value objects
- **PMO Tools → Execution**: MS Project/Jira data translated to internal Task/Milestone models
- **Time Tracking → Resource/Financial**: Timesheet entries translated to domain objects

### Published Language

Standardized formats for external communication:

- PMI scheduling formats for project data exchange
- Financial reporting formats for cost data

### Open Host Service

Context APIs available for multiple consumers:

- **Portfolio Dashboard API**: Governance data consumed by executive dashboards and reporting tools
- **Project Status API**: Execution data consumed by stakeholder portals

## Integration Map

| Upstream | Downstream | Pattern | Mechanism |
|----------|-----------|---------|-----------|
| Governance | Execution | Customer-Supplier | Events |
| Governance | Financial | Customer-Supplier | Events |
| Execution | Financial | Customer-Supplier | Events |
| Execution | Risk | Customer-Supplier | Events |
| Resource | Execution | Customer-Supplier | Events |
| Resource | Financial | Customer-Supplier | Events |
| Project Identity | All | Shared Kernel | Shared Data |
| HR System | Resource | ACL | API |
| ERP Finance | Financial | ACL | API/File |

## Relationships to Other Topics

- [Context Map](../context-map/) — Patterns documented on context map
- [Anti-Corruption Layer](../anti-corruption-layer/) — ACL details
- [Domain Events](../domain-events/) — Primary async mechanism
- [Event-Driven Architecture](../event-driven-architecture/) — Infrastructure for events

## References

- Evans, Eric. "Domain-Driven Design." Chapter 14. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 3. Addison-Wesley, 2013.
