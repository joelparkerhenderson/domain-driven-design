# Aggregates - Otolaryngology Domain

## Overview

Aggregates are clusters of related domain objects treated as a single unit for data consistency. Each aggregate has a root entity that controls access to all objects within the aggregate and enforces invariants that must remain consistent after every transaction. In the otolaryngology domain, aggregates model the clinical units of work that ENT specialists, audiologists, and other providers manage as coherent wholes.

## Aggregate Design Principles in Otolaryngology

Clinical workflows define natural aggregate boundaries. An ENT encounter is a coherent unit: you cannot record an endoscopic finding without the encounter that produced it. A surgical case is a coherent unit: you cannot have a post-operative plan without the surgical procedure it follows. These clinical realities guide aggregate boundary decisions.

Each aggregate enforces invariants that reflect clinical rules. A surgical case cannot transition to "completed" without an operative note. An immunotherapy protocol cannot advance to the next dose without documented tolerance of the previous dose. These invariants ensure that the domain model maintains clinical integrity.

## Key Aggregates by Bounded Context

### Clinical Assessment Context

**ENT Encounter** (Aggregate Root)
- Contains: Examination Findings, Endoscopic Reports, Audiometric Results, Imaging Study References, Clinical Impressions
- Invariants: Every encounter must have a provider and a date. Examination findings must reference a valid anatomical site. Clinical impressions must be supported by at least one documented finding.
- Lifecycle: Created at patient check-in, populated during the encounter, finalized when the provider signs the note.

### Surgical Management Context

**Surgical Case** (Aggregate Root)
- Contains: Pre-Operative Assessment, Surgical Consent, Operative Note, Post-Operative Plan, Complication Records, Pathology Results
- Invariants: A case cannot proceed to surgery without a completed pre-operative assessment and signed consent. An operative note must be completed within 24 hours of procedure completion. Complications must be linked to a specific procedure.
- Lifecycle: Created at surgical scheduling, evolves through pre-operative preparation, intra-operative documentation, and post-operative follow-up.

### Voice Disorders Context

**Voice Assessment** (Aggregate Root)
- Contains: Laryngoscopic Examination, Acoustic Measurements, Aerodynamic Measurements, Voice Handicap Index Score, Perceptual Voice Evaluation
- Invariants: A voice assessment must include at least one objective measurement (acoustic or aerodynamic) and one subjective measure (VHI or perceptual evaluation). Therapy recommendations must be supported by assessment findings.
- Lifecycle: Created at initial voice evaluation, updated at each follow-up assessment.

### Sleep Disorders Context

**Sleep Evaluation** (Aggregate Root)
- Contains: Sleep Screening Questionnaire, Polysomnography Result, CPAP Prescription, CPAP Compliance Records, Treatment Outcome Measurements
- Invariants: A CPAP prescription requires a documented AHI above the treatment threshold. CPAP compliance must be assessed at defined intervals. Surgical referral requires documented CPAP intolerance or failure.
- Lifecycle: Created at initial sleep complaint evaluation, tracks the patient through diagnosis and treatment.

### Allergy Management Context

**Allergy Profile** (Aggregate Root)
- Contains: Skin Test Results, Serum IgE Results, Allergen Sensitivities, Immunotherapy Protocol, Treatment History
- Invariants: An immunotherapy protocol cannot be created without documented allergen sensitivities. Dose escalation requires documented tolerance of the previous dose. Allergen extract formulations must match documented sensitivities.
- Lifecycle: Created at initial allergy evaluation, maintained longitudinally as sensitivities and treatments evolve.

### Hearing Services Context

**Audiological Record** (Aggregate Root)
- Contains: Hearing Evaluations, Audiograms, Hearing Aid Fittings, Cochlear Implant Evaluations, Aural Rehabilitation Plans
- Invariants: A hearing aid fitting requires a current audiogram (within six months). Cochlear implant candidacy requires documented hearing aid trial. Audiograms must include frequency-specific thresholds and speech recognition scores.
- Lifecycle: Created at first audiological encounter, maintained as a longitudinal record of hearing status and device history.

## Aggregate Size Guidelines

Following Vernon's guidance on small aggregates, the otolaryngology domain prefers aggregates that are as small as practical while maintaining consistency. A Surgical Case aggregate is necessarily large because clinical consistency requires that consent, operative note, and post-operative plan are always consistent with each other. An Audiogram, however, is a value object within the Audiological Record aggregate rather than its own aggregate, because it has no independent lifecycle.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 6 on aggregates.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 10 on aggregate design rules.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 5 on aggregate patterns.
