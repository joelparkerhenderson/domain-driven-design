# Otolaryngology Domain - Domain-Driven Design

## Overview

This domain applies Domain-Driven Design (DDD) to otolaryngology (ENT: ear, nose, throat) systems. Otolaryngology encompasses the diagnosis and management of disorders of the head and neck, including the ears, nasal cavity, sinuses, pharynx, larynx, thyroid, and related structures. The domain model captures clinical workflows, surgical management, subspecialty services, and cross-cutting concerns using DDD strategic and tactical patterns.

## Bounded Contexts

| Context | Description |
|---------|-------------|
| [Clinical Assessment](topics/clinical-assessment-context/index.md) | Head/neck examination, endoscopy, audiometry, imaging |
| [Surgical Management](topics/surgical-management-context/index.md) | Tonsillectomy, septoplasty, sinus surgery, thyroidectomy, head/neck oncology |
| [Voice Disorders](topics/voice-disorders-context/index.md) | Laryngoscopy, voice therapy, vocal fold pathology, phonosurgery |
| [Sleep Disorders](topics/sleep-disorders-context/index.md) | Sleep apnea evaluation, CPAP management, surgical interventions (UPPP) |
| [Allergy Management](topics/allergy-management-context/index.md) | Allergy testing, immunotherapy, rhinitis management, sinusitis |
| [Hearing Services](topics/hearing-services-context/index.md) | Hearing evaluation, hearing aid fitting, cochlear implant evaluation |

## Strategic Design Topics

- [Strategic Design](topics/strategic-design/index.md)
- [Bounded Contexts](topics/bounded-contexts/index.md)
- [Context Map](topics/context-map/index.md)
- [Ubiquitous Language](topics/ubiquitous-language/index.md)
- [Subdomains](topics/subdomains/index.md)
- [Anti-Corruption Layer](topics/anti-corruption-layer/index.md)

## Tactical Design Topics

- [Aggregates](topics/aggregates/index.md)
- [Entities](topics/entities/index.md)
- [Value Objects](topics/value-objects/index.md)
- [Domain Events](topics/domain-events/index.md)
- [Repositories](topics/repositories/index.md)
- [Domain Services](topics/domain-services/index.md)

## Cross-Cutting Concerns

- [Event-Driven Architecture](topics/event-driven-architecture/index.md)
- [Integration Patterns](topics/integration-patterns/index.md)
- [Otolaryngology Standards](topics/otolaryngology-standards/index.md)
- [Regulatory Compliance](topics/regulatory-compliance/index.md)

## Project Files

- [CLAUDE.md](CLAUDE.md) - Project instructions
- [plan.md](plan.md) - Strategic plan
- [tasks.md](tasks.md) - Task tracking
- [ubiquitous-language.md](ubiquitous-language.md) - Shared terminology
- [topics/](topics/index.md) - Detailed topic documentation

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- Flint, Paul W., et al. *Cummings Otolaryngology: Head and Neck Surgery*. 7th ed., Elsevier, 2020.
- American Academy of Otolaryngology-Head and Neck Surgery. Clinical Practice Guidelines. https://www.entnet.org.
