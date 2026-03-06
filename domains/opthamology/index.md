# Ophthalmology Domain - Domain-Driven Design

## Overview

This domain applies Domain-Driven Design (DDD) to ophthalmology systems. Ophthalmology
encompasses the medical and surgical care of the eye, covering clinical examination,
diagnostic imaging, surgical management, refractive services, glaucoma management, and
retinal care. DDD provides the modeling framework to manage the inherent complexity of
these interconnected clinical workflows while maintaining clear boundaries and shared
understanding across all stakeholders.

## Bounded Contexts

The ophthalmology domain is decomposed into six bounded contexts, each representing a
distinct area of clinical responsibility with its own internal model and terminology.

1. **Clinical Examination Context** - Visual acuity testing, slit lamp examination,
   tonometry, refraction, and comprehensive eye examinations.

2. **Diagnostic Imaging Context** - Optical coherence tomography (OCT), fundus
   photography, fluorescein angiography, and visual field testing.

3. **Surgical Management Context** - Cataract surgery, vitrectomy, corneal transplant,
   oculoplastic procedures, and perioperative management.

4. **Refractive Services Context** - LASIK, PRK, implantable lens procedures, and
   pre/post-operative management for refractive correction.

5. **Glaucoma Management Context** - Intraocular pressure monitoring, visual field
   progression analysis, medical and surgical treatment of glaucoma.

6. **Retinal Care Context** - Diabetic retinopathy, age-related macular degeneration,
   retinal detachment, and intravitreal injection protocols.

## Subdomains

- **Core Subdomain**: Clinical Examination, Surgical Management - these represent the
  highest-value, most complex areas requiring deep domain expertise.
- **Supporting Subdomain**: Diagnostic Imaging, Glaucoma Management, Retinal Care -
  these provide essential clinical capabilities that support core operations.
- **Generic Subdomain**: Refractive Services - well-established protocols and workflows
  that can leverage standardized approaches.

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
- [Diagnostic Imaging Context](topics/diagnostic-imaging-context/)
- [Surgical Management Context](topics/surgical-management-context/)
- [Refractive Services Context](topics/refractive-services-context/)
- [Glaucoma Management Context](topics/glaucoma-management-context/)
- [Retinal Care Context](topics/retinal-care-context/)
- [Event-Driven Architecture](topics/event-driven-architecture/)
- [Integration Patterns](topics/integration-patterns/)
- [Ophthalmology Standards](topics/opthamology-standards/)
- [Regulatory Compliance](topics/regulatory-compliance/)

## Key Documentation

- [Plan](plan.md) - Strategic plan for the domain
- [Tasks](tasks.md) - Task tracking for domain build-out
- [Ubiquitous Language](ubiquitous-language.md) - Shared terminology
- [Topics Index](topics/index.md) - All topic documentation

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- American Academy of Ophthalmology. *Basic and Clinical Science Course (BCSC)*.
- International Council of Ophthalmology. *Clinical Guidelines*.
