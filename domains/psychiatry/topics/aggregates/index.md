# Aggregates in the Psychiatry Domain

## Overview

An aggregate is a cluster of related domain objects treated as a single unit for the purpose of data changes and consistency enforcement. Each aggregate has a root entity that serves as the entry point for all interactions with the aggregate. In the psychiatry domain, aggregates represent the primary clinical units of work: evaluations, prescriptions, therapy sessions, crisis events, rehabilitation plans, and outcome reports. Aggregate boundaries are drawn to maintain clinical consistency invariants within each unit.

## Aggregate Design Principles

### Consistency Boundaries

Each aggregate enforces its own consistency rules (invariants) within its boundary. External references between aggregates use identity references (IDs), not direct object references. This ensures that each aggregate can be modified independently without creating cascading consistency problems across the system.

### Small Aggregates

Aggregates in the psychiatry domain are kept as small as possible while still enforcing necessary invariants. A Prescription aggregate does not contain the entire patient medication history; it contains only the data needed to enforce prescribing rules for that specific prescription. The complete medication regimen is managed through a domain service that coordinates across multiple Prescription aggregates.

## Aggregates by Bounded Context

### Diagnostic Assessment Context

**Psychiatric Evaluation (Aggregate Root)**
- Contains: evaluation date, presenting complaint, mental status examination findings, clinical impressions, assigned diagnoses, rating scale administrations, and evaluation status.
- Invariants: An evaluation must have at least one clinical impression before diagnoses can be assigned. Rating scale scores must be within valid ranges for the instrument. An evaluation cannot be finalized without a responsible clinician signature.

**Diagnostic Profile (Aggregate Root)**
- Contains: active diagnoses, historical diagnoses, diagnostic timeline, and diagnostic confidence levels.
- Invariants: A diagnosis cannot be simultaneously active and resolved. Each active diagnosis must reference the evaluation in which it was assigned.

### Medication Management Context

**Prescription (Aggregate Root)**
- Contains: medication identity, dosage, route, frequency, duration, prescribing clinician, titration schedule, monitoring requirements, and prescription status.
- Invariants: A controlled substance prescription must include DEA number verification. Dosage must be within FDA-approved ranges unless an override reason is documented. A prescription cannot be activated if a critical drug interaction exists without documented clinical justification.

**Medication Regimen (Aggregate Root)**
- Contains: list of active prescription references, polypharmacy risk score, last review date, and regimen status.
- Invariants: Polypharmacy risk must be recalculated when prescriptions are added or removed. The regimen must be reviewed at least every 90 days for patients on three or more psychotropics.

### Psychotherapy Context

**Therapy Course (Aggregate Root)**
- Contains: modality, treatment protocol reference, start date, estimated duration, treatment goals, assigned therapist, and course status.
- Invariants: A therapy course must have at least one treatment goal. The assigned modality must be evidence-based for the patient's primary diagnosis.

**Therapy Session (Aggregate Root)**
- Contains: session date, session type (individual, group, family), duration, interventions delivered, homework assigned, session notes, and session outcome rating.
- Invariants: Session duration must meet minimum requirements for the billing code. Session notes must be completed within the documentation compliance window.

### Crisis Management Context

**Crisis Event (Aggregate Root)**
- Contains: onset timestamp, presenting situation, risk assessment results, interventions performed, disposition, resolution timestamp, and follow-up plan.
- Invariants: A crisis event cannot be resolved without a documented disposition. If suicide risk is assessed as imminent, an involuntary hold must be initiated or a documented clinical justification for alternative action must exist.

**Safety Plan (Aggregate Root)**
- Contains: warning signs, coping strategies, social contacts for distraction, family or friends to contact, professionals to contact, means restriction steps, and reasons for living.
- Invariants: A safety plan must include at least one professional contact. Means restriction steps must be documented if lethal means access is identified.

### Rehabilitation Services Context

**Rehabilitation Plan (Aggregate Root)**
- Contains: rehabilitation goals, service categories, assigned providers, milestone definitions, plan start date, and review schedule.
- Invariants: A rehabilitation plan must be reviewed at least every six months. Each goal must have at least one associated milestone.

**Functional Milestone (Aggregate Root)**
- Contains: milestone description, target date, assessment criteria, current status, and evidence of achievement.
- Invariants: A milestone cannot be marked as achieved without documented evidence. Target dates must be within the associated rehabilitation plan period.

### Outcomes Measurement Context

**Outcome Report (Aggregate Root)**
- Contains: reporting period, included measurement administrations, calculated outcome scores, clinically significant change indicators, and report status.
- Invariants: An outcome report must include at least one validated measurement. Change scores must be calculated using the reliable change index appropriate for the instrument.

**Measurement Battery (Aggregate Root)**
- Contains: list of instruments, administration schedule, target population criteria, and battery status.
- Invariants: Each instrument in the battery must have established psychometric properties (reliability, validity). The administration schedule must specify measurement intervals.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 6 on aggregates.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 10 on aggregate design rules.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 5 on aggregate patterns.
