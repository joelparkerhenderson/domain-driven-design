# Repositories in the Psychiatry Domain

## Overview

A repository is an abstraction that provides the illusion of an in-memory collection of aggregate roots. Repositories encapsulate the logic for storing, retrieving, and searching aggregates, shielding the domain model from persistence infrastructure details. In the psychiatry domain, repositories provide access to clinical aggregates such as psychiatric evaluations, prescriptions, therapy sessions, crisis events, rehabilitation plans, and outcome reports without exposing database schemas, query languages, or storage mechanisms.

## Design Principles

### Aggregate Root Access Only

Repositories provide access only to aggregate roots. Internal components of an aggregate are accessed through the root. A Psychiatric Evaluation repository returns complete Psychiatric Evaluation aggregates; it does not provide direct access to individual rating scale administrations within an evaluation. This enforcement maintains aggregate consistency invariants.

### Domain-Oriented Interface

Repository interfaces use domain language, not persistence language. A method is named "findActiveEvaluationsForPatient" rather than "selectWherePatientIdEqualsAndStatusEquals." The interface expresses clinical intent, and the implementation translates that intent into infrastructure-specific queries.

### Persistence Ignorance

The domain model has no knowledge of how aggregates are persisted. Repositories could be implemented against a relational database, a document store, an event store, or an in-memory collection. The domain model code remains unchanged regardless of the persistence mechanism chosen.

## Repositories by Bounded Context

### Diagnostic Assessment Context

**PsychiatricEvaluationRepository**
- save(evaluation): Persist a new or modified psychiatric evaluation aggregate.
- findById(evaluationId): Retrieve a specific evaluation by its unique identifier.
- findByPatientId(patientId): Retrieve all evaluations for a given patient, ordered by evaluation date.
- findActiveEvaluationsForClinician(clinicianId): Retrieve evaluations in progress for a specific evaluating clinician.
- findByDiagnosisCode(diagnosisCode): Retrieve evaluations that resulted in a specific diagnosis, useful for clinical research queries.

**DiagnosticProfileRepository**
- save(diagnosticProfile): Persist a new or modified diagnostic profile.
- findByPatientId(patientId): Retrieve the current diagnostic profile for a patient.
- findPatientsWithActiveDiagnosis(diagnosisCode): Retrieve all patient diagnostic profiles containing a specific active diagnosis.

### Medication Management Context

**PrescriptionRepository**
- save(prescription): Persist a new or modified prescription aggregate.
- findById(prescriptionId): Retrieve a specific prescription.
- findActiveByPatientId(patientId): Retrieve all active prescriptions for a patient.
- findByMedication(medicationIdentifier): Retrieve all prescriptions for a specific medication across patients.
- findRequiringMonitoring(monitoringDueDate): Retrieve prescriptions with therapeutic drug monitoring due by a specified date.

**MedicationRegimenRepository**
- save(regimen): Persist a new or modified medication regimen.
- findByPatientId(patientId): Retrieve the current medication regimen for a patient.
- findRequiringReview(reviewDueDate): Retrieve regimens due for scheduled review.

### Psychotherapy Context

**TherapyCourseRepository**
- save(therapyCourse): Persist a new or modified therapy course aggregate.
- findById(courseId): Retrieve a specific therapy course.
- findActiveByPatientId(patientId): Retrieve active therapy courses for a patient.
- findByTherapistId(therapistId): Retrieve all therapy courses assigned to a therapist.
- findByModality(modality): Retrieve therapy courses using a specific therapeutic modality.

**TherapySessionRepository**
- save(session): Persist a new or modified therapy session aggregate.
- findById(sessionId): Retrieve a specific session.
- findByCourseId(courseId): Retrieve all sessions within a therapy course, ordered by date.
- findByDateRange(therapistId, startDate, endDate): Retrieve sessions for a therapist within a date range.

### Crisis Management Context

**CrisisEventRepository**
- save(crisisEvent): Persist a new or modified crisis event aggregate.
- findById(crisisEventId): Retrieve a specific crisis event.
- findByPatientId(patientId): Retrieve all crisis events for a patient, ordered by activation timestamp.
- findActiveEvents(): Retrieve all currently unresolved crisis events across the system.
- findByDateRange(startDate, endDate): Retrieve crisis events within a reporting period.

**SafetyPlanRepository**
- save(safetyPlan): Persist a new or modified safety plan.
- findCurrentByPatientId(patientId): Retrieve the most recent active safety plan for a patient.
- findPatientsWithoutSafetyPlan(riskLevel): Retrieve high-risk patients who do not have a current safety plan.

### Rehabilitation Services Context

**RehabilitationPlanRepository**
- save(plan): Persist a new or modified rehabilitation plan.
- findById(planId): Retrieve a specific rehabilitation plan.
- findActiveByPatientId(patientId): Retrieve active rehabilitation plans for a patient.
- findRequiringReview(reviewDueDate): Retrieve plans due for scheduled review.

**FunctionalMilestoneRepository**
- save(milestone): Persist a new or modified functional milestone.
- findByPlanId(planId): Retrieve all milestones for a rehabilitation plan.
- findPendingByPatientId(patientId): Retrieve milestones not yet achieved for a patient.

### Outcomes Measurement Context

**OutcomeReportRepository**
- save(report): Persist a new or modified outcome report.
- findById(reportId): Retrieve a specific outcome report.
- findByPatientId(patientId): Retrieve all outcome reports for a patient.
- findByReportingPeriod(startDate, endDate): Retrieve reports within a period for aggregate analysis.

**MeasurementBatteryRepository**
- save(battery): Persist a new or modified measurement battery configuration.
- findById(batteryId): Retrieve a specific measurement battery.
- findByDiagnosis(diagnosisCode): Retrieve measurement batteries configured for a specific diagnosis.

## Query Considerations

Complex queries that span multiple aggregates or bounded contexts are handled through read models (CQRS pattern) rather than repository queries. For example, a clinical dashboard showing a patient's diagnoses, medications, therapy progress, and outcome scores would query a denormalized read model, not individual repositories. Repositories remain focused on aggregate-level persistence operations.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 6 on repositories.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 12 on repositories.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 5 on repositories.
