# Repositories in Project Portfolio Management

## Overview

Repositories provide domain-meaningful query interfaces for PPM aggregate roots.

## PPM Repositories

### PortfolioRepository

- `findById(portfolioId)` — Retrieve specific portfolio
- `findActive()` — All active portfolios
- `findByStrategicObjective(objective)` — Portfolios aligned to an objective
- `save(portfolio)` — Persist portfolio

### ProjectRepository

- `findById(projectId)` — Retrieve specific project
- `findByPortfolio(portfolioId)` — Projects in a portfolio
- `findByStatus(status)` — Projects in a given state
- `findByProjectManager(managerId)` — Manager's projects
- `findOverdue()` — Projects past their planned end date
- `save(project)` — Persist project

### RiskRepository

- `findById(riskId)` — Retrieve specific risk
- `findByProject(projectId)` — Risks for a project
- `findByScoreAbove(threshold)` — High-priority risks across portfolio
- `findOpen()` — All unresolved risks
- `save(risk)` — Persist risk

### ResourceRepository

- `findById(resourceId)` — Retrieve specific resource
- `findBySkill(skill)` — Resources with a specific skill
- `findAvailable(dateRange)` — Resources with available capacity
- `findByDepartment(department)` — Resources in a department
- `save(resource)` — Persist resource

### BudgetRepository

- `findById(budgetId)` — Retrieve specific budget
- `findByProject(projectId)` — Budget for a project
- `findOverBudget()` — Projects exceeding budget
- `save(budget)` — Persist budget

## Relationships to Other Topics

- [Aggregates](../aggregates/) — One repository per aggregate root
- [Entities](../entities/) — Repositories persist entities
- [Domain Services](../domain-services/) — Services use repositories

## References

- Evans, Eric. "Domain-Driven Design." Chapter 6. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 12. Addison-Wesley, 2013.
