# Aggregates for Mast Cell Activation Syndrome

## Overview

Aggregates are clusters of related domain objects treated as a single unit for the purpose of data changes and consistency enforcement. Each aggregate has a root entity that serves as the entry point for all interactions with the aggregate. In the MCAS domain, aggregates model the natural groupings of clinical concepts that must remain consistent when modified.

Aggregate design determines transactional boundaries. All changes within an aggregate are committed atomically, while changes across aggregates are eventually consistent through domain events. This principle is critical in the MCAS domain, where clinical data integrity directly impacts patient safety.

## Diagnostic Assessment Aggregates

### Diagnostic Workup Aggregate

The Diagnostic Workup aggregate is the primary aggregate in the Diagnostic Assessment Context. Its root entity is the Diagnostic Workup, which groups all laboratory tests, results, and criteria evaluations for a single diagnostic episode.

The aggregate contains Mediator Panel entities representing ordered test panels, Mediator Level value objects for individual test results, and Criteria Evaluation value objects that assess results against consensus criteria. The Diagnostic Workup aggregate enforces the invariant that a diagnosis cannot be confirmed until all required criteria elements have been evaluated.

### Bone Marrow Assessment Aggregate

The Bone Marrow Assessment is a separate aggregate because it has its own lifecycle and consistency requirements. It contains the biopsy procedure record, pathology findings, and the determination of whether clonal mast cell disease is present. This aggregate enforces the invariant that a bone marrow assessment must include both aspirate and biopsy results before generating a conclusion.

## Trigger Management Aggregates

### Trigger Profile Aggregate

The Trigger Profile aggregate is the root aggregate in the Trigger Management Context. It represents the complete collection of identified triggers for a patient. The aggregate root is the Trigger Profile entity, containing Trigger Entry entities for each identified trigger.

Each Trigger Entry includes the trigger substance or condition, the evidence supporting the identification, the confidence level, and the associated avoidance strategy. The aggregate enforces the invariant that each trigger must have at least one documented exposure-reaction correlation before being classified as confirmed.

### Avoidance Plan Aggregate

The Avoidance Plan aggregate groups environmental controls and dietary restrictions into a cohesive management strategy. It ensures that avoidance recommendations do not conflict with one another and that the overall plan is clinically coherent.

## Medication Protocol Aggregates

### Medication Regimen Aggregate

The Medication Regimen aggregate is the central aggregate in the Medication Protocol Context. Its root entity groups all active medications, their dosages, schedules, and titration plans into a single consistency unit.

The aggregate enforces invariants such as checking for known medication interactions, ensuring that titration steps do not exceed safe dosage increments, and validating that compounding specifications exclude identified problematic excipients. All medication changes are applied through the aggregate root to maintain these invariants.

### Compounding Specification Aggregate

The Compounding Specification aggregate manages the formulation details for custom-compounded medications. It contains the active ingredient specification, the dosage form, and the explicit exclusion list of dyes, fillers, and excipients. The aggregate ensures that compounding instructions are complete and internally consistent.

## Symptom Tracking Aggregates

### Symptom Log Aggregate

The Symptom Log aggregate captures a time-bounded collection of symptom entries. The aggregate root manages the addition of new symptom entries, ensuring that each entry includes the required severity score, organ system classification, and timestamp. The aggregate enforces the invariant that symptom entries within the log maintain chronological order.

### Flare Record Aggregate

The Flare Record aggregate documents a single acute exacerbation episode. It contains the flare onset time, duration, peak severity, associated symptoms, suspected triggers, and interventions applied. The aggregate enforces the invariant that a flare record must have an onset time and at least one documented symptom before it can be finalized.

## Specialist Coordination Aggregates

### Care Team Aggregate

The Care Team aggregate groups all providers involved in a patient's MCAS management. The aggregate root ensures that each provider has a defined role and that there are no duplicate specialist assignments. Changes to the care team composition are managed through the aggregate root.

### Referral Aggregate

The Referral aggregate manages the lifecycle of a specialist referral from request through completion. It enforces the invariant that a referral must have a referring provider, a target specialty, and a clinical reason before it can be submitted.

## Outcomes Measurement Aggregates

### Outcomes Assessment Aggregate

The Outcomes Assessment aggregate groups all measurement data for a single assessment period. It contains symptom burden scores, quality of life ratings, medication effectiveness evaluations, and functional status measurements. The aggregate ensures that all component scores are collected before an overall assessment can be finalized.

## Design Principles

Aggregates in the MCAS domain are designed to be as small as possible while maintaining necessary invariants. Large aggregates create contention and scalability problems. Cross-aggregate references use identifiers rather than direct object references to maintain loose coupling.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 6 on aggregates.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 10 on aggregate design.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 5 on aggregate boundaries.
