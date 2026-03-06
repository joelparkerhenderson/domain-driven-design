# Hormone Replacement Therapy Domain - Topics

This directory contains detailed documentation for each topic within the hormone replacement therapy DDD domain.

## Strategic Design

- [Strategic Design](strategic-design/) - Overview of strategic design patterns applied to the HRT domain
- [Bounded Contexts](bounded-contexts/) - Logical boundaries and their domain models
- [Context Map](context-map/) - Relationships and interactions between bounded contexts
- [Ubiquitous Language](ubiquitous-language/) - Shared terminology across the HRT domain
- [Subdomains](subdomains/) - Classification of domain areas by strategic importance
- [Anti-Corruption Layer](anti-corruption-layer/) - Translation boundaries protecting bounded contexts

## Tactical Design

- [Aggregates](aggregates/) - Clusters of related objects maintaining consistency
- [Entities](entities/) - Objects with unique identity persisting over time
- [Value Objects](value-objects/) - Immutable objects defined by their attributes
- [Domain Events](domain-events/) - Significant occurrences triggering cross-context actions
- [Repositories](repositories/) - Abstractions for aggregate persistence
- [Domain Services](domain-services/) - Stateless operations spanning multiple aggregates

## Bounded Context Documentation

- [Clinical Assessment Context](clinical-assessment-context/) - Symptom evaluation and candidacy screening
- [Hormone Protocol Context](hormone-protocol-context/) - HRT regimens, delivery methods, and dosing
- [Lab Monitoring Context](lab-monitoring-context/) - Hormone levels, metabolic panels, and safety markers
- [Side Effect Management Context](side-effect-management-context/) - Adverse events and risk mitigation
- [Treatment Optimization Context](treatment-optimization-context/) - Dose titration and formulation adjustment
- [Outcomes Tracking Context](outcomes-tracking-context/) - Symptom resolution and quality of life measurement

## Cross-Cutting Concerns

- [Event-Driven Architecture](event-driven-architecture/) - Communication patterns between bounded contexts
- [Integration Patterns](integration-patterns/) - Shared kernel, published language, and other integration approaches
- [Hormone Replacement Therapy Standards](hormone-replacement-therapy-standards/) - Clinical and pharmacological standards
- [Regulatory Compliance](regulatory-compliance/) - Legal, governance, and regulatory requirements

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
