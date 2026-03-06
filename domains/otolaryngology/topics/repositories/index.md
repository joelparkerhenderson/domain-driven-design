# Repositories - Otolaryngology Domain

## Overview

Repositories provide abstractions for storing and retrieving aggregate roots, hiding the details of data persistence from the domain model. In the otolaryngology domain, repositories define the interface through which bounded contexts access their aggregates without coupling to specific storage technologies. The domain model expresses what data is needed; the repository abstracts how it is stored and retrieved.

## Repository Design Principles

Repositories in the otolaryngology domain follow these principles:

- **Aggregate root access only**: Repositories provide access to aggregate roots, not to individual entities or value objects within an aggregate. To access an operative note, you retrieve the Surgical Case aggregate through its repository.
- **Collection-like interface**: Repositories present a collection-oriented interface (add, find, remove) that hides persistence mechanics.
- **Domain-oriented queries**: Repository query methods use domain language, not database language. Methods are named "findByPatientAndDateRange" not "selectWherePatientIdEquals."
- **Infrastructure independence**: The repository interface is defined in the domain layer; implementations reside in the infrastructure layer.

## Repositories by Bounded Context

### Clinical Assessment Context

**ENTEncounterRepository**
- `findById(encounterId)`: Retrieve a specific encounter with all findings, reports, and impressions.
- `findByPatient(patientId)`: Retrieve all encounters for a patient, ordered by date.
- `findByPatientAndDateRange(patientId, startDate, endDate)`: Retrieve encounters within a time period.
- `findByProviderAndDate(providerId, date)`: Retrieve all encounters for a provider on a specific date (daily schedule view).
- `findPendingSignature(providerId)`: Retrieve encounters awaiting provider signature.
- `save(encounter)`: Persist a new or modified encounter.

### Surgical Management Context

**SurgicalCaseRepository**
- `findById(caseId)`: Retrieve a surgical case with all pre-operative, intra-operative, and post-operative details.
- `findByPatient(patientId)`: Retrieve all surgical cases for a patient.
- `findByScheduledDate(date)`: Retrieve all cases scheduled for a specific date (operative schedule).
- `findBySurgeon(surgeonId)`: Retrieve all cases assigned to a surgeon.
- `findAwaitingPathology()`: Retrieve cases with pending pathology results.
- `findByProcedureType(procedureType)`: Retrieve cases by procedure for outcomes analysis.
- `save(surgicalCase)`: Persist a new or modified surgical case.

### Voice Disorders Context

**VoiceAssessmentRepository**
- `findById(assessmentId)`: Retrieve a voice assessment with all measurements, scores, and recommendations.
- `findByPatient(patientId)`: Retrieve all voice assessments for a patient (longitudinal view).
- `findByTherapist(therapistId)`: Retrieve assessments associated with a specific therapist.
- `findActiveTherapyProtocols()`: Retrieve assessments with active voice therapy protocols.
- `save(voiceAssessment)`: Persist a new or modified assessment.

### Sleep Disorders Context

**SleepEvaluationRepository**
- `findById(evaluationId)`: Retrieve a sleep evaluation with all study results, prescriptions, and compliance records.
- `findByPatient(patientId)`: Retrieve all sleep evaluations for a patient.
- `findAwaitingStudyResults()`: Retrieve evaluations with pending polysomnography results.
- `findNonCompliantCPAP()`: Retrieve evaluations where CPAP compliance falls below the threshold.
- `findSurgicalCandidates()`: Retrieve evaluations where patients have failed conservative therapy.
- `save(sleepEvaluation)`: Persist a new or modified evaluation.

### Allergy Management Context

**AllergyProfileRepository**
- `findById(profileId)`: Retrieve an allergy profile with all test results, sensitivities, and treatment history.
- `findByPatient(patientId)`: Retrieve the allergy profile for a patient.
- `findActiveImmunotherapyProtocols()`: Retrieve profiles with active immunotherapy protocols.
- `findByAllergenSensitivity(allergenType)`: Retrieve profiles with sensitivity to a specific allergen category.
- `findDueForImmunotherapyDoseAdjustment()`: Retrieve profiles where immunotherapy dose review is scheduled.
- `save(allergyProfile)`: Persist a new or modified profile.

### Hearing Services Context

**AudiologicalRecordRepository**
- `findById(recordId)`: Retrieve an audiological record with all evaluations, fittings, and rehabilitation plans.
- `findByPatient(patientId)`: Retrieve the audiological record for a patient.
- `findCochlearImplantCandidates()`: Retrieve records where cochlear implant candidacy has been established.
- `findDueForHearingAidFollowUp()`: Retrieve records where hearing aid follow-up is scheduled.
- `findByHearingLossClassification(type, degree)`: Retrieve records by hearing loss characteristics for population analysis.
- `save(audiologicalRecord)`: Persist a new or modified record.

## Query Considerations

### Read-Only Queries (CQRS)

Following CQRS principles, complex read-only queries that span multiple aggregates or require specialized projections are handled by separate read models, not through repositories. For example, a dashboard showing "all patients with severe OSA who have failed CPAP and are awaiting surgical consultation" crosses aggregate boundaries and is better served by a dedicated query service.

### Domain-Specific Filtering

Repository query methods reflect clinical concerns. "FindNonCompliantCPAP" embeds domain knowledge about compliance thresholds. "FindAwaitingPathology" understands the expected timeline for pathology results. These domain-aware queries belong in the repository interface because they express domain concepts, even though their implementation involves data filtering.

### Temporal Queries

Many otolaryngology queries are temporal. Hearing evaluations are compared over years. Allergy treatment histories span seasons. Repository methods support date-range queries and "most recent" lookups to enable longitudinal clinical analysis.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 6 on repositories.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 12 on repository design.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 5 on repository patterns.
