# Domain Services in Project Portfolio Management

## Overview

Domain services in PPM handle cross-aggregate operations: portfolio optimization, resource allocation, schedule analysis, and risk aggregation.

## PPM Domain Services

### PortfolioPrioritizationService

- **Purpose**: Ranks projects within a portfolio based on scoring criteria
- **Inputs**: Portfolio with project proposals, scoring model
- **Logic**: Apply weighted criteria (strategic alignment, financial return, risk, urgency) → score each project → rank by score → identify cut line based on budget constraints
- **Output**: PrioritizedProjectList with rankings and recommendations
- **Aggregates involved**: Portfolio, ProjectProposal (multiple)

### ResourceAllocationOptimizer

- **Purpose**: Optimally assigns resources to projects based on skills, availability, and priority
- **Inputs**: Resource pool, project demands with required skills, project priorities
- **Logic**: Match skills → check availability → allocate by priority → identify conflicts → suggest resolutions
- **Output**: AllocationPlan with assignments and conflict reports
- **Aggregates involved**: Resource (multiple), Project (multiple)

### CriticalPathAnalyzer

- **Purpose**: Identifies the critical path and schedule risk for a project
- **Inputs**: Project tasks with durations, dependencies
- **Logic**: Forward pass (earliest start/finish) → backward pass (latest start/finish) → identify zero-float tasks → calculate total project duration
- **Output**: CriticalPathResult with critical tasks, total duration, and float values
- **Aggregates involved**: Project (single)

### EarnedValueCalculator

- **Purpose**: Calculates earned value metrics for a project
- **Inputs**: Budget baseline, completed milestones/tasks, actual costs
- **Logic**: Calculate PV, EV, AC → derive SPI, CPI → project EAC, ETC, VAC
- **Output**: EarnedValueMetrics value object
- **Aggregates involved**: Budget, Project

### PortfolioRiskAggregator

- **Purpose**: Aggregates risk across all projects in a portfolio
- **Inputs**: Portfolio, project risks
- **Logic**: Aggregate risk scores → identify correlation between project risks → calculate portfolio-level risk exposure → flag risks exceeding risk appetite
- **Output**: PortfolioRiskReport with aggregated risk profile
- **Aggregates involved**: Risk (multiple), Portfolio

### DependencyConflictChecker

- **Purpose**: Detects circular dependencies and scheduling conflicts
- **Inputs**: Tasks with dependency relationships
- **Logic**: Topological sort to detect cycles → validate date constraints → identify impossible schedules
- **Output**: ConflictReport with circular dependencies and constraint violations

## Relationships to Other Topics

- [Aggregates](../aggregates/) — Services coordinate across aggregates
- [Repositories](../repositories/) — Services use repositories to load aggregates
- [Value Objects](../value-objects/) — Services return value objects as results
- [Domain Events](../domain-events/) — Services may produce events

## References

- Evans, Eric. "Domain-Driven Design." Chapter 5. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 7. Addison-Wesley, 2013.
- PMI. "PMBOK Guide." 7th ed. PMI, 2021.
