# Hearing Aid Domain - AGENTS.md

## Overview

This domain applies Domain-Driven Design (DDD) to hearing aid systems, encompassing audiological assessment, device selection and fitting, hearing aid programming, patient rehabilitation, device maintenance, and regulatory compliance.

## Bounded Contexts

1. Audiological Assessment Context - pure tone audiometry, speech audiometry, tympanometry, otoacoustic emissions, auditory brainstem response testing
2. Device Selection and Fitting Context - hearing aid style selection, technology level matching, ear impression and scanning, real-ear measurement verification
3. Hearing Aid Programming Context - electroacoustic parameter configuration, fitting formula application, feedback management, noise reduction settings, directional microphone programming
4. Patient Rehabilitation Context - acclimatization counseling, communication strategies training, assistive listening device recommendation, outcome measurement
5. Device Maintenance Context - repair tracking, warranty management, component replacement, cleaning and servicing schedules, loaner device provisioning

## Domain Principles

- Model using shared ubiquitous language between audiologists, hearing instrument specialists, manufacturers, patients, and system developers.
- Group the system into bounded contexts reflecting hearing healthcare workflow stages from assessment through long-term care.
- Business logic remains pure and independent of infrastructure (database, UI, web, mobile).
- Follow ASHA and AAA clinical practice guidelines for audiological terminology and procedures.
- Maintain HIPAA compliance, FDA device regulations, and patient privacy as cross-cutting concerns.

## Key Topics

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
- audiological-assessment-context
- device-selection-fitting-context
- hearing-aid-programming-context
- patient-rehabilitation-context
- device-maintenance-context
- event-driven-architecture
- integration-patterns
- audiology-standards
- regulatory-compliance

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- Dillon, Harvey, et al. Hearing Aids. Thieme, 2012.
- American Academy of Audiology Clinical Practice Guidelines on the Treatment of Hearing Loss in Adults, 2024.
