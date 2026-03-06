# Project Portfolio Management DDD - Project Plan

## Goal

Create comprehensive Domain-Driven Design documentation for a Project Portfolio Management (PPM) system, covering strategic design, tactical design, bounded contexts, and cross-cutting concerns.

## Phases

### Phase 1: Scaffolding

- CLAUDE.md - Domain-specific instructions and conventions
- plan.md - This project plan
- tasks.md - Task tracking checklist
- language.md - Ubiquitous language glossary
- index.md - Main documentation entry point

### Phase 2: Strategic Design Topics

- Strategic design principles
- Bounded contexts and their boundaries
- Context map showing relationships
- Ubiquitous language conventions
- Subdomain identification (core, supporting, generic)
- Anti-corruption layers for external system integration

### Phase 3: Tactical Design Topics

- Aggregates (Portfolio + Projects, Project + Tasks)
- Entities (Project, Portfolio, Resource, Risk)
- Value objects (CurrencyAmount, DateRange, Priority, RAGStatus)
- Domain events (ProjectApproved, BudgetExceeded, MilestoneCompleted)
- Repositories for aggregate root persistence
- Domain services spanning multiple aggregates

### Phase 4: Bounded Context Topics

- Portfolio Governance context
- Execution Management context
- Risk Management context
- Resource Management context
- Financial Tracking context

### Phase 5: Cross-Cutting Concerns

- Event-driven architecture patterns
- Integration patterns between contexts
- PPM standards (PMI, PRINCE2, SAFe)
- Regulatory compliance (governance, audit, reporting)

### Phase 6: Review and Finalize

- Update tasks.md to reflect completion
- Verify all symlinks and file structure
- Cross-reference topics for consistency

## References

- Evans, Eric. "Domain-Driven Design." Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
- PMI. "The Standard for Portfolio Management." 4th ed. PMI, 2017.
- PMI. "PMBOK Guide." 7th ed. PMI, 2021.
