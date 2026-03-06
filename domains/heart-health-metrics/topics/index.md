# Heart Health Metrics - Topics

This directory contains all topic documentation for the heart health metrics domain, organized by category.

## Strategic Design Topics

Strategic design patterns address how to decompose the heart health metrics system into manageable bounded contexts with clear responsibilities and well-defined interfaces.

- [Strategic Design](strategic-design/index.md) - Overall strategic design approach for cardiac health monitoring systems.
- [Bounded Contexts](bounded-contexts/index.md) - Logical boundaries for cardiac data models and terminology.
- [Context Map](context-map/index.md) - Relationships and interactions between heart health bounded contexts.
- [Ubiquitous Language](ubiquitous-language/index.md) - Shared cardiac health terminology among domain experts and developers.
- [Subdomains](subdomains/index.md) - Classification of cardiac health areas by strategic importance.
- [Anti-Corruption Layer](anti-corruption-layer/index.md) - Translation boundaries protecting cardiac domain models from external systems.

## Tactical Design Topics

Tactical patterns define how business logic is structured within each bounded context of the heart health metrics domain.

- [Aggregates](aggregates/index.md) - Clusters of cardiac health objects treated as consistency units.
- [Entities](entities/index.md) - Objects with unique identity that persist across cardiac monitoring sessions.
- [Value Objects](value-objects/index.md) - Immutable cardiac measurements and clinical parameters.
- [Domain Events](domain-events/index.md) - Significant occurrences in cardiac monitoring that trigger downstream actions.
- [Repositories](repositories/index.md) - Abstractions for storing and retrieving cardiac data aggregates.
- [Domain Services](domain-services/index.md) - Stateless operations spanning multiple cardiac health aggregates.

## Bounded Context Topics

Each bounded context in the heart health metrics domain has its own detailed documentation describing internal models, interfaces, and integration points.

- [Data Collection Context](data-collection-context/index.md) - Sensor and device integration for cardiac data acquisition.
- [Cardiac Analysis Context](cardiac-analysis-context/index.md) - Signal processing, rhythm analysis, and HRV computation.
- [Risk Assessment Context](risk-assessment-context/index.md) - Cardiovascular risk scoring and comorbidity evaluation.
- [Monitoring & Alerts Context](monitoring-alerts-context/index.md) - Real-time surveillance and threshold-based alerting.
- [Reporting & Visualization Context](reporting-visualization-context/index.md) - Trend reports, dashboards, and clinical summaries.
- [Clinical Integration Context](clinical-integration-context/index.md) - EHR integration, FHIR exchange, and clinical decision support.

## Cross-Cutting Concern Topics

Cross-cutting concerns span multiple bounded contexts and address architecture-wide patterns.

- [Event-Driven Architecture](event-driven-architecture/index.md) - Asynchronous communication between cardiac health contexts.
- [Integration Patterns](integration-patterns/index.md) - Inter-context and external system communication strategies.
- [Heart Health Metrics Standards](heart-health-metrics-standards/index.md) - Clinical and technical standards for cardiac data systems.
- [Regulatory Compliance](regulatory-compliance/index.md) - Legal, privacy, and medical device regulatory requirements.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly, 2021.
