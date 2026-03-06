# Domain Events - Sleep Health Metrics

## Overview

Domain events represent significant occurrences within the sleep health metrics domain that are meaningful to domain experts and may trigger actions in other bounded contexts. When a sleep study recording is completed, when epochs are scored, when quality metrics are calculated, or when an intervention outcome is assessed, these are domain events that propagate information across bounded context boundaries without creating direct coupling between contexts.

## Domain Event Design Principles

Domain events in this domain are immutable records of something that happened. Each event carries a unique identifier, a timestamp, the identity of the aggregate that produced it, and a payload of relevant data. Events are named in past tense to reflect that they describe completed occurrences. They are published asynchronously and consumed by interested bounded contexts through an event-driven architecture.

## Sleep Data Collection Context Events

### SleepStudyRecorded

Published when a polysomnographic recording session is completed and all channel data has been validated. Payload includes the StudyId, PatientId, study type (diagnostic, split-night, titration, MSLT, MWT), recording date, recording duration, number of channels, and overall signal quality assessment. This event triggers the Sleep Stage Analysis Context to begin scoring.

### SleepDiarySubmitted

Published when a patient submits a sleep diary entry. Payload includes the DiaryEntryId, PatientId, date, reported bedtime, lights-out time, estimated sleep onset time, number of awakenings, estimated WASO, final awakening time, out-of-bed time, and subjective quality rating. This event is consumed by the Sleep Quality Assessment Context for correlation with objective data.

### WearableDataSynced

Published when wearable device data is synchronized and ingested. Payload includes the SyncId, PatientId, DeviceId, device type, data date range, and data completeness indicator. This event informs the Sleep Quality Assessment Context that new data is available.

## Sleep Stage Analysis Context Events

### ScoringSessionCompleted

Published when epoch-by-epoch scoring of a sleep study is finalized. Payload includes the ScoringSessionId, StudyId, PatientId, scorer type (manual, automated, hybrid), total epochs scored, and a sleep architecture summary (stage percentages, TST, number of cycles). This event triggers quality metric computation in the Sleep Quality Assessment Context.

### RespiratoryEventsScored

Published when respiratory event scoring for a study is finalized. Payload includes the ScoringSessionId, StudyId, total apneas (obstructive, central, mixed), total hypopneas, computed AHI, RDI, oxygen desaturation index, and minimum SpO2. This event is consumed by the Sleep Quality Assessment and Clinical Integration Contexts.

### ArousalIndexComputed

Published when the arousal index for a scoring session is finalized. Payload includes the ScoringSessionId, StudyId, total arousals, arousal index (events per hour), and arousal cause distribution (spontaneous, respiratory, limb movement).

## Sleep Quality Assessment Context Events

### QualityMetricsCalculated

Published when sleep quality metrics for an assessment period are computed. Payload includes the AssessmentId, PatientId, assessment period, sleep efficiency, sleep latency, REM latency, WASO, TST, number of awakenings, and arousal index. This event is consumed by the Intervention Tracking Context for efficacy evaluation and the Clinical Integration Context for reporting.

### SubjectiveAssessmentScored

Published when a patient-reported outcome instrument is scored. Payload includes the AssessmentId, PatientId, instrument type (PSQI, ESS), component scores, global/total score, and clinical interpretation (normal, borderline, clinically significant).

### SleepQualityTrendDetected

Published when longitudinal analysis identifies a significant trend in sleep quality metrics. Payload includes the PatientId, metric type, trend direction (improving, stable, deteriorating), trend duration, statistical significance, and the metric values composing the trend.

## Circadian Rhythm Context Events

### CircadianPhaseAssessed

Published when a circadian phase assessment is completed. Payload includes the ProfileId, PatientId, estimated DLMO time, chronotype classification, circadian phase angle, and social jet lag estimate. This event enriches quality assessments and informs intervention timing.

### CircadianMisalignmentDetected

Published when significant misalignment is detected between circadian phase and actual sleep timing. Payload includes the PatientId, estimated biological night window, actual sleep window, misalignment magnitude in hours, and suggested corrective actions.

## Intervention Tracking Context Events

### InterventionStarted

Published when a new intervention module begins. Payload includes the InterventionPlanId, ModuleId, PatientId, intervention type, start date, prescribed parameters, and treating clinician.

### AdherenceReported

Published when therapy device adherence data is processed for a reporting period. Payload includes the AdherenceRecordId, PatientId, DeviceId, reporting period, average usage hours, compliance percentage, residual AHI, and compliance status.

### InterventionOutcomeAssessed

Published when treatment efficacy is evaluated against baseline metrics. Payload includes the InterventionPlanId, PatientId, baseline metrics, current metrics, improvement percentages, and clinical significance assessment.

## Clinical Integration Context Events

### ClinicalReportGenerated

Published when a sleep study report is generated for clinical consumption. Payload includes the ReportId, StudyId, PatientId, report type, and recipient information.

### FHIRResourcePublished

Published when domain data is successfully mapped and published to a FHIR server. Payload includes the MappingId, FHIR resource type, FHIR resource ID, and publication timestamp.

## Event Ordering and Idempotency

Events follow a natural ordering reflecting clinical workflow: recording, scoring, quality assessment, circadian assessment, intervention evaluation, and clinical reporting. Consumers must handle events idempotently, as at-least-once delivery is assumed. Each event carries sufficient information for the consumer to act independently without querying the producer.

## References

- Evans, E. (2003). *Domain-Driven Design*. Addison-Wesley. Chapter 8: Domain Events.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley. Chapter 8: Domain Events.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media. Chapter 11: Domain Events.
