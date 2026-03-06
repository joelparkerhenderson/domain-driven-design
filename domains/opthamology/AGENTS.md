# Ophthalmology Domain - DDD Project Instructions

## Overview

This domain applies Domain-Driven Design (DDD) to ophthalmology systems,
covering clinical examination, diagnostic imaging, surgical management,
refractive services, glaucoma management, and retinal care.

## Bounded Contexts

1. Clinical Examination Context
2. Diagnostic Imaging Context
3. Surgical Management Context
4. Refractive Services Context
5. Glaucoma Management Context
6. Retinal Care Context

## Domain Scope

- Visual acuity testing and refraction
- Slit lamp examination and tonometry
- OCT, fundus photography, fluorescein angiography, visual fields
- Cataract surgery, vitrectomy, corneal transplant, oculoplastics
- LASIK, PRK, lens implants, pre/post-operative management
- IOP monitoring, visual field progression, glaucoma treatment
- Diabetic retinopathy, AMD, retinal detachment, intravitreal injections

## Project Structure

- plan.md: Strategic plan for the domain
- tasks.md: Task tracking for domain build-out
- index.md: Domain overview and navigation
- ubiquitous-language.md: Shared terminology across the domain
- topics/: Directory containing individual topic documentation

## Guidelines

- Use ubiquitous language consistently across all documentation.
- Model business logic as pure domain logic without infrastructure concerns.
- Each bounded context maintains its own internal consistency.
- Cross-context communication uses domain events and anti-corruption layers.
- Follow CQRS principles: separate read, write, and read-write logic.
- Do not create images, diagrams, web applications, front-end, or back-end code.
- Provide references and citations to authoritative sources.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
