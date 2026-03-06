# Dental Domain - Topics

## Strategic Design

- [Strategic Design](strategic-design/) - High-level decomposition of the dental domain into bounded contexts and subdomains.
- [Bounded Contexts](bounded-contexts/) - Logical boundaries where dental models and terminology are consistent.
- [Context Map](context-map/) - Relationships and interactions between the six dental bounded contexts.
- [Ubiquitous Language](ubiquitous-language/) - Shared vocabulary between dental professionals and development teams.
- [Subdomains](subdomains/) - Classification of dental domain areas by strategic importance.
- [Anti-Corruption Layer](anti-corruption-layer/) - Translation boundaries protecting dental contexts from external system concepts.

## Tactical Design

- [Aggregates](aggregates/) - Clusters of dental domain objects treated as consistency units.
- [Entities](entities/) - Dental domain objects with unique identity persisting over time.
- [Value Objects](value-objects/) - Immutable dental domain objects defined by their attributes.
- [Domain Events](domain-events/) - Significant occurrences within the dental domain triggering cross-context actions.
- [Repositories](repositories/) - Abstractions for storing and retrieving dental aggregate roots.
- [Domain Services](domain-services/) - Stateless operations spanning multiple dental aggregates.

## Bounded Context Details

- [Clinical Examination Context](clinical-examination-context/) - Oral examination, radiography, charting, caries detection, periodontal probing.
- [Treatment Planning Context](treatment-planning-context/) - Treatment sequencing, case presentation, informed consent, cost estimation.
- [Restorative Services Context](restorative-services-context/) - Fillings, crowns, bridges, implants, endodontics.
- [Orthodontic Management Context](orthodontic-management-context/) - Malocclusion assessment, braces/aligners, retention, progress tracking.
- [Periodontal Care Context](periodontal-care-context/) - Scaling/root planing, pocket depth tracking, maintenance schedules.
- [Practice Management Context](practice-management-context/) - Scheduling, insurance verification, claims processing, patient recall.

## Cross-Cutting Concerns

- [Event-Driven Architecture](event-driven-architecture/) - Communication between dental bounded contexts through domain events.
- [Integration Patterns](integration-patterns/) - Shared kernel, published language, and open host service patterns.
- [Dental Standards](dental-standards/) - ADA CDT codes, ISO standards, and clinical classification systems.
- [Regulatory Compliance](regulatory-compliance/) - HIPAA, state dental board regulations, and informed consent requirements.
