# Ergonomics Domain - AGENTS.md

## Overview

This domain applies Domain-Driven Design (DDD) to ergonomics systems, encompassing workplace risk assessment and hazard identification, workstation design and configuration, musculoskeletal disorder prevention, ergonomic intervention planning, occupational health monitoring, and human factors engineering.

## Bounded Contexts

1. Risk Assessment - Manages the identification, evaluation, and scoring of ergonomic risk factors in work environments using standardized assessment tools such as REBA, RULA, and the Strain Index.
2. Workstation Design - Handles the specification, configuration, and adjustment of workstation components including seating, desk height, monitor placement, keyboard positioning, and tool design to meet anthropometric requirements.
3. Intervention Planning - Governs the recommendation, implementation, and tracking of ergonomic interventions including equipment modifications, job rotation, work-rest schedules, and administrative controls.
4. Health Monitoring - Tracks worker health outcomes related to ergonomic exposures, including musculoskeletal symptom surveys, injury reports, return-to-work protocols, and trend analysis for proactive prevention.

## Domain Principles

- Model using shared ubiquitous language between ergonomists, occupational health professionals, safety engineers, human resources personnel, and system developers.
- Group the system into bounded contexts reflecting the distinct workflows of risk assessment, workstation design, intervention management, and health outcome monitoring.
- Business logic remains pure and independent of infrastructure (database, UI, web, mobile).
- Follow ISO 11226 (static working postures), ISO 11228 (manual handling), OSHA ergonomics guidelines, and national workplace health and safety legislation.

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
- risk-assessment
- workstation-design
- intervention-planning
- health-monitoring
- event-driven-architecture
- integration-patterns
- ergonomics-standards
- regulatory-compliance

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- Kroemer, Karl H. E. Fitting the Human: Introduction to Ergonomics / Human Factors Engineering. 7th ed. CRC Press, 2017.
- Salvendy, Gavriel, ed. Handbook of Human Factors and Ergonomics. 4th ed. Wiley, 2012.
- International Ergonomics Association. Core Competencies in Ergonomics. IEA, 2019.
