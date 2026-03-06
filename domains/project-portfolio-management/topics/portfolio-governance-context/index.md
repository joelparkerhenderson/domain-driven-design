# Portfolio Governance Context in Project Portfolio Management

## Overview

The Portfolio Governance Context manages the strategic decision-making layer of PPM: portfolio composition, project selection, prioritization, strategic alignment, investment category management, gate reviews, and portfolio-level KPIs.

## Core Responsibilities

- **Project Selection**: Evaluating project proposals against strategic criteria and resource/budget constraints
- **Prioritization**: Ranking projects using scoring models and strategic alignment
- **Portfolio Balancing**: Ensuring the portfolio mix aligns with investment categories (growth, maintenance, compliance, innovation)
- **Gate Reviews**: Conducting stage-gate decisions to continue, modify, defer, or cancel projects
- **KPI Monitoring**: Tracking portfolio-level performance indicators
- **Benefits Realization**: Tracking whether completed projects deliver expected business value

## Aggregates

### Portfolio Aggregate

- **Root**: Portfolio
- **Internal entities**: ProjectProposal, InvestmentCategory
- **Value objects**: ScoringModel, StrategicObjective, Priority, BudgetCeiling
- **Invariants**: Total committed investment ≤ budget ceiling; all active projects must be ranked; investment category allocations sum to 100%

### GateReview Aggregate

- **Root**: GateReview (identified by ReviewID)
- **Value objects**: GateDecision (proceed, modify, defer, cancel), ReviewCriteria, ReviewDate
- **Invariants**: Decision must be documented with rationale; all required reviewers must participate

## Domain Events

| Event | Consumers |
|-------|-----------|
| ProjectProposed | Governance review pipeline |
| ProjectApproved | Execution, Financial, Resource |
| ProjectDeferred | Governance pipeline |
| ProjectCancelled | Execution, Financial, Resource |
| PortfolioRebalanced | All affected projects |
| GateReviewCompleted | Project status update |
| BenefitRealized | Portfolio performance |

## Integration

- **Governance → Execution**: ProjectApproved triggers project plan creation
- **Governance → Financial**: Budget allocation flows downstream
- **Execution → Governance**: StatusReported events feed portfolio dashboards
- **Financial → Governance**: BudgetExceeded triggers governance review

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) — One of five PPM bounded contexts
- [Domain Services](../domain-services/) — PortfolioPrioritizationService
- [Aggregates](../aggregates/) — Portfolio, GateReview are aggregate roots
- [Domain Events](../domain-events/) — Governance events drive downstream contexts

## References

- PMI. "The Standard for Portfolio Management." 4th ed. PMI, 2017.
- Evans, Eric. "Domain-Driven Design." Addison-Wesley, 2003.
- Levine, Harvey A. "Project Portfolio Management." Jossey-Bass, 2005.
