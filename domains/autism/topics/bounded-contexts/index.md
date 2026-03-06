# Bounded Contexts: Autism Domain

Bounded contexts define the logical boundaries within the autism spectrum management domain where specific models, terminologies, and rules remain internally consistent.

## Purpose

In the autism domain, professionals from diverse disciplines -- diagnosticians, behavior analysts, occupational therapists, speech-language pathologists, and special educators -- each use specialized terminology and operate under distinct professional frameworks. Bounded contexts formalize these natural divisions, ensuring that each area maintains model integrity while enabling structured collaboration across boundaries.

## Diagnostic Assessment Context

This context encompasses all activities related to identifying and formally diagnosing autism spectrum disorder. It owns the screening process (M-CHAT-R/F for toddlers, broader screening questionnaires for older individuals), standardized diagnostic instruments (ADOS-2, ADI-R), DSM-5 criteria application, developmental history collection, and multi-disciplinary team evaluation. The primary entities are the Individual Under Evaluation, Assessment Session, and Diagnostic Report. Domain experts include developmental pediatricians, clinical psychologists, and neuropsychologists.

Key invariants: A formal diagnosis requires completion of at least one standardized diagnostic instrument plus clinical observation. Screening results alone do not constitute a diagnosis.

## Behavioral Intervention Context

This context manages the design, implementation, and monitoring of behavioral interventions grounded in Applied Behavior Analysis (ABA). It owns behavior intervention plans (BIPs), skill acquisition programs, discrete trial training (DTT) protocols, natural environment teaching (NET) procedures, and session-level data collection. The primary entities are Intervention Plan, Skill Acquisition Program, and Therapy Session. Domain experts are Board Certified Behavior Analysts (BCBAs) and Registered Behavior Technicians (RBTs).

Key invariants: Every behavior intervention plan must be based on a completed functional behavior assessment. Skill acquisition programs require measurable criteria for mastery.

## Sensory Processing Context

This context handles the assessment and management of sensory processing differences common in individuals with autism. It owns sensory profile assessments (Sensory Profile 2), sensory diet design, environmental modification recommendations, and sensory integration therapy protocols. The primary entities are Sensory Profile, Sensory Diet, and Environmental Modification Plan. Occupational therapists are the domain experts.

Key invariants: A sensory diet must be based on a completed sensory profile assessment. Environmental modifications require documented sensory triggers and proposed accommodations.

## Communication Support Context

This context governs all communication-related assessments and interventions. It owns AAC device evaluations, PECS implementation protocols, speech-language therapy goals, social communication interventions, and pragmatic language programs. The primary entities are Communication Plan, AAC Configuration, and Speech-Language Goal. Speech-language pathologists serve as domain experts.

Key invariants: AAC device selection must be based on a comprehensive communication assessment. PECS implementation must follow the six-phase protocol sequence.

## Educational Planning Context

This context manages the educational support framework for individuals with autism in school settings. It owns IEP development and review cycles, transition planning (beginning at age 16), inclusive education strategies, classroom accommodation plans, and paraprofessional coordination. The primary entities are IEP Document, Transition Plan, and Accommodation Record. Special education coordinators and teachers are the domain experts.

Key invariants: An IEP must be reviewed at least annually. Transition planning must begin no later than the IEP in effect when the student turns 16.

## Outcomes Tracking Context

This context tracks progress and measures effectiveness across all intervention areas. It owns developmental milestone tracking, treatment effectiveness metrics, quality of life assessments, and longitudinal outcome analysis. The primary entities are Outcome Measure, Progress Report, and Milestone Record. Care coordinators and clinical supervisors are the domain experts.

Key invariants: Outcome measures must be linked to specific intervention goals. Progress reports require data from at least two consecutive measurement periods.

## Boundary Enforcement

Each bounded context enforces its boundaries by maintaining its own domain model, repositories, and domain services. When one context needs information from another, it communicates through published integration interfaces rather than directly accessing another context's internal model. This prevents model corruption and allows each context to evolve independently.

## References

- Evans, E. (2003). *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley. Chapter 14 on maintaining model integrity.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley. Chapter 2 on bounded contexts.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media. Chapter 5 on bounded context boundaries.
- Lord, C., et al. (2012). *ADOS-2 Manual*. Western Psychological Services.
- Cooper, J. O., Heron, T. E., & Heward, W. L. (2020). *Applied Behavior Analysis* (3rd ed.). Pearson.
