# Aggregates - Neurology Domain

## Overview

Aggregates are clusters of related domain objects treated as a single unit for data consistency. Each aggregate has a root entity that serves as the sole entry point for external references. In the neurology domain, aggregates enforce the clinical invariants that must hold true within each bounded context.

Aggregate design in neurology must balance clinical completeness with transactional boundaries. A neurological examination contains many components, but not all components must be consistent within the same transaction. Proper aggregate boundaries prevent unnecessarily large consistency scopes.

## Aggregate Design Principles

### Consistency Boundaries

Each aggregate enforces invariants that must be true at all times within its boundary. For example, a NeurologicalExamination aggregate ensures that a GCS score has all three components (eye, verbal, motor) before it can be considered valid. These invariants reflect real clinical rules.

### Small Aggregates

Following Vernon's guidance (2013), aggregates should be as small as possible while still enforcing their invariants. In neurology, this means a complete neurological examination is not a single aggregate; rather, it is composed of multiple smaller aggregates (CranialNerveExamination, MotorExamination, SensoryExamination) that can be completed and validated independently.

### Reference by Identity

Aggregates reference other aggregates by identity (ID) rather than direct object references. A SeizureEpisode in the Epilepsy Management Context references a PatientId, not the full Patient aggregate from Clinical Assessment.

## Aggregates by Bounded Context

### Clinical Assessment Context

**NeurologicalExamination** (Aggregate Root)
- Enforces that examination date, examiner, and patient reference are present.
- Contains CranialNerveFindings, MotorFindings, SensoryFindings, ReflexFindings, CoordinationFindings as value objects.
- Invariant: an examination must have at least one completed component to be persisted.

**CognitiveScreening** (Aggregate Root)
- Enforces scoring rules for MMSE (0-30) and MoCA (0-30).
- Contains individual domain scores as value objects.
- Invariant: total score must equal the sum of domain subscores.

### Neuroimaging Context

**ImagingStudy** (Aggregate Root)
- Enforces that a study has an ordering provider, clinical indication, and modality.
- Contains StudyProtocol, AcquisitionParameters as value objects.
- Invariant: a study cannot transition to "interpreted" status without an associated report.

**NeurophysiologyStudy** (Aggregate Root)
- Encompasses EEG, EMG, and NCS studies.
- Contains RecordingParameters, Channels, and Findings as value objects.
- Invariant: a study must have at least one recording channel with valid data.

### Neurodegenerative Disease Context

**DiseaseProgression** (Aggregate Root)
- Tracks longitudinal disease trajectory for a specific condition and patient.
- Contains ProgressionMilestones, BiomarkerMeasurements, and TreatmentEpisodes.
- Invariant: milestones must be chronologically ordered.

**TreatmentProtocol** (Aggregate Root)
- Manages a disease-modifying therapy protocol for a patient.
- Contains DosageSchedule, MonitoringSchedule, and AdverseEventRecords.
- Invariant: monitoring schedule must comply with the medication's safety protocol.

### Epilepsy Management Context

**SeizureEpisode** (Aggregate Root)
- Records a single seizure event with ILAE classification.
- Contains SeizureClassification, Duration, Triggers, and PostictalState as value objects.
- Invariant: classification must follow ILAE onset type hierarchy.

**AEDRegimen** (Aggregate Root)
- Manages a patient's antiepileptic drug regimen.
- Contains individual AEDPrescription entries with dosage and titration schedules.
- Invariant: total daily dose must not exceed maximum recommended dose per medication.

### Neuromuscular Disorders Context

**DiagnosticWorkup** (Aggregate Root)
- Coordinates the diagnostic evaluation for a suspected neuromuscular disorder.
- Contains ClinicalFindings, ElectrodiagnosticResults, LaboratoryResults, and GeneticTestResults.
- Invariant: a diagnostic conclusion requires at least clinical findings and one confirmatory test.

### Neurorehabilitation Context

**RehabilitationEpisode** (Aggregate Root)
- Tracks a single episode of rehabilitation care.
- Contains FunctionalAssessments, TherapyGoals, and TherapySessions.
- Invariant: discharge assessment must be completed before episode can be closed.

**FunctionalOutcome** (Aggregate Root)
- Records a standardized functional assessment (FIM, mRS, Barthel Index).
- Contains domain subscores and total score.
- Invariant: total score must fall within the valid range for the assessment instrument.

## Aggregate Lifecycle

Neurology aggregates follow clinical lifecycle patterns:

1. **Creation**: initiated by a clinical event (examination started, study ordered, seizure reported).
2. **Progression**: enriched as clinical data accumulates (findings added, results received).
3. **Completion**: finalized when clinical workflow is complete (examination signed, study interpreted).
4. **Archival**: retained for longitudinal reference but no longer actively modified.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 6 on aggregates.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 10 on aggregate design rules.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 5 on aggregate patterns.
