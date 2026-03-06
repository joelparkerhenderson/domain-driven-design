# Otolaryngology Domain - Topics

## Overview

This directory contains detailed documentation for each DDD concept and bounded context applied to the otolaryngology domain. Topics are organized into three categories: strategic design, tactical design, and bounded context documentation, plus cross-cutting concerns.

## Strategic Design

- [Strategic Design](strategic-design/index.md) - High-level approach to decomposing the otolaryngology domain
- [Bounded Contexts](bounded-contexts/index.md) - Logical boundaries for each ENT subspecialty area
- [Context Map](context-map/index.md) - Relationships and interactions between bounded contexts
- [Ubiquitous Language](ubiquitous-language/index.md) - Shared terminology across all contexts
- [Subdomains](subdomains/index.md) - Classification by strategic importance (core, supporting, generic)
- [Anti-Corruption Layer](anti-corruption-layer/index.md) - Translation boundaries with external systems

## Tactical Design

- [Aggregates](aggregates/index.md) - Clusters of related objects maintaining consistency
- [Entities](entities/index.md) - Objects with unique identity persisting over time
- [Value Objects](value-objects/index.md) - Immutable objects defined by attributes
- [Domain Events](domain-events/index.md) - Significant occurrences triggering cross-context actions
- [Repositories](repositories/index.md) - Abstractions for aggregate persistence
- [Domain Services](domain-services/index.md) - Stateless operations spanning multiple aggregates

## Bounded Context Documentation

- [Clinical Assessment Context](clinical-assessment-context/index.md) - Head/neck examination, endoscopy, audiometry, imaging
- [Surgical Management Context](surgical-management-context/index.md) - ENT surgical procedures and perioperative management
- [Voice Disorders Context](voice-disorders-context/index.md) - Laryngoscopy, voice therapy, vocal fold pathology
- [Sleep Disorders Context](sleep-disorders-context/index.md) - Sleep apnea evaluation, CPAP, surgical interventions
- [Allergy Management Context](allergy-management-context/index.md) - Allergy testing, immunotherapy, rhinitis, sinusitis
- [Hearing Services Context](hearing-services-context/index.md) - Hearing evaluation, hearing aids, cochlear implants

## Cross-Cutting Concerns

- [Event-Driven Architecture](event-driven-architecture/index.md) - Communication between bounded contexts
- [Integration Patterns](integration-patterns/index.md) - Inter-context and external system integration
- [Otolaryngology Standards](otolaryngology-standards/index.md) - Domain-specific clinical and coding standards
- [Regulatory Compliance](regulatory-compliance/index.md) - Legal and governance requirements

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
