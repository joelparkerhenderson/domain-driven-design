# Pediatrics Domain - AGENTS.md

## Overview

This domain applies Domain-Driven Design (DDD) to pediatric healthcare systems, encompassing well-child care, growth and development monitoring, childhood illness management, pediatric immunization programs, newborn and neonatal care, and adolescent health services. Pediatrics is the medical specialty dedicated to the health, growth, and development of infants, children, and adolescents from birth through age eighteen.

## Bounded Contexts

1. Well-Child Care Context - Scheduled preventive health visits that include physical examination, developmental screening, anticipatory guidance, vision and hearing screening, and parent education, following the American Academy of Pediatrics (AAP) Bright Futures periodicity schedule.
2. Growth and Development Context - Longitudinal tracking of physical growth parameters (weight, height, head circumference, BMI) using WHO and CDC growth charts, developmental milestone assessment using standardized tools, and early identification of growth disorders, developmental delays, and behavioral concerns.
3. Childhood Illness Management Context - Diagnosis, treatment, and follow-up of acute and chronic pediatric conditions including respiratory infections, otitis media, asthma, gastroenteritis, allergies, and chronic disease management with age-appropriate medication dosing and care plans.
4. Immunization Context - Administration, scheduling, documentation, and adverse event monitoring of childhood vaccines following the CDC/ACIP immunization schedule, including catch-up scheduling, contraindication screening, and vaccine information statement distribution.
5. Neonatal Care Context - Assessment and management of newborns from birth through the first 28 days of life, including Apgar scoring, newborn screening tests, jaundice monitoring, feeding support, congenital anomaly detection, and neonatal intensive care coordination.
6. Adolescent Health Context - Age-appropriate health services for patients aged ten through eighteen, including confidential screening for mental health, substance use, sexual health, and risk behaviors using validated tools such as the HEEADSSS assessment framework.

## Domain Principles

- Model using shared ubiquitous language between pediatricians, pediatric nurses, developmental specialists, parents/guardians, and software developers.
- Group the system into bounded contexts reflecting the distinct clinical workflows of preventive care, growth monitoring, illness management, immunization, neonatal care, and adolescent services.
- Business logic remains pure and independent of infrastructure (database, UI, web, mobile).
- Follow AAP Bright Futures guidelines, CDC immunization schedules, WHO growth standards, and age-appropriate clinical practice guidelines.
- Maintain child safety, parental consent, age-specific dosing, and developmental sensitivity as cross-cutting concerns.

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
- well-child-care-context
- growth-and-development-context
- childhood-illness-management-context
- immunization-context
- neonatal-care-context
- adolescent-health-context
- event-driven-architecture
- integration-patterns
- pediatric-standards
- regulatory-compliance

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- Hagan, Joseph F., Shaw, Judith S., and Duncan, Paula M. Bright Futures: Guidelines for Health Supervision of Infants, Children, and Adolescents. American Academy of Pediatrics, 2017.
- Kliegman, Robert M. et al. Nelson Textbook of Pediatrics. Elsevier, 2020.
- Centers for Disease Control and Prevention. Recommended Child and Adolescent Immunization Schedule. CDC/ACIP, updated annually.
