# Repositories - Asthma Domain

## Overview

Repositories are abstractions for storing and retrieving aggregate roots. They provide a collection-like interface for accessing domain objects while hiding the details of data storage and retrieval. In the asthma management domain, repositories decouple the domain model from persistence technology, allowing clinical business logic to remain pure and independent of database, file system, or external service implementation details.

Repositories operate exclusively on aggregate roots. You retrieve a PatientAssessment aggregate through a PatientAssessmentRepository, not individual SpirometryManeuver entities. This constraint reinforces aggregate boundaries and ensures consistency rules are maintained during retrieval and persistence.

## Repository Design Principles

**Aggregate Root Access Only.** Repositories provide access to aggregate roots. Internal entities and value objects within an aggregate are accessed through the aggregate root, not through separate repositories. The SpirometrySession aggregate is retrieved as a whole, including its contained SpirometryManeuver entities and FEV1 value objects.

**Collection Semantics.** Repositories behave like in-memory collections. They support adding, removing, and querying aggregate roots. The domain layer treats the repository as if all aggregates are available in memory.

**No Infrastructure Leakage.** Repository interfaces are defined in the domain layer using domain concepts. The interface does not expose SQL, document IDs, API endpoints, or any persistence mechanism details. Implementation classes in the infrastructure layer handle these concerns.

**Specification Pattern.** Complex queries are expressed as specification objects that encapsulate search criteria in domain terms. A query for "all patients with uncontrolled asthma and declining FEV1 trend" is expressed as a domain specification, not a database query.

## Clinical Assessment Context Repositories

### PatientAssessmentRepository

**Purpose:** Store and retrieve PatientAssessment aggregates.

**Key Operations:**
- findById(assessmentId): Retrieve a specific assessment by its unique identifier.
- findByPatientId(patientId): Retrieve all assessments for a given patient, ordered by date.
- findLatestByPatientId(patientId): Retrieve the most recent finalized assessment for a patient.
- findByControlLevel(controlLevel, dateRange): Retrieve assessments matching a specific control level within a time period.
- save(patientAssessment): Persist a new or updated assessment aggregate.

### SpirometrySessionRepository

**Purpose:** Store and retrieve SpirometrySession aggregates with their contained maneuvers.

**Key Operations:**
- findById(sessionId): Retrieve a specific spirometry session.
- findByPatientId(patientId): Retrieve all spirometry sessions for a patient.
- findByDateRange(patientId, startDate, endDate): Retrieve sessions within a date range for trend analysis.
- save(spirometrySession): Persist a session with all its maneuvers and calculated values.

### PeakFlowRecordRepository

**Purpose:** Store and retrieve PeakFlowRecord aggregates.

**Key Operations:**
- findById(recordId): Retrieve a specific peak flow record.
- findByPatientId(patientId, dateRange): Retrieve peak flow records for a patient within a date range.
- findPersonalBest(patientId): Retrieve the record containing the patient's personal best PEF.
- save(peakFlowRecord): Persist a peak flow record.

## Treatment Management Context Repositories

### TreatmentPlanRepository

**Purpose:** Store and retrieve TreatmentPlan aggregates including medication assignments and step change history.

**Key Operations:**
- findById(treatmentPlanId): Retrieve a specific treatment plan.
- findActiveByPatientId(patientId): Retrieve the currently active treatment plan for a patient.
- findHistoryByPatientId(patientId): Retrieve all treatment plans (active and superseded) for a patient.
- findByTherapyStep(step): Retrieve all active treatment plans at a specific GINA step.
- save(treatmentPlan): Persist a treatment plan with all contained medication assignments and step change records.

### BiologicTherapyRepository

**Purpose:** Store and retrieve BiologicTherapy aggregates.

**Key Operations:**
- findById(therapyId): Retrieve a specific biologic therapy record.
- findActiveByPatientId(patientId): Retrieve the active biologic therapy for a patient.
- findByAgent(biologicAgent): Retrieve all therapies using a specific biologic agent.
- findDueForReview(reviewDate): Retrieve biologic therapies due for response assessment.
- save(biologicTherapy): Persist a biologic therapy with eligibility criteria and response assessments.

## Trigger Monitoring Context Repositories

### TriggerProfileRepository

