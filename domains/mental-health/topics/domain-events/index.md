# Domain Events in the Mental Health Domain

## Overview

A domain event is a record of something significant that happened in the
domain. Domain events are named in the past tense (AssessmentCompleted,
CrisisInitiated, MedicationPrescribed) because they describe facts that
have already occurred. They are the primary mechanism for communication
between bounded contexts in an event-driven architecture, enabling loose
coupling while preserving the causal chain of clinical workflows.

In the mental health domain, domain events capture the clinically significant
moments that trigger downstream actions: a completed assessment that initiates
treatment planning, a crisis episode that suspends scheduled sessions, a
medication change that requires outcome reassessment.

## Domain Events by Bounded Context

### Clinical Assessment Context

**AssessmentInitiated**: Published when a new assessment is started. Carries
the patient identifier, assessment type, instrument selected, and initiating
clinician. Consumers may use this to prepare downstream workflows.

**AssessmentCompleted**: Published when an assessment is scored, interpreted,
and finalized. Carries the patient identifier, diagnostic codes, severity
ratings, and clinical formulation summary. This is the key event that triggers
treatment planning.

**ScreeningFlagged**: Published when a screening instrument produces a score
above the clinical threshold. Carries the instrument name, score, threshold
exceeded, and urgency level. May trigger expedited assessment or crisis
evaluation.

### Treatment Planning Context

**TreatmentPlanCreated**: Published when a new treatment plan is drafted and
approved. Carries the plan identifier, patient identifier, diagnoses,
selected modalities, care level, and goal summaries.

**TreatmentPlanRevised**: Published when an existing plan is updated after
review. Carries the plan identifier, change summary, and updated goals.
Downstream contexts adjust their workflows accordingly.

**CareEscalated**: Published when a patient's care level is increased (e.g.,
from outpatient to intensive outpatient). Carries the previous and new care
levels, the clinical justification, and the effective date.

### Therapy Management Context

**SessionCompleted**: Published after a therapy session is documented with a
signed progress note. Carries the session identifier, date, modality used,
interventions applied, and patient response summary.

**SessionCancelled**: Published when a scheduled session is cancelled. Carries
the session identifier, cancellation reason, and whether it was patient or
provider initiated. Relevant for no-show tracking and outcome analysis.

**TherapeuticAllianceRated**: Published when a clinician or patient rates the
quality of the therapeutic relationship. Carries the rating instrument, score,
and session reference.

### Crisis Intervention Context

**CrisisInitiated**: Published when a patient enters a crisis state. This is
a high-priority event that other contexts must handle with urgency. Carries
the patient identifier, crisis type, initial risk level, and responding
clinician.

**SafetyPlanCreated**: Published when a new safety plan is established or
an existing one is significantly revised. Carries the plan identifier, key
coping strategies, and emergency contacts.

**CrisisResolved**: Published when a crisis episode reaches a stable
disposition. Carries the episode identifier, resolution type (stabilized,
hospitalized, referred), and follow-up plan.

### Medication Management Context

**MedicationPrescribed**: Published when a new psychotropic medication is
ordered. Carries the prescription identifier, medication, dosage, prescribing
rationale, and expected titration schedule.

**DosageAdjusted**: Published when a medication dosage is changed as part
of titration or clinical response. Carries the prescription identifier,
previous dosage, new dosage, and reason for adjustment.

**MedicationDiscontinued**: Published when a medication is stopped. Carries
the prescription identifier, discontinuation reason, and taper plan if
applicable.

**DrugInteractionDetected**: Published when a clinically significant drug
interaction is identified. Carries the interacting medications, severity
level, and recommended action.

### Outcomes Measurement Context

**OutcomeMeasured**: Published when a standardized outcome measure is
administered and scored. Carries the instrument, score, previous score,
change metric, and clinical significance assessment.

**ClinicallySignificantChange**: Published when a patient's outcome scores
cross the threshold for reliable clinical improvement or deterioration.
Carries the instrument, direction of change, and magnitude.

**TreatmentEffectivenessReported**: Published when an effectiveness report
is generated for a treatment episode. Carries summary statistics and
comparative benchmarks.

## Event Design Principles

Domain events in the mental health domain follow these principles:

- Events are immutable facts: once published, an event cannot be changed.
  If a correction is needed, a new compensating event is published.
- Events carry enough context for consumers to act without querying the
  source context. This reduces coupling and improves resilience.
- Events use the ubiquitous language of the publishing context.
- Events include a timestamp, a correlation identifier for tracing, and
  the identity of the actor who caused the event.
- Sensitive clinical information in events is protected according to
  HIPAA and 42 CFR Part 2 requirements, with appropriate access controls
  on event streams.

## Event Ordering and Causality

Clinical workflows have causal dependencies that event ordering must
respect. An AssessmentCompleted event must be processed before a
TreatmentPlanCreated event that references its results. The system uses
causal ordering (per patient) rather than total ordering to balance
correctness with scalability.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 8 on domain events.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 8 on domain events.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 6 on domain events.
