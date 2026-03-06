# Pulmonolgy Domain - Topics

## Strategic Design Topics

- [Strategic Design](strategic-design/index.md) - How to decompose the pulmonolgy domain into manageable bounded contexts using DDD strategic patterns.
- [Bounded Contexts](bounded-contexts/index.md) - The six logical boundaries within pulmonolgy where specific models and terminology are consistent.
- [Context Map](context-map/index.md) - Relationships and interactions between the pulmonolgy bounded contexts.
- [Ubiquitous Language](ubiquitous-language/index.md) - The shared terminology used by pulmonologists, developers, and stakeholders.
- [Subdomains](subdomains/index.md) - Classification of pulmonolgy areas by strategic importance: core, supporting, and generic.
- [Anti-Corruption Layer](anti-corruption-layer/index.md) - Translation boundaries that protect bounded contexts from external system concepts.

## Tactical Design Topics

- [Aggregates](aggregates/index.md) - Clusters of related objects treated as single units maintaining consistency within pulmonolgy contexts.
- [Entities](entities/index.md) - Objects with unique identity persisting over time within the pulmonolgy domain.
- [Value Objects](value-objects/index.md) - Immutable objects defined by their attributes, representing pulmonary measurements and clinical concepts.
- [Domain Events](domain-events/index.md) - Significant occurrences within the pulmonolgy domain that trigger actions across contexts.
- [Repositories](repositories/index.md) - Abstractions for storing and retrieving aggregate roots in each bounded context.
- [Domain Services](domain-services/index.md) - Stateless operations spanning multiple aggregates within pulmonolgy contexts.

## Bounded Context Detail Topics

- [Pulmonary Assessment Context](pulmonary-assessment-context/index.md) - Pulmonary function testing, chest imaging interpretation, and symptom evaluation.
- [Respiratory Diagnostics Context](respiratory-diagnostics-context/index.md) - Spirometry, DLCO, bronchoscopy, thoracentesis, and lung biopsy procedures.
- [Chronic Disease Management Context](chronic-disease-management-context/index.md) - COPD, asthma, ILD, pulmonary hypertension, and cystic fibrosis care.
- [Critical Care Context](critical-care-context/index.md) - Mechanical ventilation, ARDS management, ICU protocols, and weaning.
- [Sleep Medicine Context](sleep-medicine-context/index.md) - Sleep studies, OSA management, CPAP/BiPAP therapy, and narcolepsy.
- [Pulmonary Rehabilitation Context](pulmonary-rehabilitation-context/index.md) - Exercise training, breathing techniques, education, and self-management.

## Cross-Cutting Concern Topics

- [Event-Driven Architecture](event-driven-architecture/index.md) - Communication between bounded contexts through domain events.
- [Integration Patterns](integration-patterns/index.md) - Shared kernel, published language, open host service, and customer-supplier patterns.
- [Pulmonolgy Standards](pulmonolgy-standards/index.md) - Clinical guidelines and standards that inform value object design and published languages.
- [Regulatory Compliance](regulatory-compliance/index.md) - Legal and governance requirements shaping the pulmonolgy domain model.
