# Strategic Design in Project Portfolio Management

## Overview

Strategic design in DDD decomposes a PPM system into bounded contexts that align with how organizations manage portfolios, execute projects, and allocate resources. PPM involves multiple stakeholder perspectives — executives care about strategic alignment and ROI, project managers care about schedules and deliverables, resource managers care about capacity and skills.

## Key Concepts

### Knowledge Crunching

Collaborative extraction of domain knowledge from PPM professionals:

- Workshops with portfolio managers on project selection and prioritization criteria
- Sessions with project managers on planning, tracking, and reporting needs
- Interviews with resource managers on capacity planning and skills matching
- Observation of gate review and governance board processes

### Domain Vision Statement

> "To enable organizations to maximize the value of their project investments by providing a platform that aligns project selection with strategy, optimizes resource allocation, manages risk across the portfolio, and provides transparent visibility into execution progress and financial performance."

### Core Domain Identification

- **Core**: Portfolio optimization (project selection, prioritization, strategic alignment) — the decision-making that differentiates organizations
- **Supporting**: Project execution tracking, resource management, financial tracking
- **Generic**: Authentication, notifications, document storage, reporting infrastructure

## PPM Domain Decomposition

- **Portfolio Governance** — Selection, prioritization, strategic alignment, KPIs, gate reviews
- **Execution Management** — Project plans, milestones, tasks, deliverables, status reporting
- **Risk Management** — Risk identification, assessment, mitigation, monitoring
- **Resource Management** — Allocation, capacity planning, skills matching, demand forecasting
- **Financial Tracking** — Budgets, actuals, forecasts, earned value, ROI

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) — Primary output of strategic design
- [Context Map](../context-map/) — Visualizes context relationships
- [Subdomains](../subdomains/) — Classification by strategic importance
- [Ubiquitous Language](../ubiquitous-language/) — Shared vocabulary within each context

## References

- Evans, Eric. "Domain-Driven Design." Chapters 14–16. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Part I. Addison-Wesley, 2013.
- PMI. "The Standard for Portfolio Management." 4th ed. PMI, 2017.
