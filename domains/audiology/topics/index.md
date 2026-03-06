# Audiology Domain - Topics

## Overview

This directory contains detailed documentation for each topic within the audiology domain-driven design model. Topics are organized into four categories: strategic design patterns, tactical design patterns, bounded context details, and cross-cutting concerns.

## Strategic Design

- [Strategic Design](strategic-design/index.md) - High-level decomposition of the audiology domain into bounded contexts and subdomains
- [Bounded Contexts](bounded-contexts/index.md) - Logical boundaries where specific audiology models and terminology are consistent
- [Context Map](context-map/index.md) - Relationships and interactions between the six audiology bounded contexts
- [Ubiquitous Language](ubiquitous-language/index.md) - Shared terminology between audiologists, developers, and stakeholders
- [Subdomains](subdomains/index.md) - Classification of audiology areas by strategic importance
- [Anti-Corruption Layer](anti-corruption-layer/index.md) - Translation boundaries protecting audiology contexts from external systems

## Tactical Design

- [Aggregates](aggregates/index.md) - Clusters of related audiology objects treated as consistency units
- [Entities](entities/index.md) - Audiology objects with unique identity persisting over time
- [Value Objects](value-objects/index.md) - Immutable audiology concepts defined by their attributes
- [Domain Events](domain-events/index.md) - Significant occurrences within audiology workflows
- [Repositories](repositories/index.md) - Abstractions for storing and retrieving audiology aggregates
- [Domain Services](domain-services/index.md) - Stateless operations spanning multiple audiology aggregates

## Bounded Context Details

- [Hearing Assessment Context](hearing-assessment-context/index.md) - Diagnostic evaluation and audiometric testing
- [Device Management Context](device-management-context/index.md) - Hearing aids, cochlear implants, fitting, and maintenance
- [Rehabilitation Context](rehabilitation-context/index.md) - Aural rehabilitation, auditory training, and counseling
- [Vestibular Services Context](vestibular-services-context/index.md) - Balance assessment and vestibular rehabilitation
- [Pediatric Audiology Context](pediatric-audiology-context/index.md) - Newborn screening and pediatric hearing care
- [Clinical Documentation Context](clinical-documentation-context/index.md) - Audiograms, reports, referrals, and outcome tracking

## Cross-Cutting Concerns

- [Event-Driven Architecture](event-driven-architecture/index.md) - Communication between bounded contexts through domain events
- [Integration Patterns](integration-patterns/index.md) - Patterns for connecting audiology bounded contexts
- [Audiology Standards](audiology-standards/index.md) - ANSI, ASHA, and AAA standards informing the domain model
- [Regulatory Compliance](regulatory-compliance/index.md) - HIPAA, FDA, and governance requirements

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media.
