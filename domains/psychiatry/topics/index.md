# Psychiatry Domain - Topics

## Strategic Design

- [Strategic Design](strategic-design/index.md) - High-level architectural decisions for decomposing the psychiatry domain into bounded contexts aligned with clinical workflows.
- [Bounded Contexts](bounded-contexts/index.md) - The six bounded contexts that partition the psychiatry domain into cohesive areas of clinical responsibility.
- [Context Map](context-map/index.md) - Relationships and integration patterns between bounded contexts in the psychiatry system.
- [Ubiquitous Language](ubiquitous-language/index.md) - Shared terminology between domain experts and developers ensuring consistent communication across the psychiatry domain.
- [Subdomains](subdomains/index.md) - Classification of psychiatry areas by strategic importance into core, supporting, and generic subdomains.
- [Anti-Corruption Layer](anti-corruption-layer/index.md) - Translation boundaries protecting bounded contexts from external system concepts and terminology drift.

## Tactical Design

- [Aggregates](aggregates/index.md) - Clusters of domain objects treated as single units of consistency within the psychiatry domain.
- [Entities](entities/index.md) - Domain objects with unique identity and lifecycle that persist across psychiatric care episodes.
- [Value Objects](value-objects/index.md) - Immutable objects defined by their attributes, representing clinical measurements, codes, and scores.
- [Domain Events](domain-events/index.md) - Significant clinical occurrences that trigger actions across bounded contexts.
- [Repositories](repositories/index.md) - Abstractions for persisting and retrieving aggregate roots in the psychiatry domain.
- [Domain Services](domain-services/index.md) - Stateless operations spanning multiple aggregates for cross-cutting psychiatric business logic.

## Bounded Context Details

- [Diagnostic Assessment Context](diagnostic-assessment-context/index.md) - Detailed design for psychiatric evaluation, DSM-5-TR diagnosis, and clinical rating scales.
- [Medication Management Context](medication-management-context/index.md) - Detailed design for psychopharmacological treatment and therapeutic drug monitoring.
- [Psychotherapy Context](psychotherapy-context/index.md) - Detailed design for therapeutic modalities, session management, and treatment protocols.
- [Crisis Management Context](crisis-management-context/index.md) - Detailed design for psychiatric emergencies, risk assessment, and safety planning.
- [Rehabilitation Services Context](rehabilitation-services-context/index.md) - Detailed design for psychosocial rehabilitation, vocational support, and community reintegration.
- [Outcomes Measurement Context](outcomes-measurement-context/index.md) - Detailed design for clinical outcomes tracking, functional assessment, and quality metrics.

## Cross-Cutting Concerns

- [Event-Driven Architecture](event-driven-architecture/index.md) - Asynchronous communication patterns between bounded contexts using domain events.
- [Integration Patterns](integration-patterns/index.md) - Strategies for inter-context communication including shared kernel, published language, and open host service.
- [Psychiatry Standards](psychiatry-standards/index.md) - Domain-specific standards (DSM-5-TR, ICD-11, CPT, LOINC) that inform the domain model.
- [Regulatory Compliance](regulatory-compliance/index.md) - Legal and governance requirements (HIPAA, 42 CFR Part 2, state mental health laws) shaping the domain design.
