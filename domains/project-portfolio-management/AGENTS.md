# Project Portfolio Management - Domain-Driven Design

This domain applies Domain-Driven Design (DDD) to Project Portfolio Management (PPM), creating a modular and scalable system by modeling software directly after portfolio governance, project execution, risk management, and resource allocation using a shared "ubiquitous language" between developers and project management professionals.

## Bounded Contexts

- **Portfolio Governance Context** - Portfolio selection, prioritization, strategic alignment, KPIs
- **Execution Management Context** - Project plans, milestones, task tracking, deliverables
- **Risk Management Context** - Risk identification, assessment, mitigation, monitoring
- **Resource Management Context** - Resource allocation, capacity planning, skills management
- **Financial Tracking Context** - Budgets, actuals, forecasts, cost tracking, ROI analysis

## Ubiquitous Language

The shared vocabulary between developers and project management professionals is defined in `language.md`.

## Directory Structure

- `index.md` - Main documentation entry point
- `plan.md` - Project plan
- `tasks.md` - Task tracking
- `language.md` - Ubiquitous language glossary
- `topics/` - Detailed topic documentation

## Conventions

- Every directory has `index.md` with a symlink `README.md` -> `index.md`
- No images, diagrams, web apps, front-end, or back-end code
- Comprehensive documentation with references and citations
- Ubiquitous language terms: one per line, one-sentence explanation

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
- Vernon, Vaughn. "Domain-Driven Design Distilled." Addison-Wesley, 2016.
- Millett, Scott & Tune, Nick. "Patterns, Principles, and Practices of Domain-Driven Design." Wrox, 2015.
- PMI. "The Standard for Portfolio Management." Project Management Institute, 2017.
- PMI. "A Guide to the Project Management Body of Knowledge (PMBOK Guide)." Project Management Institute, 2021.
