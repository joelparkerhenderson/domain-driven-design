# Domain Events - Neurology Domain

## Overview

Domain events represent significant occurrences within the domain that domain experts care about. In the neurology domain, domain events capture clinically meaningful state changes that trigger actions in other parts of the system or in other bounded contexts. Domain events are immutable records of something that happened at a specific point in time.

Domain events are the primary mechanism for communication between bounded contexts in the neurology domain. When a neurological examination is completed, when a seizure is recorded, or when an imaging study is interpreted, these events propagate through the system to trigger downstream workflows.

## Domain Event Design Principles

### Named in Past Tense

Domain events describe something that has already occurred. They are named using past tense verbs: ExaminationCompleted, SeizureRecorded, ImagingStudyInterpreted. This naming convention reinforces that events are immutable facts.

### Carry Sufficient Data

Each domain event carries enough data for consumers to react without needing to query the source bounded context. This reduces coupling between contexts. However, events should not carry the entire aggregate state; they should include the essential facts about what happened.

### Temporal Ordering

Domain events include a timestamp and can be ordered chronologically. In neurology, temporal ordering is clinically significant because the sequence of events often determines clinical interpretation.

## Domain Events by Bounded Context

### Clinical Assessment Context

**ExaminationInitiated**
- Triggered when a neurological examination begins.
- Payload: examinationId, patientId, examinerId, encounterDate, chiefComplaint.
- Consumers: Neuroimaging Context (prepares for potential imaging orders).

**ExaminationCompleted**
- Triggered when all examination components are finalized and signed.
- Payload: examinationId, patientId, summaryFindings, clinicalImpression, referralRecommendations.
- Consumers: Neurodegenerative Disease Context, Epilepsy Management Context, Neuromuscular Disorders Context, Neurorehabilitation Context.

**CognitiveScreeningScored**
- Triggered when a cognitive screening instrument is completed.
- Payload: screeningId, patientId, instrument, totalScore, domainSubscores, assessmentDate.
- Consumers: Neurodegenerative Disease Context (monitors for cognitive decline).

**AbnormalFindingDetected**
- Triggered when a clinically significant abnormality is found during examination.
- Payload: findingId, patientId, findingType, severity, location, laterality.
- Consumers: Neuroimaging Context (may trigger urgent imaging), specialist contexts as appropriate.

### Neuroimaging Context

**ImagingStudyOrdered**
- Triggered when a new imaging study is ordered.
- Payload: orderId, patientId, modality, clinicalIndication, priority, requestedProtocol.
- Consumers: scheduling systems, imaging department workflow.

**ImagingStudyAcquired**
- Triggered when imaging acquisition is complete.
- Payload: studyId, patientId, modality, acquisitionDate, numberOfSeries, studyInstanceUID.
- Consumers: interpretation queue, quality assurance workflow.

**ImagingStudyInterpreted**
- Triggered when an imaging study has been read and a report is generated.
- Payload: studyId, patientId, modality, findings, impression, recommendations, criticalFlag.
- Consumers: Clinical Assessment, Neurodegenerative Disease, Epilepsy Management, Neuromuscular Disorders contexts.

**CriticalFindingIdentified**
- Triggered when a life-threatening or time-sensitive finding is discovered.
- Payload: studyId, patientId, criticalFinding, urgencyLevel, communicationRequired.
- Consumers: Clinical Assessment Context (immediate notification), all relevant specialist contexts.

### Neurodegenerative Disease Context

**DiseaseProgressionRecorded**
- Triggered when a new assessment shows measurable disease progression.
- Payload: progressionId, patientId, disease, previousStage, currentStage, assessmentDate.
- Consumers: Neurorehabilitation Context, Clinical Assessment Context.

**TreatmentProtocolModified**
- Triggered when a disease-modifying therapy is started, changed, or discontinued.
- Payload: protocolId, patientId, disease, previousTherapy, newTherapy, reason.
- Consumers: pharmacy integration, monitoring schedule adjustment.

**ClinicalMilestoneReached**
- Triggered when a patient reaches a significant disease milestone.
- Payload: milestoneId, patientId, disease, milestoneType, date, implications.
- Consumers: Neurorehabilitation Context, care planning teams.

### Epilepsy Management Context

**SeizureRecorded**
- Triggered when a seizure episode is documented.
- Payload: seizureId, patientId, classification, duration, triggers, postIctalState, dateTime.
- Consumers: AED management workflow, seizure frequency tracking.

**AEDRegimenAdjusted**
- Triggered when antiepileptic medication is added, changed, or removed.
- Payload: regimenId, patientId, medication, previousDose, newDose, reason.
- Consumers: pharmacy integration, therapeutic drug monitoring.

**SurgicalCandidacyDetermined**
- Triggered when a patient's surgical eligibility is evaluated.
- Payload: evaluationId, patientId, candidacyStatus, supportingEvidence, recommendations.
- Consumers: Neuroimaging Context (additional imaging may be needed).

### Neuromuscular Disorders Context

**DiagnosticWorkupCompleted**
- Triggered when the neuromuscular diagnostic evaluation reaches a conclusion.
- Payload: workupId, patientId, diagnosis, supportingEvidence, treatmentRecommendations.
- Consumers: treatment planning, Neurorehabilitation Context.

**GeneticTestResultReceived**
- Triggered when genetic testing results are available.
- Payload: resultId, patientId, gene, variant, pathogenicity, clinicalSignificance.
- Consumers: diagnostic workup, genetic counseling workflow.

### Neurorehabilitation Context

**RehabilitationEpisodeStarted**
- Triggered when a new rehabilitation episode begins.
- Payload: episodeId, patientId, diagnosis, baselineFunctionalStatus, goals.
- Consumers: therapy scheduling, outcome tracking.

**FunctionalOutcomeAssessed**
- Triggered when a standardized functional assessment is completed.
- Payload: assessmentId, patientId, instrument, score, previousScore, changeFromBaseline.
- Consumers: Neurodegenerative Disease Context, discharge planning.

**RehabilitationGoalAchieved**
- Triggered when a therapy goal is met.
- Payload: goalId, patientId, goalDescription, achievedDate, measuredOutcome.
- Consumers: care plan adjustment, discharge readiness evaluation.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 8 on domain events.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 8 on domain events.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 6 on domain events and integration.
