# Repositories in the Mental Health Domain

## Overview

A repository is an abstraction that mediates between the domain model and the
data persistence layer, providing the illusion of an in-memory collection of
aggregate roots. Repositories encapsulate the logic needed to store, retrieve,
and search for aggregates, shielding the domain model from infrastructure
concerns such as database queries, ORM configurations, and data serialization.

In the mental health domain, repositories serve as the access points for each
bounded context's aggregate roots. The domain model interacts with repositories
using domain concepts (find an assessment by patient and date range, retrieve
active prescriptions for a patient) rather than database concepts (SQL joins,
table scans, index lookups).

## Repositories by Bounded Context

### Clinical Assessment Context

**AssessmentRepository**

Provides access to Assessment aggregates. Key operations include:

- findById(assessmentId): Retrieve a specific assessment by its unique
  identifier.
- findByPatient(patientId): Retrieve all assessments for a given patient,
  ordered by date.
- findByPatientAndInstrument(patientId, instrumentType): Retrieve
  assessments filtered by the instrument used (PHQ-9, GAD-7, structured
  interview).
- findPendingInterpretation(clinicianId): Retrieve assessments that have
  been scored but not yet interpreted by the assigned clinician.
- save(assessment): Persist a new or modified assessment.

### Treatment Planning Context

**TreatmentPlanRepository**

Provides access to TreatmentPlan aggregates. Key operations include:

- findById(treatmentPlanId): Retrieve a specific treatment plan.
- findActiveByPatient(patientId): Retrieve the currently active treatment
  plan for a patient. Only one plan should be active at a time.
- findDueForReview(beforeDate): Retrieve treatment plans whose review date
  falls before the specified date, enabling proactive plan reviews.
- findByDiagnosis(diagnosticCode): Retrieve plans that address a specific
  diagnosis, useful for clinical quality analysis.
- save(treatmentPlan): Persist a new or modified treatment plan.

### Therapy Management Context

**SessionRepository**

Provides access to Session aggregates. Key operations include:

- findById(sessionId): Retrieve a specific session.
- findByPatientAndDateRange(patientId, startDate, endDate): Retrieve
  sessions within a time window for a patient.
- findUpcomingByPatient(patientId): Retrieve future scheduled sessions.
- findAwaitingDocumentation(clinicianId): Retrieve completed sessions
  whose progress notes have not yet been signed.
- findByModality(modality): Retrieve sessions using a specific therapeutic
  modality.
- save(session): Persist a new or modified session.

### Crisis Intervention Context

**CrisisEpisodeRepository**

Provides access to CrisisEpisode aggregates. Key operations include:

- findById(crisisEpisodeId): Retrieve a specific crisis episode.
- findActiveByPatient(patientId): Retrieve any currently open crisis
  episodes for a patient.
- findByPatient(patientId): Retrieve the full crisis history for a patient.
- findByRiskLevel(riskLevel): Retrieve episodes at a specific risk level,
  useful for quality review and supervision.
- findRequiringFollowUp(beforeDate): Retrieve resolved episodes whose
  follow-up date is approaching or overdue.
- save(crisisEpisode): Persist a new or modified crisis episode.

### Medication Management Context

**PrescriptionRepository**

Provides access to Prescription aggregates. Key operations include:

- findById(prescriptionId): Retrieve a specific prescription.
- findActiveByPatient(patientId): Retrieve all active prescriptions for a
  patient, essential for drug interaction checking.
- findByMedication(medicationIdentifier): Retrieve prescriptions for a
  specific medication across patients, useful for safety alerts.
- findRequiringTitrationReview(beforeDate): Retrieve prescriptions with
  upcoming titration steps that need clinician review.
- findByPrescriber(clinicianId): Retrieve all prescriptions written by a
  specific clinician.
- save(prescription): Persist a new or modified prescription.

### Outcomes Measurement Context

**OutcomeRecordRepository**

Provides access to OutcomeRecord aggregates. Key operations include:

- findById(outcomeRecordId): Retrieve a specific outcome record.
- findByPatient(patientId): Retrieve all outcome records for a patient
  across treatment episodes.
- findByInstrument(instrumentType): Retrieve records using a specific
  measurement instrument.
- findWithClinicallySignificantChange(): Retrieve records where the most
  recent measurement crossed a clinically significant threshold.
- findByTreatmentEpisode(treatmentPlanId): Retrieve outcome records linked
  to a specific treatment plan.
- save(outcomeRecord): Persist a new or modified outcome record.

## Repository Design Principles

Repositories in the mental health domain follow these design principles:

- Aggregate root access only: repositories provide access to aggregate roots,
  never to internal entities or value objects directly. To access a
  TreatmentGoal, retrieve the TreatmentPlan aggregate through its repository.

- Domain-oriented queries: query methods use domain language, not database
  language. The method is findDueForReview, not selectWhereReviewDateLessThan.

- Encapsulated persistence: the repository interface belongs to the domain
  layer; the implementation belongs to the infrastructure layer. The domain
  model has no knowledge of databases, ORMs, or serialization formats.

- Collection semantics: repositories behave like in-memory collections. Adding
  an aggregate is like adding to a collection; retrieving one is like looking
  it up. The persistence mechanism is an implementation detail.

- No business logic: repositories do not contain domain logic. They store and
  retrieve; they do not validate, calculate, or enforce invariants.

## Query Separation

Following CQRS principles, repositories in the mental health domain may be
complemented by separate read models for complex queries that span multiple
aggregates. Clinical dashboards, population health reports, and caseload
summaries are better served by denormalized read models than by querying
aggregate repositories directly.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 6 on repositories.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 12 on repositories.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 5 on persistence.
