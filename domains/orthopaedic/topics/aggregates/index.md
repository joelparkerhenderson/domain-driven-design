# Aggregates in the Orthopaedic Domain

## Overview

An aggregate is a cluster of related domain objects treated as a single unit for
data consistency. Each aggregate has a root entity that controls access to the
cluster and enforces invariants. In the orthopaedic domain, aggregates encapsulate
the clinical and surgical business rules that must remain consistent within each
bounded context.

## Purpose

Orthopaedic workflows involve complex clinical data that must maintain internal
consistency. A surgical plan cannot reference an implant that is incompatible with
the patient's anatomy. A fracture case cannot have a fixation method that contradicts
its classification. Aggregates enforce these rules by defining transactional
boundaries around related objects.

## Key Aggregates by Bounded Context

### Clinical Assessment: Clinical Encounter Aggregate

- Root Entity: ClinicalEncounter
- Contains: ExaminationFindings, ImagingStudyReferences, ProvisionalDiagnoses
- Invariants: An encounter must have at least one examination finding before a
  diagnosis can be recorded. Imaging studies must be linked to the correct patient
  and anatomical region.
- Lifecycle: Created when a patient presents, completed when the clinician finalizes
  the assessment.

### Surgical Planning: Surgical Plan Aggregate

- Root Entity: SurgicalPlan
- Contains: SurgicalApproach, ImplantSelections, TemplatingResults, RiskAssessment
- Invariants: Selected implants must be compatible with the planned approach. The
  plan must have a completed pre-operative assessment before approval. Implant sizes
  must be consistent with templating measurements.
- Lifecycle: Created from a clinical referral, approved by the operating surgeon.

### Joint Replacement: Arthroplasty Case Aggregate

- Root Entity: ArthroplastyCase
- Contains: ImplantComponents, BearingSurface, OutcomeScores, RevisionHistory
- Invariants: All implant components must form a valid combination (e.g., compatible
  head size and acetabular liner). Outcome scores must be recorded at defined intervals.
  Revision history must link to the original case.
- Lifecycle: Created at surgery, persists for the lifetime of the implant.

### Sports Medicine: Sports Injury Case Aggregate

- Root Entity: SportsInjuryCase
- Contains: InjuryDiagnosis, ArthroscopicProcedures, ReturnToPlayProgress
- Invariants: Return-to-play progression cannot advance until the criteria for the
  current phase are met. Arthroscopic findings must be recorded before treatment
  decisions are finalized.
- Lifecycle: Created at injury presentation, closed when the athlete returns to play
  or treatment concludes.

### Fracture Management: Fracture Case Aggregate

- Root Entity: FractureCase
- Contains: AOOTAClassification, ReductionRecord, FixationMethod, HealingAssessments
- Invariants: The fixation method must be appropriate for the classified fracture type.
  Healing assessments must follow the prescribed follow-up schedule. Reduction quality
  must be documented before fixation proceeds.
- Lifecycle: Created at fracture diagnosis, closed when union is confirmed or
  treatment is concluded.

### Rehabilitation: Rehabilitation Episode Aggregate

- Root Entity: RehabilitationEpisode
- Contains: TherapyPhases, FunctionalMilestones, TherapySessions, OutcomeMeasures
- Invariants: Phases must be completed sequentially. Milestones cannot be marked
  achieved without supporting assessment data. Weight-bearing progression must
  follow the surgical team's prescription.
- Lifecycle: Created when rehabilitation begins, closed when discharge criteria are met.

## Aggregate Design Principles

- Keep aggregates small: include only what is needed for consistency.
- Reference other aggregates by identity, not by direct object reference.
- One transaction per aggregate: do not modify multiple aggregates in one transaction.
- Use domain events to coordinate changes across aggregates.
- Design aggregate boundaries around true invariants, not convenience.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 6.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 10.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 10.
