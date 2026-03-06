# Bounded Contexts - Otolaryngology Domain

## Overview

Bounded contexts define the logical boundaries within which specific domain models and terminologies remain consistent. In the otolaryngology domain, each bounded context corresponds to a distinct area of ENT clinical practice, with its own ubiquitous language, aggregates, invariants, and workflows. The six bounded contexts reflect the natural divisions in otolaryngology practice.

## Bounded Context Definitions

### 1. Clinical Assessment Context

The Clinical Assessment Context encompasses all diagnostic and evaluative activities in ENT practice. This includes the initial patient encounter, head and neck physical examination, nasal endoscopy, laryngoscopy, audiometric testing, and diagnostic imaging interpretation. The primary aggregate is the ENT Encounter, which collects examination findings, test results, and clinical impressions into a coherent diagnostic picture.

Key concepts: ENT Encounter, Examination Finding, Endoscopic Report, Audiogram, Imaging Study, Clinical Impression, Referral.

### 2. Surgical Management Context

The Surgical Management Context manages the full lifecycle of ENT surgical procedures. It spans pre-operative evaluation and planning, surgical scheduling, intra-operative documentation, and post-operative follow-up. Procedures range from minor office-based interventions to major head and neck oncologic resections. The primary aggregate is the Surgical Case, which maintains consistency across the pre-operative, intra-operative, and post-operative phases.

Key concepts: Surgical Case, Operative Note, Pre-Operative Assessment, Surgical Consent, Post-Operative Plan, Complication, Pathology Result.

### 3. Voice Disorders Context

The Voice Disorders Context handles the evaluation and management of voice, speech, and swallowing disorders. It operates at the intersection of otolaryngology and speech-language pathology, requiring collaboration between ENT surgeons and speech therapists. The primary aggregate is the Voice Assessment, which combines laryngoscopic findings, acoustic measurements, and patient-reported outcomes.

Key concepts: Voice Assessment, Laryngoscopic Examination, Voice Handicap Index, Vocal Fold Pathology, Voice Therapy Protocol, Phonosurgery Plan.

### 4. Sleep Disorders Context

The Sleep Disorders Context manages sleep-disordered breathing evaluation and treatment. It encompasses polysomnography ordering and interpretation, continuous positive airway pressure (CPAP) therapy management, and surgical interventions for obstructive sleep apnea. The primary aggregate is the Sleep Evaluation, which tracks the patient's sleep disorder from initial screening through treatment outcomes.

Key concepts: Sleep Evaluation, Polysomnography Result, Apnea-Hypopnea Index, CPAP Prescription, CPAP Compliance Record, Surgical Intervention Plan.

### 5. Allergy Management Context

The Allergy Management Context handles allergic disease evaluation and treatment within the ENT scope. It covers allergy testing (skin prick, intradermal, serum-specific IgE), immunotherapy protocol management, allergic rhinitis treatment, and chronic rhinosinusitis management. The primary aggregate is the Allergy Profile, which captures a patient's sensitization pattern and treatment history.

Key concepts: Allergy Profile, Skin Test Result, Allergen Panel, Immunotherapy Protocol, Rhinitis Management Plan, Sinusitis Treatment Plan.

### 6. Hearing Services Context

The Hearing Services Context provides comprehensive audiological care. It covers hearing evaluation, hearing aid selection and fitting, cochlear implant candidacy evaluation, and aural rehabilitation. The primary aggregate is the Audiological Record, which tracks a patient's hearing status, device history, and rehabilitation outcomes over time.

Key concepts: Audiological Record, Hearing Evaluation, Audiogram, Hearing Aid Fitting, Cochlear Implant Evaluation, Aural Rehabilitation Plan.

## Context Boundaries

Each bounded context maintains strict boundaries:

- **Language boundary**: The term "assessment" means different things in Clinical Assessment (a diagnostic encounter) versus Voice Disorders (a voice-specific evaluation protocol). Each context defines its own precise meaning.
- **Model boundary**: A patient in the Surgical Management Context is modeled with surgical history, anesthesia risk, and anatomical considerations. The same patient in Hearing Services is modeled with audiometric history, device preferences, and communication needs.
- **Consistency boundary**: Each context enforces its own invariants. A Surgical Case cannot proceed without a completed consent. An Immunotherapy Protocol cannot advance without documented allergen sensitivities.

## Anti-Corruption Between Contexts

When information crosses context boundaries, anti-corruption layers translate concepts. A "hearing evaluation" in Hearing Services becomes a "pre-operative audiometric baseline" when consumed by Surgical Management for a cochlear implant surgery. This translation preserves each context's model integrity.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 14 on maintaining model integrity.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 2 on bounded contexts.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 6 on bounded context boundaries.
