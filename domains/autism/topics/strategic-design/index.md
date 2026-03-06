# Strategic Design: Autism Domain

Strategic design decomposes the autism spectrum management domain into manageable bounded contexts, each with clear responsibilities, consistent terminology, and well-defined integration points.

## Purpose

The autism domain spans clinical diagnosis, behavioral therapy, sensory processing, communication intervention, educational planning, and outcomes tracking. Strategic design establishes the high-level architecture that organizes these diverse concerns into cohesive, independently evolvable bounded contexts while maintaining coordinated care across the entire system.

## Domain Decomposition

The autism domain is decomposed into six bounded contexts based on distinct areas of clinical and educational responsibility:

- Diagnostic Assessment Context: Owns the screening, evaluation, and formal diagnosis process. This context manages standardized instruments such as M-CHAT-R/F and ADOS-2, applies DSM-5 criteria, and coordinates multi-disciplinary team assessments. It produces the foundational diagnostic information consumed by all other contexts.

- Behavioral Intervention Context: Manages Applied Behavior Analysis (ABA) therapy programs, behavior intervention plans, skill acquisition protocols, and systematic data collection. This context is owned by Board Certified Behavior Analysts (BCBAs) and Registered Behavior Technicians (RBTs).

- Sensory Processing Context: Handles sensory profile assessments, sensory diet design, environmental modification recommendations, and sensory integration therapy. Occupational therapists serve as the primary domain experts for this context.

- Communication Support Context: Governs augmentative and alternative communication (AAC) device selection, Picture Exchange Communication System (PECS) implementation, speech-language pathology goals, and social communication interventions. Speech-language pathologists are the domain experts.

- Educational Planning Context: Manages Individualized Education Program (IEP) development, transition planning for post-secondary life, inclusive education strategies, and paraprofessional support coordination. Special education coordinators and educators drive this context.

- Outcomes Tracking Context: Tracks developmental milestones, measures treatment effectiveness, assesses quality of life, and provides longitudinal analysis across all intervention areas. This context aggregates data from all other contexts to inform care decisions.

## Strategic Importance

Each bounded context is classified by strategic importance to determine investment priority:

- Core Subdomains: Diagnostic Assessment and Behavioral Intervention represent the highest-value, most complex areas that differentiate autism management systems from generic healthcare platforms.

- Supporting Subdomains: Sensory Processing and Communication Support provide essential specialized capabilities that enhance the core but are not unique to the overall system architecture.

- Generic Subdomains: Educational Planning and Outcomes Tracking follow well-established patterns from special education and healthcare analytics that can leverage existing frameworks.

## Context Relationships

The six bounded contexts interact through defined relationships documented in the context map. The Diagnostic Assessment Context serves as an upstream context that provides diagnostic information to all downstream contexts. The Outcomes Tracking Context serves as a downstream consumer that aggregates data from all other contexts.

## Design Principles

- Autonomy: Each bounded context can evolve independently without breaking others.
- Ubiquitous Language: Each context maintains its own consistent vocabulary aligned with the professional discipline it represents.
- Explicit Boundaries: Integration between contexts occurs through published interfaces, not shared databases or internal models.
- Domain Expert Alignment: Each context is designed in close collaboration with the clinical or educational professionals who own that area of expertise.

## Subdomain Analysis

Strategic design requires ongoing analysis to ensure subdomain classifications remain accurate as the domain evolves. A supporting subdomain may become core if the organization identifies it as a competitive differentiator. Regular review sessions with domain experts ensure the strategic design stays aligned with organizational priorities and clinical best practices.

## References

- Evans, E. (2003). *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley. Chapters 14-15 on maintaining model integrity and strategic design.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley. Chapter 2 on domains, subdomains, and bounded contexts.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media. Part II on strategic design decisions.
- American Psychiatric Association. (2013). *Diagnostic and Statistical Manual of Mental Disorders* (5th ed.). Autism Spectrum Disorder diagnostic criteria.
