# Aggregates in the Mental Health Domain

## Overview

An aggregate is a cluster of related domain objects treated as a single unit
for the purpose of data changes and consistency enforcement. Each aggregate
has a root entity that controls access to all objects within the cluster and
enforces the aggregate's invariants. In DDD, aggregates define transactional
consistency boundaries: all changes within an aggregate are atomic, while
changes across aggregates are eventually consistent.

In the mental health domain, aggregates encapsulate the natural consistency
boundaries of clinical workflows. A treatment plan and its goals must be
consistent with each other at all times, but a treatment plan does not need
to be transactionally consistent with a therapy session happening
simultaneously.

## Aggregates by Bounded Context

### Clinical Assessment Context

**Assessment Aggregate** (root: Assessment)

The Assessment aggregate encapsulates a single clinical evaluation episode.
It includes the assessment instrument used (PHQ-9, GAD-7, structured
diagnostic interview), the patient's responses, the scoring results, and
the clinician's interpretation. Invariants include: an assessment cannot be
scored until all required items are completed; a diagnostic interpretation
must reference valid DSM-5 criteria; and an assessment cannot be modified
after it is finalized.

### Treatment Planning Context

**TreatmentPlan Aggregate** (root: TreatmentPlan)

The TreatmentPlan aggregate contains the plan itself along with its treatment
goals, selected modalities, care level determination, and review schedule.
Invariants include: every treatment plan must have at least one active
treatment goal; each goal must be linked to at least one diagnosis; the care
level must be clinically justified; and the plan must have a scheduled review
date. The treatment plan transitions through states: draft, active, under
review, revised, and discharged.

### Therapy Management Context

**Session Aggregate** (root: Session)

The Session aggregate encapsulates a single therapeutic encounter. It includes
the session metadata (date, duration, modality), the progress note, the
interventions applied, and the patient's response. Invariants include: a
session must be associated with an active treatment plan; the progress note
must be completed within a configurable time window after the session; and
the modality used must match one of the modalities specified in the
treatment plan.

### Crisis Intervention Context

**CrisisEpisode Aggregate** (root: CrisisEpisode)

The CrisisEpisode aggregate tracks a crisis event from detection through
resolution. It includes the risk assessment, the safety plan (if created or
updated), the interventions performed, the disposition decision, and the
follow-up plan. Invariants include: every crisis episode must have a risk
assessment; high-risk episodes must have a safety plan; and the disposition
must be documented before the episode can be closed.

### Medication Management Context

**Prescription Aggregate** (root: Prescription)

The Prescription aggregate manages the lifecycle of a psychotropic medication
order. It includes the medication, dosage, titration schedule, prescribing
rationale, and monitoring requirements. Invariants include: the medication
must pass drug interaction checks against the patient's current medication
list; dosage changes must follow the titration schedule; and discontinuation
requires a taper plan for medications that cannot be stopped abruptly.

### Outcomes Measurement Context

**OutcomeRecord Aggregate** (root: OutcomeRecord)

The OutcomeRecord aggregate captures a series of measurement data points
for a single patient over a treatment episode. It includes the instruments
administered, scores at each time point, and calculated change metrics.
Invariants include: measurements must use validated instruments; scores must
fall within the instrument's valid range; and time points must be ordered
chronologically.

## Aggregate Design Principles

The mental health domain applies several aggregate design principles
from Vernon (2013):

- Design small aggregates: each aggregate contains only the objects
  necessary to enforce its invariants. Patient demographics are not
  embedded in every aggregate; they are referenced by identity.
- Reference other aggregates by identity: the Session aggregate references
  the treatment plan by TreatmentPlanId, not by containing a full
  TreatmentPlan object.
- Use domain events for cross-aggregate communication: when an Assessment
  is finalized, it publishes an AssessmentCompleted event rather than
  directly modifying the TreatmentPlan.
- Enforce invariants within aggregate boundaries: the Prescription
  aggregate enforces drug interaction rules at the time of prescribing,
  not after the fact.

## Eventual Consistency Across Aggregates

When a crisis episode is opened, the CrisisEpisode aggregate publishes a
CrisisInitiated event. Other aggregates in other contexts respond
asynchronously: the Session aggregate may cancel upcoming sessions, the
TreatmentPlan aggregate may flag the plan for urgent review, and the
Prescription aggregate may trigger an emergency medication review. These
cross-aggregate updates are eventually consistent, not transactionally
atomic.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 6 on aggregates.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 10 on aggregate design rules.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 5 on aggregates and consistency.
