# Bounded Contexts in the Mental Health Domain

## Overview

A bounded context is a logical boundary within which a particular domain model
is defined and applicable. Inside a bounded context, every term has a precise,
unambiguous meaning, and the model is internally consistent. The mental health
domain is decomposed into six bounded contexts, each encapsulating a distinct
area of clinical workflow with its own rules, vocabulary, and consistency
requirements.

Bounded contexts are the most fundamental strategic design pattern in DDD. They
prevent the creation of a single, monolithic model that attempts to represent
all aspects of mental health care simultaneously, which would inevitably lead
to conflicting definitions, tangled dependencies, and a model that satisfies
no one.

## The Six Bounded Contexts

### 1. Clinical Assessment Context

This context owns the full lifecycle of clinical evaluation, from initial
screening through comprehensive diagnostic assessment. It manages standardized
instruments such as the PHQ-9 for depression, the GAD-7 for anxiety, and
structured diagnostic interviews based on DSM-5 criteria. The key aggregate
is the Assessment, which encapsulates the instrument used, the patient's
responses, the scoring algorithm, and the resulting clinical interpretation.

### 2. Treatment Planning Context

This context governs the creation, review, and revision of individualized
treatment plans. It consumes diagnostic information from the Clinical
Assessment Context and translates it into actionable goals, selected treatment
modalities, care level determinations, and review schedules. The Treatment
Plan aggregate is a living document that evolves as the patient progresses
through care.

### 3. Therapy Management Context

This context manages the delivery of therapeutic services. It handles session
scheduling, progress note documentation, therapeutic modality implementation
(Cognitive Behavioral Therapy, Dialectical Behavior Therapy, Eye Movement
Desensitization and Reprocessing), and tracking of the therapeutic alliance.
The Session aggregate captures what happens during each clinical encounter.

### 4. Crisis Intervention Context

This context addresses acute psychiatric emergencies. It owns risk assessment
protocols, safety plan creation, crisis hotline workflows, and coordination
with emergency departments and inpatient facilities. This context operates
under the tightest time constraints and the highest availability requirements.
The Crisis Episode aggregate tracks the full arc of a crisis event from
detection through resolution.

### 5. Medication Management Context

This context handles psychotropic medication prescribing, dosage titration,
drug interaction checking, adherence monitoring, and side effect tracking.
It interfaces with pharmacy systems and formularies through anti-corruption
layers. The Prescription aggregate manages the lifecycle of a medication
order from initial prescribing through discontinuation.

### 6. Outcomes Measurement Context

This context tracks treatment effectiveness over time. It collects symptom
measures at regular intervals, performs functional assessments, aggregates
patient-reported outcomes, and generates effectiveness reports. The Outcome
Record aggregate captures longitudinal data points tied to specific treatment
episodes.

## Boundary Enforcement

Each bounded context enforces its boundary through several mechanisms:

- Separate ubiquitous language subsets: "assessment" in the Clinical Assessment
  Context means a diagnostic evaluation; in the Outcomes Measurement Context
  it means a repeated symptom measure.
- Independent aggregate roots: each context defines its own aggregates with
  context-specific invariants.
- Explicit interfaces: contexts communicate through domain events, published
  languages, or anti-corruption layers rather than direct model sharing.
- Team alignment: ideally, each bounded context is owned by a team that
  includes both domain experts (clinicians) and developers.

## Relationships Between Contexts

Contexts do not operate in isolation. The context map defines their
relationships:

- Clinical Assessment feeds diagnostic data to Treatment Planning.
- Treatment Planning directs Therapy Management and Medication Management.
- Crisis Intervention can interrupt and override workflows in other contexts.
- Outcomes Measurement subscribes to events from all contexts.

## Benefits in Mental Health Systems

- Clinicians working in crisis intervention are not burdened with outcome
  reporting concepts that are irrelevant to their immediate workflow.
- Medication management can evolve its drug interaction models independently
  of therapy session scheduling.
- Regulatory requirements that differ by context (e.g., psychotherapy notes
  under HIPAA have additional protections) can be addressed locally.
- New assessment instruments can be added to the Clinical Assessment Context
  without affecting other contexts.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 14 on maintaining model integrity.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 2 on bounded contexts.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 6 on bounded contexts.
