# Mobility Domain - AGENTS.md

## Overview

This domain applies Domain-Driven Design (DDD) to mobility systems, encompassing functional mobility assessment, assistive device management, gait analysis, fall prevention, accessibility planning, and rehabilitation program coordination. Mobility as a clinical domain addresses the ability of individuals to move safely and effectively in their environments, spanning conditions from acute injury recovery to chronic degenerative disorders.

## Bounded Contexts

1. Functional Mobility Assessment Context - Standardized evaluation of a patient's ability to perform transfers, ambulation, stair negotiation, and positional changes using validated assessment tools such as the Timed Up and Go (TUG) test, Functional Independence Measure (FIM), and Berg Balance Scale.
2. Assistive Device Management Context - Selection, fitting, prescription, training, and maintenance of mobility aids including walkers, canes, crutches, wheelchairs, scooters, and orthotics, ensuring appropriate device-patient matching and insurance authorization.
3. Gait Analysis Context - Quantitative and qualitative analysis of walking patterns, including spatiotemporal parameters, kinematics, kinetics, and electromyographic data, used to diagnose gait deviations and guide intervention planning.
4. Fall Prevention Context - Risk assessment, environmental modification, exercise programming, medication review, and multifactorial intervention planning to reduce fall incidence and injury severity in at-risk populations.
5. Accessibility Planning Context - Evaluation and modification of home, workplace, and community environments to remove architectural barriers, ensure ADA compliance, and promote independent mobility for individuals with physical limitations.

## Domain Principles

- Model using shared ubiquitous language between physical therapists, occupational therapists, physiatrists, orthotists, rehabilitation engineers, and software developers.
- Group the system into bounded contexts reflecting the distinct clinical workflows of assessment, device management, gait analysis, fall prevention, and accessibility.
- Business logic remains pure and independent of infrastructure (database, UI, web, mobile).
- Follow clinical practice guidelines from the American Physical Therapy Association (APTA), World Health Organization ICF framework, and ADA accessibility standards.
- Maintain patient safety, functional independence, and evidence-based practice as cross-cutting concerns.

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
- functional-mobility-assessment-context
- assistive-device-management-context
- gait-analysis-context
- fall-prevention-context
- accessibility-planning-context
- event-driven-architecture
- integration-patterns
- mobility-standards
- regulatory-compliance

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- World Health Organization. International Classification of Functioning, Disability and Health (ICF). WHO, 2001.
- O'Sullivan, Susan B., Schmitz, Thomas J., and Fulk, George D. Physical Rehabilitation. F.A. Davis Company, 2019.
- Americans with Disabilities Act (ADA) Standards for Accessible Design. U.S. Department of Justice, 2010.
