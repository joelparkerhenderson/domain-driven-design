# Project Portfolio Management - Domain-Driven Design

Domain-Driven Design (DDD) for Project Portfolio Management creates a modular and scalable system by modeling software directly after portfolio governance, project execution, risk management, and resource allocation using a shared "ubiquitous language" between developers and project management professionals. It breaks the system into bounded contexts to manage the complexity of portfolio-level decision making and project-level execution.

## Bounded Contexts

- **[Portfolio Governance Context](topics/portfolio-governance-context/)** - Portfolio selection, prioritization, strategic alignment, KPIs
- **[Execution Management Context](topics/execution-management-context/)** - Project plans, milestones, task tracking, deliverables
- **[Risk Management Context](topics/risk-management-context/)** - Risk identification, assessment, mitigation, monitoring
- **[Resource Management Context](topics/resource-management-context/)** - Resource allocation, capacity planning, skills management
- **[Financial Tracking Context](topics/financial-tracking-context/)** - Budgets, actuals, forecasts, cost tracking, ROI analysis

## Strategic Design

- **[Strategic Design](topics/strategic-design/)** - Principles for structuring the PPM domain
- **[Bounded Contexts](topics/bounded-contexts/)** - Defining model boundaries
- **[Context Map](topics/context-map/)** - Relationships between bounded contexts
- **[Ubiquitous Language](topics/ubiquitous-language/)** - Shared vocabulary for developers and PMs
- **[Subdomains](topics/subdomains/)** - Core, supporting, and generic subdomains
- **[Anti-Corruption Layer](topics/anti-corruption-layer/)** - Protecting domain models from external systems

## Tactical Design

- **[Aggregates](topics/aggregates/)** - Clusters of entities and value objects
- **[Entities](topics/entities/)** - Objects with unique identity
- **[Value Objects](topics/value-objects/)** - Immutable objects defined by attributes
- **[Domain Events](topics/domain-events/)** - Significant occurrences in the domain
- **[Repositories](topics/repositories/)** - Persistence abstraction for aggregates
- **[Domain Services](topics/domain-services/)** - Operations spanning multiple aggregates

## Cross-Cutting Concerns

- **[Event-Driven Architecture](topics/event-driven-architecture/)** - Event flows, sagas, choreography vs. orchestration
- **[Integration Patterns](topics/integration-patterns/)** - Shared kernel, published language, open host service
- **[PPM Standards](topics/ppm-standards/)** - PMI, PRINCE2, SAFe, OPM3
- **[Regulatory Compliance](topics/regulatory-compliance/)** - Governance, audit, reporting requirements

## Key Files

- [Ubiquitous Language Glossary](language.md) - Canonical list of domain terms
- [Project Plan](plan.md) - Implementation plan and phases
- [Tasks](tasks.md) - Task tracking checklist
- [All Topics](topics/) - Complete topic listing

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
- Vernon, Vaughn. "Domain-Driven Design Distilled." Addison-Wesley, 2016.
- PMI. "The Standard for Portfolio Management." 4th ed. Project Management Institute, 2017.
- PMI. "A Guide to the Project Management Body of Knowledge (PMBOK Guide)." 7th ed. Project Management Institute, 2021.
- PMI. "The Standard for Program Management." 4th ed. Project Management Institute, 2017.
