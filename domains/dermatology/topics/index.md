# Dermatology Domain - Topics

This directory contains comprehensive documentation for each topic in the dermatology Domain-Driven Design model. Topics are organized into strategic design, tactical design, bounded context details, and cross-cutting concerns.

## Strategic Design

- [Strategic Design](strategic-design/index.md) - High-level decomposition of the dermatology domain into bounded contexts and subdomains.
- [Bounded Contexts](bounded-contexts/index.md) - Logical boundaries defining consistent models and terminology for each area of dermatology practice.
- [Context Map](context-map/index.md) - Relationships and interactions between the six bounded contexts in the dermatology domain.
- [Ubiquitous Language](ubiquitous-language/index.md) - Shared terminology between dermatologists, pathologists, clinical staff, and developers.
- [Subdomains](subdomains/index.md) - Classification of dermatology domain areas by strategic importance.
- [Anti-Corruption Layer](anti-corruption-layer/index.md) - Translation boundaries protecting bounded contexts from external system concepts.

## Tactical Design

- [Aggregates](aggregates/index.md) - Clusters of related dermatology objects treated as single consistency units.
- [Entities](entities/index.md) - Objects with unique identity persisting over time across the dermatology domain.
- [Value Objects](value-objects/index.md) - Immutable objects defined by their attributes in dermatology contexts.
- [Domain Events](domain-events/index.md) - Significant occurrences that trigger actions across dermatology bounded contexts.
- [Repositories](repositories/index.md) - Abstractions for storing and retrieving dermatology aggregate roots.
- [Domain Services](domain-services/index.md) - Stateless operations spanning multiple dermatology aggregates.

## Bounded Context Details

- [Clinical Assessment Context](clinical-assessment-context/index.md) - Skin examination, dermoscopy, lesion documentation, and classification.
- [Lesion Management Context](lesion-management-context/index.md) - Lesion tracking, biopsy management, excision planning, and follow-up.
- [Procedure Management Context](procedure-management-context/index.md) - Mohs surgery, cryotherapy, laser therapy, and phototherapy.
- [Cosmetic Services Context](cosmetic-services-context/index.md) - Aesthetic consultations, injectables, and skin rejuvenation.
- [Pathology Integration Context](pathology-integration-context/index.md) - Specimen tracking, histopathology reports, and staging.
- [Treatment Outcomes Context](treatment-outcomes-context/index.md) - Treatment effectiveness, wound healing, and recurrence tracking.

## Cross-Cutting Concerns

- [Event-Driven Architecture](event-driven-architecture/index.md) - Communication between dermatology bounded contexts through domain events.
- [Integration Patterns](integration-patterns/index.md) - Shared kernel, published language, and open host service patterns.
- [Dermatology Standards](dermatology-standards/index.md) - ICD-10, CPT, SNOMED CT, and clinical classification systems.
- [Regulatory Compliance](regulatory-compliance/index.md) - HIPAA, FDA, state licensing, and clinical documentation requirements.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
