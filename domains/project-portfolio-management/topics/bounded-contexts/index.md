# Bounded Contexts in Project Portfolio Management

## Overview

Bounded contexts define the boundaries within which PPM domain models are consistent. A "Project" means different things to a portfolio governance board (investment decision), a project manager (execution plan), and a financial controller (budget line).

## The Five PPM Bounded Contexts

### Portfolio Governance Context

- **Focus**: Portfolio composition, project selection, prioritization, strategic alignment, investment categories, gate reviews, KPIs
- **Aggregate Roots**: Portfolio, ProjectProposal, GateReview
- **Language**: Portfolio, Strategic Alignment, Scoring Model, Investment Category, Gate, Pipeline, Prioritization

### Execution Management Context

- **Focus**: Project plans, WBS, milestones, tasks, deliverables, status reporting, change requests
- **Aggregate Roots**: Project, Milestone, ChangeRequest
- **Language**: Project Plan, WBS, Task, Milestone, Deliverable, Critical Path, Gantt, Sprint, RAG Status

### Risk Management Context

- **Focus**: Risk identification, qualitative and quantitative assessment, mitigation strategies, risk monitoring, risk registers
- **Aggregate Roots**: Risk, RiskRegister
- **Language**: Risk, Probability, Impact, Mitigation, Contingency, Risk Appetite, Risk Owner, Residual Risk

### Resource Management Context

- **Focus**: Resource pools, capacity planning, allocation, skills inventory, demand forecasting, utilization
- **Aggregate Roots**: Resource, ResourcePool, AllocationPlan
- **Language**: Resource, Capacity, Allocation, Utilization, Skill, Demand, Supply, Availability

### Financial Tracking Context

- **Focus**: Budgets, cost tracking, actuals, forecasts, earned value analysis, ROI, cost-benefit analysis
- **Aggregate Roots**: Budget, CostRecord, Forecast
- **Language**: Budget, Actual, Forecast, Variance, EVM, ROI, NPV, IRR, Cost Baseline

## Boundary Enforcement

Each context maintains its own model. A "Project" in Governance is a ProjectProposal (business case, strategic fit, expected benefits). In Execution, it is a ProjectPlan (tasks, milestones, deliverables). In Financial Tracking, it is a BudgetItem (planned costs, actuals, variances).

## Relationships to Other Topics

- [Strategic Design](../strategic-design/) — Bounded contexts result from strategic design
- [Context Map](../context-map/) — Shows how contexts relate
- [Ubiquitous Language](../ubiquitous-language/) — Each context has its own language

## References

- Evans, Eric. "Domain-Driven Design." Chapter 14. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 2. Addison-Wesley, 2013.
- PMI. "The Standard for Portfolio Management." PMI, 2017.
