# Repositories - Neurology Domain

## Overview

Repositories provide an abstraction for storing and retrieving aggregate roots, hiding the details of the underlying persistence mechanism. In the neurology domain, repositories enable the domain model to remain pure and independent of infrastructure concerns such as databases, file systems, or external storage services.

Repositories operate exclusively on aggregate roots. There is no repository for a value object or a non-root entity within an aggregate. If you need to retrieve a TherapyGoal, you access it through the RehabilitationEpisode repository, not through a separate TherapyGoal repository.

## Repository Design Principles

### Collection-Oriented Interface

Repositories present a collection-like interface to the domain layer. They support operations such as adding a new aggregate, finding an aggregate by identity, finding aggregates matching criteria, and removing aggregates. The repository interface uses domain terminology, not database terminology.

### Aggregate Root Only

Each repository manages exactly one type of aggregate root. The repository ensures that the entire aggregate is loaded and saved as a unit, preserving the aggregate's invariants across persistence operations.

### Domain Language in Queries

Repository query methods use the ubiquitous language. Instead of generic methods like "findByField," neurology repositories expose methods such as "findByPatientAndDateRange" or "findActiveByDisease." This keeps the domain language present even in data access patterns.

### Infrastructure Independence

Repository interfaces are defined in the domain layer. Implementation details (SQL, document store, FHIR server) belong to the infrastructure layer. The domain model has no knowledge of how data is physically stored.

## Repositories by Bounded Context

### Clinical Assessment Context

**NeurologicalExaminationRepository**
- add(examination): persists a new neurological examination.
- findById(examinationId): retrieves a specific examination.
- findByPatient(patientId): retrieves all examinations for a patient.
- findByPatientAndDateRange(patientId, startDate, endDate): retrieves examinations within a date range.
- findRecentByPatient(patientId, count): retrieves the most recent examinations.

**CognitiveScreeningRepository**
- add(screening): persists a new cognitive screening result.
- findById(screeningId): retrieves a specific screening.
- findByPatient(patientId): retrieves all screenings for a patient.
- findByPatientAndInstrument(patientId, instrument): retrieves screenings by assessment tool.
- findWithDecline(patientId, thresholdChange): identifies screenings showing significant cognitive decline.

### Neuroimaging Context

**ImagingStudyRepository**
- add(study): persists a new imaging study record.
- findById(studyId): retrieves a specific study.
- findByAccessionNumber(accessionNumber): retrieves a study by DICOM accession number.
- findByPatient(patientId): retrieves all studies for a patient.
- findByPatientAndModality(patientId, modality): retrieves studies filtered by imaging type.
- findPendingInterpretation(): retrieves studies awaiting interpretation.
- findCritical(dateRange): retrieves studies with critical findings.

**NeurophysiologyStudyRepository**
- add(study): persists a new neurophysiology study.
- findById(studyId): retrieves a specific study.
- findByPatient(patientId): retrieves all neurophysiology studies for a patient.
- findByType(studyType): retrieves studies by type (EEG, EMG, NCS).
- findByPatientAndType(patientId, studyType): retrieves patient studies filtered by type.

### Neurodegenerative Disease Context

**DiseaseProgressionRepository**
- add(progression): persists a new disease progression record.
- findById(progressionId): retrieves a specific progression record.
- findByPatient(patientId): retrieves all progression records for a patient.
- findByPatientAndDisease(patientId, disease): retrieves progression for a specific disease.
- findWithRecentProgression(disease, period): identifies patients with recent disease progression.

**TreatmentProtocolRepository**
- add(protocol): persists a new treatment protocol.
- findById(protocolId): retrieves a specific protocol.
- findActiveByPatient(patientId): retrieves currently active treatment protocols.
- findByMedication(medication): retrieves all protocols using a specific medication.

### Epilepsy Management Context

**SeizureEpisodeRepository**
- add(episode): persists a new seizure episode.
- findById(episodeId): retrieves a specific episode.
- findByPatient(patientId): retrieves all seizure episodes for a patient.
- findByPatientAndDateRange(patientId, startDate, endDate): retrieves episodes within a period.
- countByPatientAndPeriod(patientId, period): calculates seizure frequency.
- findByClassification(classificationType): retrieves episodes by seizure type.

**AEDRegimenRepository**
- add(regimen): persists a new AED regimen.
- findById(regimenId): retrieves a specific regimen.
- findCurrentByPatient(patientId): retrieves the active AED regimen.
- findHistoryByPatient(patientId): retrieves the complete AED history.

### Neuromuscular Disorders Context

**DiagnosticWorkupRepository**
- add(workup): persists a new diagnostic workup.
- findById(workupId): retrieves a specific workup.
- findByPatient(patientId): retrieves all workups for a patient.
- findPendingResults(patientId): retrieves workups with outstanding test results.
- findByDiagnosis(diagnosis): retrieves workups by confirmed diagnosis.

### Neurorehabilitation Context

**RehabilitationEpisodeRepository**
- add(episode): persists a new rehabilitation episode.
- findById(episodeId): retrieves a specific episode.
- findByPatient(patientId): retrieves all rehabilitation episodes.
- findActiveByPatient(patientId): retrieves the current active episode.
- findByDiagnosisAndOutcome(diagnosis, outcomeThreshold): retrieves episodes for outcome analysis.

**FunctionalOutcomeRepository**
- add(outcome): persists a new functional outcome assessment.
- findById(outcomeId): retrieves a specific assessment.
- findByPatient(patientId): retrieves all assessments for a patient.
- findByPatientAndInstrument(patientId, instrument): retrieves assessments by scale type.
- findTrajectory(patientId, instrument): retrieves chronological scores for trend analysis.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 6 on repositories.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 12 on repository design.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 5 on persistence patterns.
