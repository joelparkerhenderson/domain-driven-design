# Agile - Domain-Driven Design

## Overview

This domain applies Domain-Driven Design (DDD) to agile methodology management systems, modeling backlog management, sprint execution, team capacity planning, continuous improvement ceremonies, and release management. The domain encompasses Scrum, Kanban, and scaled frameworks.

## Bounded Contexts

- **Backlog Management Context**: Product backlog, user stories, epics, prioritization, and refinement
- **Sprint Execution Context**: Sprint planning, daily standups, sprint reviews, and work tracking
- **Team & Capacity Context**: Team composition, velocity tracking, capacity planning, and resource management
- **Continuous Improvement Context**: Retrospectives, action items, metrics analysis, and process evolution
- **Release Management Context**: Release planning, deployment coordination, feature flags, and versioning

## Topics

### Strategic Design

- [Strategic Design](topics/strategic-design/) — Strategic DDD patterns applied to agile management
- [Bounded Contexts](topics/bounded-contexts/) — Agile bounded context definitions
- [Context Map](topics/context-map/) — Relationships between agile contexts
- [Ubiquitous Language](topics/ubiquitous-language/) — Shared agile terminology
- [Subdomains](topics/subdomains/) — Core, supporting, and generic agile subdomains
- [Anti-Corruption Layer](topics/anti-corruption-layer/) — Translation boundaries with external systems

### Tactical Design

- [Aggregates](topics/aggregates/) — Agile aggregate roots and consistency boundaries
- [Entities](topics/entities/) — Identifiable agile domain objects
- [Value Objects](topics/value-objects/) — Immutable agile descriptive objects
- [Domain Events](topics/domain-events/) — Significant agile occurrences
- [Repositories](topics/repositories/) — Persistence abstractions for agile aggregates
- [Domain Services](topics/domain-services/) — Cross-aggregate agile operations

### Bounded Contexts

- [Backlog Management Context](topics/backlog-management-context/) — Product backlog and user story management
- [Sprint Execution Context](topics/sprint-execution-context/) — Sprint lifecycle and work tracking
- [Team & Capacity Context](topics/team-capacity-context/) — Team and velocity management
- [Continuous Improvement Context](topics/continuous-improvement-context/) — Retrospectives and process improvement
- [Release Management Context](topics/release-management-context/) — Release planning and deployment

### Cross-Cutting Concerns

- [Event-Driven Architecture](topics/event-driven-architecture/) — Event flows between agile contexts
- [Integration Patterns](topics/integration-patterns/) — Integration with CI/CD, issue trackers, and tools
- [Agile Standards](topics/agile-standards/) — Scrum Guide, Kanban Method, SAFe, and frameworks
- [Regulatory Compliance](topics/regulatory-compliance/) — SOX, audit trails, and governance requirements

## References

- Schwaber, Ken and Sutherland, Jeff. "The Scrum Guide." 2020.
- Anderson, David. "Kanban: Successful Evolutionary Change for Your Technology Business." Blue Hole Press, 2010.
- Cohn, Mike. "Agile Estimating and Planning." Prentice Hall, 2005.
- Evans, Eric. "Domain-Driven Design." Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
