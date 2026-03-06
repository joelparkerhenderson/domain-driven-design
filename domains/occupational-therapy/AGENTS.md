# Occupational Therapy Domain - AGENTS.md

## Overview

This domain applies Domain-Driven Design (DDD) to occupational therapy (OT) systems, encompassing functional assessment, intervention planning, activities of daily living (ADL) training, adaptive equipment provision, cognitive rehabilitation, and outcomes measurement. Occupational therapy focuses on enabling individuals to participate in meaningful occupations and daily activities despite physical, cognitive, sensory, or psychosocial limitations.

## Bounded Contexts

1. Functional Assessment Context - Comprehensive evaluation of a client's occupational performance using standardized assessments such as the Canadian Occupational Performance Measure (COPM), occupational profile development, activity analysis, and identification of performance deficits across self-care, productivity, and leisure domains.
2. Intervention Planning Context - Development of individualized, goal-directed treatment plans incorporating evidence-based interventions, therapeutic activities, client priorities, discharge criteria, and progress benchmarks aligned with the Occupational Therapy Practice Framework (OTPF).
3. ADL and IADL Training Context - Direct therapeutic training in basic activities of daily living (bathing, dressing, grooming, feeding, toileting) and instrumental activities of daily living (meal preparation, medication management, community mobility, financial management) using compensatory strategies and skill remediation.
4. Adaptive Equipment Context - Assessment, recommendation, fabrication, fitting, and training for assistive technology, adaptive devices, splints, orthoses, and environmental modifications that compensate for functional limitations and promote occupational engagement.
5. Cognitive Rehabilitation Context - Assessment and treatment of cognitive deficits including attention, memory, executive function, visual perception, and problem-solving, using restorative and compensatory approaches to support occupational performance.
6. Outcomes Measurement Context - Systematic collection, analysis, and reporting of functional outcomes using standardized measures, goal attainment scaling, patient-reported outcomes, and discharge status to evaluate treatment effectiveness and support clinical decision-making.

## Domain Principles

- Model using shared ubiquitous language between occupational therapists, occupational therapy assistants, rehabilitation physicians, clients, caregivers, and software developers.
- Group the system into bounded contexts reflecting the distinct clinical workflows of assessment, planning, training, equipment provision, cognitive rehabilitation, and outcomes tracking.
- Business logic remains pure and independent of infrastructure (database, UI, web, mobile).
- Follow the Occupational Therapy Practice Framework (OTPF-4), American Occupational Therapy Association (AOTA) guidelines, and WHO ICF classification.
- Maintain client-centered practice, occupational justice, and evidence-based intervention as cross-cutting concerns.

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
- functional-assessment-context
- intervention-planning-context
- adl-iadl-training-context
- adaptive-equipment-context
- cognitive-rehabilitation-context
- outcomes-measurement-context
- event-driven-architecture
- integration-patterns
- occupational-therapy-standards
- regulatory-compliance

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- American Occupational Therapy Association. Occupational Therapy Practice Framework: Domain and Process, 4th Edition. AJOT, 2020.
- Radomski, Mary Vining and Trombly Latham, Catherine A. Occupational Therapy for Physical Dysfunction. Wolters Kluwer, 2014.
- World Health Organization. International Classification of Functioning, Disability and Health (ICF). WHO, 2001.
