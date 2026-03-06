# Ubiquitous Language in Project Portfolio Management

## Overview

Ubiquitous language in PPM ensures developers and project management professionals use the same terms consistently. PPM has well-established terminology from PMI, PRINCE2, and agile frameworks that the software model should respect.

## One Language per Context

The term "Project" means different things:

- **Governance**: An investment proposal with business case, strategic fit, and expected ROI
- **Execution**: A plan with tasks, milestones, deliverables, and a schedule
- **Financial**: A budget with planned costs, actuals, and variances
- **Resource**: A demand for specific skills and capacity over time
- **Risk**: A source of uncertainty requiring identification and mitigation

## Key Terms by Context

| Term | Governance | Execution | Financial | Resource | Risk |
|------|-----------|-----------|-----------|----------|------|
| Project | Investment decision | Execution plan | Budget item | Resource demand | Risk source |
| Status | Pipeline stage | RAG status | Variance | Utilization | Risk level |
| Value | Strategic alignment | Deliverables | ROI/NPV | Skills match | Risk-adjusted |

See [language.md](../../language.md) for the full glossary.

## Language in Code

- Entity names: `Portfolio`, `Project`, `Risk`, `Resource`, `Budget`
- Methods: `portfolio.prioritizeProjects()`, `project.completeeMilestone()`, `risk.escalate()`
- Events: `ProjectApproved`, `MilestoneCompleted`, `BudgetExceeded`, `RiskEscalated`
- Value objects: `CurrencyAmount`, `DateRange`, `Priority`, `RAGStatus`, `RiskScore`

## Relationships to Other Topics

- [Strategic Design](../strategic-design/) — Language developed during strategic design
- [Bounded Contexts](../bounded-contexts/) — Each context has its own language
- [Entities](../entities/) — Named with ubiquitous language
- [Domain Events](../domain-events/) — Named with past-tense domain language

## References

- Evans, Eric. "Domain-Driven Design." Chapter 2. Addison-Wesley, 2003.
- PMI. "PMBOK Guide." 7th ed. PMI, 2021.
- PMI. "The Standard for Portfolio Management." PMI, 2017.
