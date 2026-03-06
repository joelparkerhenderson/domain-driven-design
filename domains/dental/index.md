# Dental Domain - Domain-Driven Design

## Overview

The dental domain applies Domain-Driven Design (DDD) principles to model the systems and workflows of dental practice operations. Dental practices integrate clinical care delivery with administrative management, creating a rich domain that benefits from careful bounded context definition and ubiquitous language alignment between dental professionals and software teams.

This domain covers clinical examination, treatment planning, restorative services, orthodontic management, periodontal care, and practice management. Each area maps to a bounded context with its own aggregate roots, entities, value objects, and domain events.

## Bounded Contexts

1. **Clinical Examination Context** - Oral examination, radiography, dental charting, caries detection, and periodontal probing. This context owns the patient's diagnostic record.

2. **Treatment Planning Context** - Treatment sequencing, case presentation, informed consent, and cost estimation. This context translates clinical findings into actionable plans.

3. **Restorative Services Context** - Fillings, crowns, bridges, implants, and endodontic procedures. This context manages restorative procedure execution and outcomes.

4. **Orthodontic Management Context** - Malocclusion assessment, braces and aligner management, retention planning, and progress tracking over extended treatment timelines.

5. **Periodontal Care Context** - Scaling and root planing, pocket depth tracking, and maintenance scheduling for periodontal disease management.

6. **Practice Management Context** - Appointment scheduling, insurance verification, claims processing, and patient recall systems supporting clinical operations.

## Topics

- [Strategic Design](topics/strategic-design/)
- [Bounded Contexts](topics/bounded-contexts/)
- [Context Map](topics/context-map/)
- [Ubiquitous Language](topics/ubiquitous-language/)
- [Subdomains](topics/subdomains/)
- [Anti-Corruption Layer](topics/anti-corruption-layer/)
- [Aggregates](topics/aggregates/)
- [Entities](topics/entities/)
- [Value Objects](topics/value-objects/)
- [Domain Events](topics/domain-events/)
- [Repositories](topics/repositories/)
- [Domain Services](topics/domain-services/)
- [Clinical Examination Context](topics/clinical-examination-context/)
- [Treatment Planning Context](topics/treatment-planning-context/)
- [Restorative Services Context](topics/restorative-services-context/)
- [Orthodontic Management Context](topics/orthodontic-management-context/)
- [Periodontal Care Context](topics/periodontal-care-context/)
- [Practice Management Context](topics/practice-management-context/)
- [Event-Driven Architecture](topics/event-driven-architecture/)
- [Integration Patterns](topics/integration-patterns/)
- [Dental Standards](topics/dental-standards/)
- [Regulatory Compliance](topics/regulatory-compliance/)

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- American Dental Association. Current Dental Terminology (CDT). ADA, updated annually.
- U.S. Department of Health and Human Services. HIPAA Privacy Rule. 45 CFR Part 164.
