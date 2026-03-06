# Sleep Quality Domain - Topics

## Strategic Design

- [Strategic Design](strategic-design/index.md) - High-level approach to decomposing the sleep quality domain into bounded contexts and subdomains.
- [Bounded Contexts](bounded-contexts/index.md) - Logical boundaries within the sleep quality domain where specific models and terminology are consistent.
- [Context Map](context-map/index.md) - Relationships and interactions between bounded contexts in the sleep quality domain.
- [Ubiquitous Language](ubiquitous-language/index.md) - Shared vocabulary used by domain experts, developers, and stakeholders across the sleep quality domain.
- [Subdomains](subdomains/index.md) - Classification of sleep quality domain areas by strategic importance.
- [Anti-Corruption Layer](anti-corruption-layer/index.md) - Translation boundaries that protect bounded contexts from external system concepts.

## Tactical Design

- [Aggregates](aggregates/index.md) - Clusters of related domain objects treated as single units of consistency.
- [Entities](entities/index.md) - Domain objects with unique identities that persist over time.
- [Value Objects](value-objects/index.md) - Immutable objects defined by their attributes rather than identity.
- [Domain Events](domain-events/index.md) - Significant occurrences within the domain that trigger actions across contexts.
- [Repositories](repositories/index.md) - Abstractions for storing and retrieving aggregate roots.
- [Domain Services](domain-services/index.md) - Stateless operations that span multiple aggregates.

## Bounded Context Details

- [Sleep Assessment Context](sleep-assessment-context/index.md) - Sleep diaries, questionnaires, actigraphy, and polysomnography.
- [Sleep Disorders Context](sleep-disorders-context/index.md) - Insomnia, sleep apnea, restless legs syndrome, narcolepsy, and parasomnias.
- [Sleep Environment Context](sleep-environment-context/index.md) - Bedroom optimization, light, noise, temperature, and bedding.
- [Behavioral Interventions Context](behavioral-interventions-context/index.md) - CBT-I, sleep restriction, stimulus control, and relaxation techniques.
- [Device Integration Context](device-integration-context/index.md) - Wearables, smart mattresses, CPAP data, and ambient sensors.
- [Outcomes Tracking Context](outcomes-tracking-context/index.md) - Sleep efficiency trends, quality scores, daytime functioning, and satisfaction.

## Cross-Cutting Concerns

- [Event-Driven Architecture](event-driven-architecture/index.md) - Communication between bounded contexts through domain events.
- [Integration Patterns](integration-patterns/index.md) - Shared kernel, published language, open host service, and customer-supplier patterns.
- [Sleep Quality Standards](sleep-quality-standards/index.md) - Domain-specific standards from AASM, ICSD-3, PSQI, and other clinical frameworks.
- [Regulatory Compliance](regulatory-compliance/index.md) - HIPAA, FDA, and governance requirements that shape the domain model.
