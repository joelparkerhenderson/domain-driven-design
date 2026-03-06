# Otolaryngology Domain - DDD Project Instructions

## Domain Overview

This domain applies Domain-Driven Design (DDD) to otolaryngology (ENT: ear, nose, throat) systems, encompassing clinical assessment, surgical management, voice disorders, sleep disorders, allergy management, and hearing services.

## Bounded Contexts

1. Clinical Assessment Context - head/neck examination, endoscopy, audiometry, imaging
2. Surgical Management Context - tonsillectomy, septoplasty, sinus surgery, thyroidectomy, head/neck oncology
3. Voice Disorders Context - laryngoscopy, voice therapy, vocal fold pathology, phonosurgery
4. Sleep Disorders Context - sleep apnea evaluation, CPAP management, surgical interventions (UPPP)
5. Allergy Management Context - allergy testing, immunotherapy, rhinitis management, sinusitis
6. Hearing Services Context - hearing evaluation, hearing aid fitting, cochlear implant evaluation

## Project Structure

- `plan.md` - Strategic plan for domain modeling
- `tasks.md` - Task tracking for domain build-out
- `ubiquitous-language.md` - Shared terminology across all bounded contexts
- `index.md` - Domain overview and navigation
- `topics/` - Detailed documentation for each DDD concept and bounded context

## Design Principles

- Model using shared ubiquitous language between ENT clinicians, audiologists, speech pathologists, and developers.
- Group system into bounded contexts aligned with ENT subspecialty workflows.
- Business logic is pure and independent of infrastructure (no database, UI, web, or mobile details).
- Apply CQRS to separate read-only, write-only, and read-write logic.
- Use event-driven architecture for communication between bounded contexts.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
