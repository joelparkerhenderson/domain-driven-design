# Ophthalmology Domain - Topics Index

## Strategic Design Patterns

Strategic design addresses how to decompose the ophthalmology system into manageable
bounded contexts, each with clear responsibilities and well-defined integration points.

- [Strategic Design](strategic-design/) - Overview of strategic DDD patterns applied to ophthalmology
- [Bounded Contexts](bounded-contexts/) - Logical boundaries for ophthalmology subsystems
- [Context Map](context-map/) - Relationships and interactions between bounded contexts
- [Ubiquitous Language](ubiquitous-language/) - Shared terminology across ophthalmology stakeholders
- [Subdomains](subdomains/) - Classification of domain areas by strategic importance
- [Anti-Corruption Layer](anti-corruption-layer/) - Translation boundaries for external system integration

## Tactical Design Patterns

Tactical patterns define how business logic is structured within each bounded context,
using entities, value objects, aggregates, events, repositories, and services.

- [Aggregates](aggregates/) - Clusters of domain objects treated as consistency units
- [Entities](entities/) - Objects with unique identity persisting over time
- [Value Objects](value-objects/) - Immutable objects defined by their attributes
- [Domain Events](domain-events/) - Significant occurrences triggering cross-context actions
- [Repositories](repositories/) - Abstractions for aggregate persistence and retrieval
- [Domain Services](domain-services/) - Stateless operations spanning multiple aggregates

## Bounded Context Documentation

Each bounded context is documented with its internal model, aggregates, entities,
value objects, domain events, and integration points.

- [Clinical Examination Context](clinical-examination-context/) - Visual acuity, slit lamp, tonometry, refraction
- [Diagnostic Imaging Context](diagnostic-imaging-context/) - OCT, fundus photography, angiography, visual fields
- [Surgical Management Context](surgical-management-context/) - Cataract surgery, vitrectomy, corneal transplant
- [Refractive Services Context](refractive-services-context/) - LASIK, PRK, lens implants, pre/post-op management
- [Glaucoma Management Context](glaucoma-management-context/) - IOP monitoring, visual field progression, treatment
- [Retinal Care Context](retinal-care-context/) - Diabetic retinopathy, AMD, retinal detachment, injections

## Cross-Cutting Concerns

Cross-cutting concerns address communication, integration, standards, and compliance
requirements that span multiple bounded contexts.

- [Event-Driven Architecture](event-driven-architecture/) - Communication through domain events
- [Integration Patterns](integration-patterns/) - Shared kernel, published language, open host service
- [Ophthalmology Standards](opthamology-standards/) - DICOM, ICD-10, CPT, HL7 FHIR, and clinical standards
- [Regulatory Compliance](regulatory-compliance/) - HIPAA, FDA, clinical trial requirements
