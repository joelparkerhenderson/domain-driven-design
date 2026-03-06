# Integration Patterns

## Overview

In Domain-Driven Design, integration patterns define how bounded contexts interact with each other and with external systems. These patterns govern data exchange, translation between different domain models, and the contracts that enable independent evolution of system components.

In the OKR domain, integration patterns address both internal communication between the five OKR bounded contexts and external integration with enterprise systems such as HR platforms, project management tools, and business intelligence suites.

## Internal Integration Patterns

### Published Language

The OKR domain defines a published language for inter-context communication. Domain events use a shared schema that all contexts understand, expressed in the ubiquitous language. Event payloads contain well-defined value objects (Score, ConfidenceLevel, DateRange) with documented semantics and validation rules.

This published language enables contexts to evolve their internal models independently while maintaining a stable communication contract.

### Shared Kernel

The OKR domain uses a minimal shared kernel containing identity types and core value objects that are referenced across all contexts:

- **Identity types**: ObjectiveId, KeyResultId, CycleId, PersonId, TeamId, SessionId.
- **Core value objects**: Score, DateRange, ConfidenceLevel.

The shared kernel is versioned and its changes require agreement from all consuming contexts. It is intentionally kept small to minimize coupling.

### Customer-Supplier

Several context relationships follow the customer-supplier pattern:

- **Objective Setting Context (supplier)** and **Key Result Tracking Context (customer)**: The tracking context depends on key result definitions from the setting context. The setting context publishes KeyResultDefined events, and the tracking context adapts its internal models accordingly.
- **Key Result Tracking Context (supplier)** and **Review & Cadence Context (customer)**: The cadence context depends on ScoreFinalized events to determine cycle closure readiness.

In customer-supplier relationships, the upstream (supplier) context commits to a stable event contract, and downstream (customer) contexts adapt to consume it.

### Conformist

The Analytics & Reporting Context operates as a conformist in its relationships with all upstream contexts. It accepts the event schemas published by each context and conforms its read models to the upstream models, without requesting changes.

## External Integration Patterns

### HR System Integration

**Pattern**: Anti-Corruption Layer

**Purpose**: The OKR domain needs team and individual data (names, organizational hierarchy, team memberships) from enterprise HR systems (e.g., Workday, BambooHR, SAP SuccessFactors).

**Approach**: An Anti-Corruption Layer translates HR system concepts into OKR domain concepts. The HR system's employee model (which may include salary, benefits, and performance review data) is mapped to the OKR domain's lightweight Individual entity (PersonId, DisplayName, Email, TeamMemberships, OKRRole). This prevents HR domain complexity from leaking into the OKR model.

**Synchronization**: Periodic batch synchronization or event-driven updates when the HR system publishes employee change events.

### Project Management Integration

**Pattern**: Open Host Service

**Purpose**: Project management tools (e.g., Jira, Asana, Monday.com) contain task-level progress data that can feed into Key Result tracking. Conversely, OKR status may be displayed in project management dashboards.

**Approach**: The OKR system exposes an Open Host Service (API) that project management tools can query for objective and key result status. For inbound data, an Anti-Corruption Layer translates project milestones and completion metrics into ProgressUpdate value objects.

**Use Cases**:
- A Key Result like "Ship feature X by end of quarter" can be automatically tracked by monitoring the corresponding Jira epic's completion percentage.
- Project managers can view how their project contributes to organizational OKRs.

### Business Intelligence Tool Integration

**Pattern**: Published Language (data export)

**Purpose**: BI tools (e.g., Tableau, Power BI, Looker) need access to OKR data for custom reporting and cross-domain analysis.

**Approach**: The Analytics & Reporting Context exposes OKR data through a published language in standard formats (REST APIs, data warehouse tables, or event streams). The BI tool consumes this data and applies its own visualization and analysis capabilities.

**Data Provided**: Objective definitions, key result progress histories, scores, alignment structures, cycle metadata, and health metrics.

### Performance Review System Integration

**Pattern**: Anti-Corruption Layer

**Purpose**: OKR scores and achievement data may inform performance reviews, but the two systems serve different purposes and must remain decoupled.

**Approach**: An Anti-Corruption Layer ensures that OKR scores are not automatically equated with performance ratings. The ACL translates OKR achievement summaries into performance review inputs while preserving the OKR principle that aspirational objective scores of 0.6-0.7 represent success, not underperformance.

**Design Consideration**: This integration requires careful domain modeling to prevent OKR scores from being misused as individual performance metrics, which would undermine the psychological safety needed for ambitious goal-setting.

### Financial Systems Integration

**Pattern**: Anti-Corruption Layer

**Purpose**: Budget and financial planning systems may provide data for financially-oriented Key Results (revenue targets, cost reduction goals, budget utilization).

**Approach**: Financial data is ingested through an ACL that translates accounting concepts into OKR measurement values. Currency-based Key Results receive automated progress updates from financial system data feeds.

## Integration Design Principles

### Autonomy Preservation

Integration patterns are chosen to maximize the autonomy of each bounded context. No context directly accesses another context's database. All communication flows through events or well-defined APIs.

### Translation Over Conformity

When integrating with external systems, Anti-Corruption Layers are preferred over direct model conformity. This ensures the OKR domain model remains clean and is not corrupted by the concepts and constraints of external systems.

### Idempotent Processing

All integration endpoints and event consumers are idempotent, ensuring that duplicate messages or retried API calls do not produce incorrect state.

## Relationships to Other Topics

- **Anti-Corruption Layer**: External integration patterns rely on ACLs described in [Anti-Corruption Layer](../anti-corruption-layer/).
- **Event-Driven Architecture**: Internal integration uses the event infrastructure from [Event-Driven Architecture](../event-driven-architecture/).
- **Context Map**: Integration relationships are visualized in the [Context Map](../context-map/).
- **Bounded Contexts**: Each integration involves specific bounded contexts documented in [Bounded Contexts](../bounded-contexts/).
- **Domain Events**: Inter-context events are defined in [Domain Events](../domain-events/).
- **Regulatory Compliance**: Integration with external systems must satisfy compliance requirements. See [Regulatory Compliance](../regulatory-compliance/).

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003. Chapter 14: Maintaining Model Integrity (Published Language, Shared Kernel, Anti-Corruption Layer).
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013. Chapter 13: Integrating Bounded Contexts.
- Doerr, John. "Measure What Matters: How Google, Bono, and the Gates Foundation Rock the World with OKRs." Portfolio/Penguin, 2018.
- Khononov, Vlad. "Learning Domain-Driven Design." O'Reilly, 2021. Chapter 10: Design Heuristics for Integration.
- Hohpe, Gregor and Woolf, Bobby. "Enterprise Integration Patterns." Addison-Wesley, 2003.
