# Asthma Domain - Topics

## Strategic Design

- [Strategic Design](strategic-design/index.md) - High-level decomposition of the asthma management domain into bounded contexts and subdomains.
- [Bounded Contexts](bounded-contexts/index.md) - The six logical boundaries that partition the asthma domain model.
- [Context Map](context-map/index.md) - Relationships and interactions between bounded contexts.
- [Ubiquitous Language](ubiquitous-language/index.md) - Shared vocabulary across all asthma management stakeholders.
- [Subdomains](subdomains/index.md) - Classification of domain areas by strategic importance.
- [Anti-Corruption Layer](anti-corruption-layer/index.md) - Translation boundaries protecting bounded contexts from external system concepts.

## Tactical Design

- [Aggregates](aggregates/index.md) - Clusters of related objects treated as consistency units.
- [Entities](entities/index.md) - Objects with unique identity that persist over time.
- [Value Objects](value-objects/index.md) - Immutable objects defined by their attributes.
- [Domain Events](domain-events/index.md) - Significant occurrences that trigger actions across contexts.
- [Repositories](repositories/index.md) - Abstractions for storing and retrieving aggregate roots.
- [Domain Services](domain-services/index.md) - Stateless operations spanning multiple aggregates.

## Bounded Context Details

- [Clinical Assessment Context](clinical-assessment-context/index.md) - Spirometry, peak flow, FeNO testing, severity classification.
- [Treatment Management Context](treatment-management-context/index.md) - Stepwise therapy, biologic agents, immunotherapy, inhaler technique.
- [Trigger Monitoring Context](trigger-monitoring-context/index.md) - Allergen tracking, air quality, environmental factors.
- [Action Planning Context](action-planning-context/index.md) - Asthma action plans, zone system, emergency protocols.
- [Medication Management Context](medication-management-context/index.md) - Controller medications, rescue inhalers, adherence monitoring.
- [Outcomes Tracking Context](outcomes-tracking-context/index.md) - ACT scores, exacerbation rates, lung function trends.

## Cross-Cutting Concerns

- [Event-Driven Architecture](event-driven-architecture/index.md) - Communication between bounded contexts through domain events.
- [Integration Patterns](integration-patterns/index.md) - Shared kernel, published language, open host service patterns.
- [Asthma Standards](asthma-standards/index.md) - Clinical standards (GINA, NAEPP) informing domain model design.
- [Regulatory Compliance](regulatory-compliance/index.md) - HIPAA, FDA, and governance requirements shaping the domain.
