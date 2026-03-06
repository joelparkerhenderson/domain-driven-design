# Rheumatology Domain - Topics

## Strategic Design

- [Strategic Design](strategic-design/) - How to decompose the rheumatology domain into manageable bounded contexts using strategic DDD patterns.
- [Bounded Contexts](bounded-contexts/) - The six bounded contexts that define logical boundaries within the rheumatology domain.
- [Context Map](context-map/) - Relationships and interactions between the rheumatology bounded contexts.
- [Ubiquitous Language](ubiquitous-language/) - Shared terminology between rheumatology domain experts and developers.
- [Subdomains](subdomains/) - Classification of rheumatology domain areas by strategic importance.
- [Anti-Corruption Layer](anti-corruption-layer/) - Translation boundaries protecting bounded contexts from external system concepts.

## Tactical Design

- [Aggregates](aggregates/) - Clusters of related objects treated as single units within rheumatology contexts.
- [Entities](entities/) - Objects with unique identity that persist over time in rheumatology workflows.
- [Value Objects](value-objects/) - Immutable objects defined by their attributes in rheumatology clinical data.
- [Domain Events](domain-events/) - Significant occurrences within the rheumatology domain that trigger actions.
- [Repositories](repositories/) - Abstractions for storing and retrieving rheumatology aggregate roots.
- [Domain Services](domain-services/) - Stateless operations spanning multiple aggregates in rheumatology contexts.

## Bounded Context Details

- [Clinical Assessment Context](clinical-assessment-context/) - Joint examination, serology, and disease activity scoring.
- [Autoimmune Disease Context](autoimmune-disease-context/) - RA, lupus, scleroderma, vasculitis, and Sjogren's disease management.
- [Joint Disease Context](joint-disease-context/) - Osteoarthritis, gout, crystal arthropathy, and joint injection therapy.
- [Biologic Therapy Context](biologic-therapy-context/) - TNF inhibitors, IL-6 blockers, JAK inhibitors, biosimilars, infusion management.
- [Pain Management Context](pain-management-context/) - Multimodal pain strategies, physical therapy, occupational therapy.
- [Outcomes Tracking Context](outcomes-tracking-context/) - DAS28, HAQ-DI, radiographic progression, patient-reported outcomes.

## Cross-Cutting Concerns

- [Event-Driven Architecture](event-driven-architecture/) - Communication between bounded contexts through domain events.
- [Integration Patterns](integration-patterns/) - Shared kernel, published language, open host service patterns.
- [Rheumatology Standards](rheumatology-standards/) - ACR, EULAR, and OMERACT standards informing domain model design.
- [Regulatory Compliance](regulatory-compliance/) - HIPAA, FDA, and EMA requirements shaping domain model constraints.
