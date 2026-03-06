# Bounded Contexts in the Audiology Domain

## Overview

Bounded contexts define the logical boundaries within the audiology domain where specific models, terminology, and business rules remain internally consistent. Each bounded context in the audiology domain represents a distinct area of clinical practice with its own ubiquitous language, aggregates, and invariants. The audiology domain comprises six bounded contexts that together cover the full scope of hearing healthcare delivery.

## The Six Bounded Contexts

### 1. Hearing Assessment Context

The Hearing Assessment Context encompasses all diagnostic evaluation activities. This includes pure tone audiometry (air and bone conduction), tympanometry, otoacoustic emissions (OAE) testing, auditory brainstem response (ABR), and speech recognition testing (SRT and WRS). Within this context, the central concept is the diagnostic evaluation, which produces an audiogram and associated test results. The language of this context centers on thresholds, frequencies, decibels, masking, and hearing loss classification.

### 2. Device Management Context

The Device Management Context covers the selection, fitting, programming, verification, and maintenance of hearing devices. This includes hearing aids of all form factors, cochlear implants, bone-anchored hearing systems, and assistive listening devices. Within this context, the central concepts are the device fitting and the device itself. The language centers on gain, compression, channels, programs, real ear measurement, and prescriptive targets. This context maintains its own model of the patient's hearing profile, translated from the Hearing Assessment Context through a published language.

### 3. Rehabilitation Context

The Rehabilitation Context manages aural rehabilitation programs, auditory training activities, communication strategy instruction, and counseling services. Within this context, the central concept is the rehabilitation plan, which tracks goals, interventions, and outcomes over time. The language centers on communication strategies, auditory skills, self-assessment, and functional outcomes. This context receives diagnostic and device information from upstream contexts to tailor rehabilitation approaches.

### 4. Vestibular Services Context

The Vestibular Services Context handles balance assessment and vestibular rehabilitation. This includes videonystagmography (VNG), electronystagmography (ENG), rotary chair testing, vestibular evoked myogenic potentials (VEMP), and vestibular rehabilitation therapy (VRT). Within this context, the central concept is the vestibular evaluation, which characterizes the nature and site of balance dysfunction. The language centers on nystagmus, vertigo, semicircular canals, otolith function, and postural stability.

### 5. Pediatric Audiology Context

The Pediatric Audiology Context addresses the specialized needs of hearing assessment and intervention for children from birth through adolescence. This includes newborn hearing screening, visual reinforcement audiometry, conditioned play audiometry, pediatric device fitting, and developmental monitoring. Within this context, age-appropriate methods and family-centered care are central concerns. The language adapts general audiology terms for pediatric application and adds concepts like developmental milestones, early intervention, and family engagement.

### 6. Clinical Documentation Context

The Clinical Documentation Context manages the creation, storage, and distribution of clinical records. This includes audiogram generation, clinical reports, referral management, outcome measure tracking, and compliance documentation. Within this context, the central concept is the clinical document, which must conform to regulatory and professional standards. The language centers on report templates, referral workflows, outcome instruments, and documentation compliance.

## Boundary Definitions

Each bounded context maintains clear boundaries that define what falls inside and outside its scope. The Hearing Assessment Context does not prescribe devices; it produces diagnostic data. The Device Management Context does not perform diagnostic testing; it consumes assessment results. These boundaries prevent model corruption and ensure each context can evolve independently.

Boundary enforcement is critical in audiology because the same patient appears across multiple contexts. The patient entity in Hearing Assessment carries diagnostic history, while the patient entity in Device Management carries device fitting history. These are different projections of the same real-world person, each optimized for its context's concerns.

## Context Autonomy

Each bounded context operates with maximum autonomy. The Hearing Assessment Context can adopt new testing protocols without affecting Device Management. The Rehabilitation Context can introduce new training methodologies without requiring changes to Clinical Documentation. This autonomy enables each area of audiologic practice to evolve at its own pace, reflecting the reality that clinical advances occur unevenly across subspecialties.

## Language Boundaries

The same term may carry different meanings across bounded contexts. "Threshold" in Hearing Assessment refers to the minimum audible sound level at a specific frequency. In Device Management, a threshold concept might refer to the activation level of a compression circuit. Bounded contexts make these distinctions explicit and prevent confusion that would arise from forcing a single definition across the entire domain.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley. Chapter 14 on maintaining model integrity.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley. Chapter 2 on bounded contexts.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media. Chapter 4 on bounded context boundaries.
- American Speech-Language-Hearing Association (ASHA). Scope of Practice in Audiology.
