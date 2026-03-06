# Resource Management Context in Project Portfolio Management

## Overview

The Resource Management Context manages the people, teams, and capacity required to execute projects: resource pools, skills inventory, allocation, capacity planning, demand forecasting, and utilization tracking.

## Core Responsibilities

- **Resource Pool Management**: Maintaining a catalog of available resources with skills, roles, and capacity
- **Capacity Planning**: Determining available capacity by skill, team, or department over time periods
- **Resource Allocation**: Assigning resources to projects based on skills, availability, and priority
- **Demand Forecasting**: Projecting future resource needs based on portfolio pipeline and project plans
- **Utilization Tracking**: Monitoring actual resource utilization against planned allocation
- **Skills Management**: Tracking skills inventory, identifying gaps, and planning development

## Aggregates

### Resource Aggregate

- **Root**: Resource (identified by ResourceID)
- **Value objects**: Skill (name, category, proficiency), Capacity (hours per period), Availability (calendar), AllocationPeriod (project, percentage, date range), CostRate (hourly/daily rate)
- **Invariants**: Total allocations ≤ 100% capacity; skills from valid taxonomy; cost rate must be positive

### ResourcePool Aggregate

- **Root**: ResourcePool (identified by PoolID — may align with department)
- **Value objects**: PoolCapacity, SkillDistribution
- **Invariants**: Pool capacity = sum of member capacities; utilization cannot exceed capacity

### AllocationPlan Aggregate

- **Root**: AllocationPlan (for a planning period)
- **Internal entities**: AllocationEntry (resource, project, percentage, period)
- **Invariants**: No resource over-allocated across all entries; allocations must have project and resource references

## Domain Events

| Event | Consumers |
|-------|-----------|
| ResourceAllocated | Execution (schedule), Financial (cost forecast) |
| ResourceReleased | Resource pool, Other projects (waitlist) |
| CapacityChanged | Execution (reschedule), Planning |
| ResourceConflictDetected | Governance (prioritization) |
| SkillGapIdentified | HR/Training, Recruitment |
| UtilizationReported | Management reporting |

## Integration

- **Resource → Execution**: Allocation changes affect project schedules
- **Governance → Resource**: Project priority determines allocation priority
- **Resource → Financial**: Resource costs (rates × hours) feed cost tracking
- **HR System → Resource**: Employee data, skills, and availability via ACL

## Key Metrics

- **Utilization rate**: Actual productive hours / available hours
- **Allocation rate**: Allocated hours / available hours
- **Demand-supply gap**: Forecasted demand - available supply by skill
- **Resource conflict count**: Number of over-allocation situations

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) — One of five PPM bounded contexts
- [Context Map](../context-map/) — Upstream to Execution, downstream from Governance
- [Aggregates](../aggregates/) — Resource, ResourcePool, AllocationPlan are aggregate roots
- [Domain Services](../domain-services/) — ResourceAllocationOptimizer
- [Anti-Corruption Layer](../anti-corruption-layer/) — ACL for HR system integration

## References

- PMI. "PMBOK Guide." 7th ed. PMI, 2021.
- PMI. "The Standard for Portfolio Management." PMI, 2017.
- Evans, Eric. "Domain-Driven Design." Addison-Wesley, 2003.
- Wysocki, Robert K. "Effective Project Management." Wiley, 2019.