**Purpose:** Store and retrieve TriggerProfile aggregates.

**Key Operations:**
- findById(profileId): Retrieve a specific trigger profile.
- findByPatientId(patientId): Retrieve the trigger profile for a patient.
- findByAllergen(allergenCode): Retrieve all profiles with sensitivity to a specific allergen.
- save(triggerProfile): Persist a trigger profile with allergen sensitivities.

### EnvironmentalExposureRepository

**Purpose:** Store and retrieve EnvironmentalExposure aggregates.

**Key Operations:**
- findById(exposureId): Retrieve a specific exposure record.
- findByLocation(location, dateRange): Retrieve environmental readings for a geographic area.
- findByPatientId(patientId, dateRange): Retrieve exposure records relevant to a patient.
- findExceedingThreshold(pollutant, threshold): Retrieve exposures where a pollutant exceeds a specified level.
- save(environmentalExposure): Persist an environmental exposure record.

## Action Planning Context Repositories

### AsthmaActionPlanRepository

**Purpose:** Store and retrieve AsthmaActionPlan aggregates.

**Key Operations:**
- findById(actionPlanId): Retrieve a specific action plan.
- findActiveByPatientId(patientId): Retrieve the currently active action plan for a patient.
- findExpiring(withinDays): Retrieve action plans approaching their review date.
- save(asthmaActionPlan): Persist an action plan with all zone definitions, medication instructions, and emergency contacts.

## Medication Management Context Repositories

### MedicationRegimenRepository

**Purpose:** Store and retrieve MedicationRegimen aggregates.

**Key Operations:**
- findById(regimenId): Retrieve a specific medication regimen.
- findActiveByPatientId(patientId): Retrieve the active medication regimen for a patient.
- findByMedication(medicationCode): Retrieve all active regimens containing a specific medication.
- findWithRefillDue(withinDays): Retrieve regimens where prescriptions need refilling soon.
- save(medicationRegimen): Persist a medication regimen with prescriptions and dosing schedules.

### AdherenceRecordRepository

**Purpose:** Store and retrieve AdherenceRecord aggregates.

**Key Operations:**
- findById(recordId): Retrieve a specific adherence record.
- findByPatientId(patientId, period): Retrieve adherence data for a patient over a measurement period.
- findBelowThreshold(adherenceThreshold): Retrieve records where adherence rate falls below a threshold.
- save(adherenceRecord): Persist an adherence record with missed dose events and calculated rates.

## Outcomes Tracking Context Repositories

### PatientOutcomeRecordRepository

**Purpose:** Store and retrieve PatientOutcomeRecord aggregates.

**Key Operations:**
- findById(outcomeRecordId): Retrieve a specific outcome record.
- findByPatientId(patientId): Retrieve the outcome record for a patient.
- findByACTScoreRange(minScore, maxScore): Retrieve records with ACT scores in a specified range.
- findWithDecliningTrend(metric, period): Retrieve records showing declining trends for a specific metric.
- save(patientOutcomeRecord): Persist an outcome record with ACT scores, trends, and summaries.

### ExacerbationEventRepository

**Purpose:** Store and retrieve ExacerbationEvent aggregates.

**Key Operations:**
- findById(exacerbationId): Retrieve a specific exacerbation event.
- findByPatientId(patientId, dateRange): Retrieve exacerbation events for a patient within a period.
- findBySeverity(severityLevel, dateRange): Retrieve exacerbation events of a specific severity.
- findByTrigger(triggerType, dateRange): Retrieve exacerbation events attributed to a specific trigger.
- save(exacerbationEvent): Persist an exacerbation event with severity, treatment, and contributing factors.

## Query Specifications

Complex domain queries are encapsulated in specification objects:

- **UncontrolledAsthmaSpecification:** Patients with ControlLevel = Uncontrolled and ACTScore < 16.
- **HighExacerbationRiskSpecification:** Patients with 2 or more exacerbations in the past 12 months.
- **AdherenceInterventionSpecification:** Patients with adherence rate below 50% and increasing rescue inhaler use.
- **BiologicReviewDueSpecification:** Patients on biologic therapy with response assessment overdue by more than 2 weeks.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 6 on repositories.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 12 on repositories.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 5 on repository pattern.
