# Audiology Domain - DDD Project Instructions

This domain applies Domain-Driven Design (DDD) to audiology systems, encompassing hearing assessment, hearing device management, aural rehabilitation, vestibular services, pediatric audiology, and clinical documentation.

## Bounded Contexts

1. Hearing Assessment Context - audiometry, tympanometry, OAE testing, ABR, speech recognition testing
2. Device Management Context - hearing aids, cochlear implants, fitting, programming, maintenance
3. Rehabilitation Context - aural rehabilitation, auditory training, communication strategies, counseling
4. Vestibular Services Context - balance assessment, VNG/ENG testing, vestibular rehabilitation
5. Pediatric Audiology Context - newborn screening, developmental monitoring, pediatric fitting
6. Clinical Documentation Context - audiograms, reports, referral management, outcome measures

## Domain Structure

- `plan.md` - Strategic plan for the audiology DDD project
- `tasks.md` - Task tracking for domain implementation
- `ubiquitous-language.md` - Shared terminology across all bounded contexts
- `index.md` - Domain overview and navigation
- `topics/` - Detailed documentation for each DDD pattern and bounded context

## Topics

- strategic-design
- bounded-contexts
- context-map
- ubiquitous-language
- subdomains
- anti-corruption-layer
- aggregates
- entities
- value-objects
- domain-events
- repositories
- domain-services
- hearing-assessment-context
- device-management-context
- rehabilitation-context
- vestibular-services-context
- pediatric-audiology-context
- clinical-documentation-context
- event-driven-architecture
- integration-patterns
- audiology-standards
- regulatory-compliance

## Guidelines

- Use ubiquitous language consistently across all documentation.
- Model business logic as pure domain logic, independent of infrastructure.
- Each bounded context maintains its own internal consistency.
- Cross-context communication occurs through domain events and integration patterns.
- Comply with HIPAA, FDA, and audiology-specific regulatory standards.
- Do not create images, diagrams, web applications, front-end, or back-end code.
- Provide references and citations to authoritative sources.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software.
- Vernon, V. (2013). Implementing Domain-Driven Design.
- Khononov, V. (2021). Learning Domain-Driven Design.
