# Gerontology Domain - Topics

## Strategic Design

- [Strategic Design](strategic-design/) - High-level decomposition of the gerontology domain into bounded contexts and subdomains.
- [Bounded Contexts](bounded-contexts/) - Logical boundaries where specific models and terminology are consistent within the gerontology domain.
- [Context Map](context-map/) - Relationships and interactions between the six bounded contexts in geriatric care.
- [Ubiquitous Language](ubiquitous-language/) - Shared terminology between geriatricians, nurses, social workers, caregivers, and developers.
- [Subdomains](subdomains/) - Classification of gerontology domain areas by strategic importance (core, supporting, generic).
- [Anti-Corruption Layer](anti-corruption-layer/) - Translation boundaries protecting bounded contexts from external system concepts.

## Tactical Design

- [Aggregates](aggregates/) - Clusters of related objects treated as single units maintaining consistency within geriatric care.
- [Entities](entities/) - Objects with unique identity that persist over time in geriatric care workflows.
- [Value Objects](value-objects/) - Immutable objects defined by attributes representing clinical measurements, scores, and classifications.
- [Domain Events](domain-events/) - Significant occurrences within the gerontology domain that trigger actions across bounded contexts.
- [Repositories](repositories/) - Abstractions for storing and retrieving aggregate roots in geriatric care systems.
- [Domain Services](domain-services/) - Stateless operations spanning multiple aggregates within and across geriatric care contexts.

## Bounded Context Detail

- [Geriatric Assessment Context](geriatric-assessment-context/) - Comprehensive geriatric assessment, frailty screening, and fall risk evaluation.
- [Care Coordination Context](care-coordination-context/) - Interdisciplinary team management, care transitions, and caregiver support.
- [Cognitive Health Context](cognitive-health-context/) - Dementia screening, cognitive testing, and memory care planning.
- [Functional Independence Context](functional-independence-context/) - ADL/IADL assessment, mobility aids, and home modification planning.
- [Medication Management Context](medication-management-context/) - Polypharmacy review, Beers criteria, and deprescribing protocols.
- [End of Life Planning Context](end-of-life-planning-context/) - Advance directives, palliative care, and hospice coordination.

## Cross-Cutting Concerns

- [Event-Driven Architecture](event-driven-architecture/) - Communication between bounded contexts through domain events.
- [Integration Patterns](integration-patterns/) - Shared kernel, published language, and open host service patterns in geriatric care.
- [Gerontology Standards](gerontology-standards/) - Clinical standards and assessment instruments that inform domain model design.
- [Regulatory Compliance](regulatory-compliance/) - Legal and governance requirements shaping the gerontology domain model.
