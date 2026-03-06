# Entities in the Mental Health Domain

## Overview

An entity is a domain object defined primarily by its unique identity rather
than its attributes. Entities persist over time and can change their state
while retaining the same identity. In contrast to value objects, two entities
with identical attributes but different identities are considered different
objects. In the mental health domain, entities represent the clinical objects
that have lifecycles, undergo state transitions, and must be tracked
individually across the system.

## Key Entities by Bounded Context

### Clinical Assessment Context

**Assessment**: The primary entity representing a clinical evaluation. Each
Assessment has a unique AssessmentId and progresses through a lifecycle:
initiated, in-progress, scored, interpreted, and finalized. The Assessment
entity tracks which clinician administered it, when it was started, and its
current state. Two assessments administered to the same patient using the
same instrument on the same day are distinct entities with different
identities.

**AssessmentItem**: An individual question or observation within an
assessment instrument. Each item has an identity within the scope of its
parent assessment. Items track the patient's response and any clinician
notes. Items are part of the Assessment aggregate and cannot exist
independently.

### Treatment Planning Context

**TreatmentPlan**: The central entity of this context. Each TreatmentPlan
has a unique TreatmentPlanId and evolves through states: draft, active,
under-review, revised, and discharged. The plan maintains its identity
even as goals are added, modalities change, and care levels are adjusted
over the course of treatment.

**TreatmentGoal**: A specific objective within a treatment plan. Each goal
has its own identity (TreatmentGoalId), a target condition, measurable
criteria, a target date, and a status (not started, in progress, achieved,
modified, discontinued). Goals are entities rather than value objects because
they have lifecycles and their progress is tracked over time.

### Therapy Management Context

**Session**: A scheduled or completed therapeutic encounter. Each Session
has a unique SessionId, a date and time, a duration, the modality used, and
a status (scheduled, in-progress, completed, cancelled, no-show). Sessions
are entities because each encounter is unique and must be individually
documented and tracked.

**ProgressNote**: The clinical documentation for a session. Each note has
a unique identity (ProgressNoteId), the clinician author, the documentation
timestamp, and a completion status. Notes are entities because they undergo
a lifecycle from draft to signed, and regulatory requirements demand
individual tracking.

### Crisis Intervention Context

**CrisisEpisode**: An entity representing a discrete crisis event. Each
episode has a CrisisEpisodeId, a start time, a severity level, a
disposition, and a resolution status. Crisis episodes are entities because
each event is unique, even if the same patient experiences multiple crises
with similar characteristics.

**SafetyPlan**: A personalized safety document created or updated during
crisis intervention. Each SafetyPlan has a unique identity (SafetyPlanId)
and tracks version history, last review date, and activation status. Safety
plans are entities because they evolve over time as the patient's
circumstances change.

### Medication Management Context

**Prescription**: A medication order with a unique PrescriptionId. Each
prescription tracks the medication, dosage, start date, prescribing
clinician, current status (active, on hold, discontinued, expired), and
the reason for any status changes. Prescriptions are entities because each
order is legally and clinically distinct.

**TitrationSchedule**: A planned series of dosage adjustments for a
medication. Each schedule has a unique identity and tracks planned steps,
completed steps, and the patient's response at each step. Titration
schedules are entities because their progression must be tracked over time.

### Outcomes Measurement Context

**OutcomeRecord**: An entity that tracks a patient's measurement data across
a treatment episode. Each OutcomeRecord has a unique OutcomeRecordId and
accumulates data points over time. The record maintains its identity from
the first measurement through treatment completion.

**MeasurementPoint**: A single administration of an outcome instrument at
a specific time. Each measurement point has a unique identity within the
outcome record and captures the date, instrument used, raw score, and
interpretation.

## Entity Design Principles

Entities in the mental health domain follow these design principles:

- Identity is assigned at creation and never changes.
- Equality is based on identity, not attributes.
- Entities encapsulate behavior related to their state transitions.
- Entities validate their own invariants during state changes.
- Entities that belong to an aggregate are accessed only through the
  aggregate root.

## Identity Generation

Entity identities in the mental health domain use system-generated
unique identifiers (UUIDs) rather than natural keys. Clinical identifiers
like medical record numbers are value objects attached to entities, not
the entities' primary identity. This protects against identity collisions
when integrating data from multiple source systems.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 5 on entities.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 5 on entities.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 5 on implementing domain models.
