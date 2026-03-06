# Repositories - Sleep Health Metrics

## Overview

Repositories provide abstractions for persisting and retrieving aggregate roots, shielding the domain model from infrastructure concerns. In the sleep health metrics domain, repositories define how sleep studies, scoring sessions, quality assessments, circadian profiles, intervention plans, and clinical reports are stored and retrieved. They express retrieval operations using domain concepts -- finding a sleep study by patient and date, retrieving quality assessments within a date range, locating intervention plans by disorder type -- without exposing database query details.

## Repository Design Principles

Repositories in this domain follow DDD conventions: each aggregate root has its own repository; repositories operate on whole aggregates (not individual entities or value objects within an aggregate); retrieval methods are expressed in ubiquitous language terms; and the repository interface belongs to the domain layer while the implementation belongs to the infrastructure layer. This separation keeps business logic pure and independent of storage technology.

## Sleep Data Collection Context Repositories

### SleepStudyRepository

The SleepStudyRepository provides persistence and retrieval for SleepStudy aggregates.

- findById(StudyId): Retrieves a complete sleep study with all recording channels and signal quality assessments.
- findByPatientAndDateRange(PatientId, DateRange): Retrieves all studies for a patient within a specified date range.
- findByStudyType(StudyType): Retrieves studies filtered by type (diagnostic PSG, split-night, titration, MSLT, MWT).
- findPendingScoring(): Retrieves studies that have been recorded but not yet scored.
- save(SleepStudy): Persists a new or updated sleep study aggregate.

### SleepDiaryRepository

The SleepDiaryRepository provides persistence and retrieval for SleepDiaryEntry aggregates.

- findById(DiaryEntryId): Retrieves a single diary entry.
- findByPatientAndDateRange(PatientId, DateRange): Retrieves all diary entries for a patient within a date range.
- findByPatientMostRecent(PatientId, count): Retrieves the most recent entries for longitudinal review.
- save(SleepDiaryEntry): Persists a new diary entry.

## Sleep Stage Analysis Context Repositories

### ScoringSessionRepository

The ScoringSessionRepository provides persistence and retrieval for ScoringSession aggregates.

- findById(ScoringSessionId): Retrieves a complete scoring session with all scored epochs and architecture summary.
- findByStudyId(StudyId): Retrieves all scoring sessions for a given study (supporting multiple scoring attempts or inter-scorer comparison).
- findByScorerType(ScorerType): Retrieves sessions filtered by scorer type (manual, automated, hybrid).
- findByPatientAndDateRange(PatientId, DateRange): Retrieves scoring sessions for a patient within a date range.
- save(ScoringSession): Persists a new or updated scoring session.

### RespiratoryEventSummaryRepository

The RespiratoryEventSummaryRepository provides persistence and retrieval for RespiratoryEventSummary aggregates.

- findByScoringSessionId(ScoringSessionId): Retrieves the respiratory event summary for a scoring session.
- findByPatientWithSevereAHI(PatientId, ahiThreshold): Retrieves summaries where AHI exceeds a clinical threshold.
- save(RespiratoryEventSummary): Persists a new respiratory event summary.

## Sleep Quality Assessment Context Repositories

### QualityAssessmentRepository

The QualityAssessmentRepository provides persistence and retrieval for QualityAssessment aggregates.

- findById(AssessmentId): Retrieves a complete quality assessment with all computed metrics.
- findByPatientAndDateRange(PatientId, DateRange): Retrieves assessments for trend analysis.
- findByEfficiencyBelow(PatientId, threshold): Retrieves assessments where sleep efficiency falls below a clinical threshold.
- findByPatientMostRecent(PatientId): Retrieves the most recent assessment for clinical review.
- save(QualityAssessment): Persists a new quality assessment.

### SubjectiveAssessmentRepository

The SubjectiveAssessmentRepository provides persistence and retrieval for SubjectiveAssessment aggregates.

- findById(AssessmentId): Retrieves a scored subjective assessment.
- findByPatientAndInstrument(PatientId, InstrumentType): Retrieves all assessments of a specific type (PSQI, ESS) for a patient.
- findClinicallySignificant(PatientId): Retrieves assessments where scores exceed clinical significance thresholds.
- save(SubjectiveAssessment): Persists a new scored assessment.

## Circadian Rhythm Context Repositories

### CircadianProfileRepository

The CircadianProfileRepository provides persistence and retrieval for CircadianProfile aggregates.

- findById(ProfileId): Retrieves a complete circadian profile with light exposure data and phase markers.
- findByPatient(PatientId): Retrieves all circadian profiles for a patient.
- findByChronotype(ChronotypeClassification): Retrieves profiles filtered by chronotype for population analysis.
- findWithSignificantSocialJetLag(threshold): Retrieves profiles where social jet lag exceeds a threshold.
- save(CircadianProfile): Persists a new or updated profile.

## Intervention Tracking Context Repositories

### InterventionPlanRepository

The InterventionPlanRepository provides persistence and retrieval for InterventionPlan aggregates.

- findById(InterventionPlanId): Retrieves a complete plan with all intervention modules.
- findByPatient(PatientId): Retrieves all intervention plans for a patient.
- findActiveByPatient(PatientId): Retrieves currently active plans.
- findByInterventionType(InterventionType): Retrieves plans containing a specific intervention modality.
- save(InterventionPlan): Persists a new or updated plan.

### AdherenceRecordRepository

The AdherenceRecordRepository provides persistence and retrieval for AdherenceRecord aggregates.

- findByPatientAndDateRange(PatientId, DateRange): Retrieves adherence data for a reporting period.
- findNonCompliant(PatientId): Retrieves records where adherence falls below CMS compliance criteria.
- save(AdherenceRecord): Persists a new adherence record.

## Clinical Integration Context Repositories

### ClinicalReportRepository

The ClinicalReportRepository provides persistence and retrieval for ClinicalReport aggregates.

- findById(ReportId): Retrieves a complete clinical report.
- findByStudyId(StudyId): Retrieves reports associated with a specific sleep study.
- findByPatientAndDateRange(PatientId, DateRange): Retrieves reports for a patient within a date range.
- findPendingReview(): Retrieves reports awaiting clinician review and signature.
- save(ClinicalReport): Persists a new or updated report.

## Repository Invariants

All repositories enforce that only complete, valid aggregates are persisted. Partial aggregates or aggregates in an invalid state are rejected. Repositories do not expose internal aggregate structure -- callers retrieve and persist whole aggregates, not individual internal entities.

## References

- Evans, E. (2003). *Domain-Driven Design*. Addison-Wesley. Chapter 6: Repositories.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley. Chapter 12: Repositories.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media. Chapter 10: Repositories.
