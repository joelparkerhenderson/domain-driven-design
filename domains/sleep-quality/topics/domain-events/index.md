# Domain Events in the Sleep Quality Domain

## Overview

Domain events represent significant occurrences within the domain that other parts of the system need to know about. They are named in past tense (something that has already happened) and carry the data necessary for consumers to react appropriately. In the sleep quality domain, domain events enable loose coupling between bounded contexts, allowing each context to evolve independently while maintaining coherent system-wide behavior.

## Sleep Assessment Context Events

### AssessmentCompleted

Published when a comprehensive sleep assessment has been fully scored and reviewed. Carries the assessment ID, patient ID, assessment date, instrument types administered, summary scores (PSQI global score, ESS total, ISI score if applicable), and overall sleep quality classification. Consumed by the Sleep Disorders Context for diagnostic input and by the Outcomes Tracking Context for baseline or follow-up measurement recording.

### SleepDiaryEntryRecorded

Published when a new sleep diary entry is submitted and validated. Carries the diary ID, patient ID, entry date, reported bedtime, lights-out time, estimated sleep onset latency, number of awakenings, wake after sleep onset, final wake time, rise time, and computed metrics (total sleep time, time in bed, sleep efficiency). Consumed by the Behavioral Interventions Context for treatment parameter adjustment and by the Outcomes Tracking Context for daily tracking.

### AcligraphyDataProcessed

Published when a period of actigraphy data has been processed into sleep-wake estimates. Carries the patient ID, recording period, estimated total sleep time, sleep efficiency, sleep onset latency, WASO, and fragmentation index. Consumed by the Assessment Context for incorporation into assessments and by the Outcomes Tracking Context for device-derived metrics.

## Sleep Disorders Context Events

### DisorderDiagnosed

Published when a clinician confirms a sleep disorder diagnosis. Carries the diagnosis ID, patient ID, disorder type (ICSD-3 classification), severity classification, diagnostic criteria met, and diagnosing clinician identifier. Consumed by the Behavioral Interventions Context for treatment eligibility determination and by the Outcomes Tracking Context for clinical milestone recording.

### DisorderSeverityChanged

Published when the severity classification of a diagnosed disorder changes based on new clinical data. Carries the diagnosis ID, patient ID, previous severity, new severity, and the assessment data that triggered the reclassification. Consumed by the Behavioral Interventions Context for treatment adjustment and by the Outcomes Tracking Context for trend recording.

### DiagnosisResolved

Published when a disorder diagnosis is clinically resolved. Carries the diagnosis ID, patient ID, resolution date, and resolution reason. Consumed by the Behavioral Interventions Context for protocol completion and by the Outcomes Tracking Context for milestone recording.

## Behavioral Interventions Context Events

### InterventionProtocolStarted

Published when a new intervention protocol is activated for a patient. Carries the protocol ID, patient ID, intervention type (CBT-I, sleep restriction, stimulus control), start date, target duration, and initial parameters. Consumed by the Outcomes Tracking Context for tracking the temporal relationship between interventions and outcomes.

### SessionCompleted

Published when an intervention session is completed. Carries the protocol ID, session ID, patient ID, session number, completion date, topics covered, and adherence indicators. Consumed by the Outcomes Tracking Context for intervention progress tracking.

### SleepRestrictionWindowAdjusted

Published when the prescribed time-in-bed window in a sleep restriction protocol is adjusted based on sleep diary data. Carries the protocol ID, patient ID, previous window, new window, and the sleep efficiency that triggered the adjustment. Consumed by the Outcomes Tracking Context.

## Device Integration Context Events

### DeviceDataReceived

Published when normalized device data is available from an external device. Carries the feed ID, device type, patient ID, data timestamp, data type (sleep staging, movement, respiratory, environmental), and the normalized data payload. Consumed by the relevant bounded context based on data type.

### DeviceConnectionStatusChanged

Published when a device feed's connection status changes (connected, disconnected, error). Carries the feed ID, device type, patient ID, previous status, new status, and timestamp. Consumed by the Outcomes Tracking Context for data completeness monitoring.

## Sleep Environment Context Events

### EnvironmentAssessmentCompleted

Published when an environmental assessment produces findings and recommendations. Carries the assessment ID, patient ID, location identifier, environmental readings summary, and generated recommendations. Consumed by the Behavioral Interventions Context for sleep hygiene integration.

## Outcomes Tracking Context Events

### OutcomeTrendIdentified

Published when longitudinal analysis identifies a significant trend in sleep quality outcomes. Carries the record ID, patient ID, metric type, trend direction (improving, stable, deteriorating), trend duration, and statistical confidence. Consumed by clinician notification systems and the Behavioral Interventions Context for treatment review triggers.

### QualityMilestoneReached

Published when a patient reaches a predefined quality milestone (e.g., sleep efficiency consistently above 85 percent for two weeks). Carries the record ID, patient ID, milestone type, achievement date, and supporting metrics.

## Event Design Principles

Domain events in the sleep quality domain are immutable records of facts. They carry enough data for consumers to act without querying back to the producer. Events use the ubiquitous language of the producing context. Timestamps use UTC. Patient identifiers are consistent across contexts. Event schemas are versioned to support evolution without breaking consumers.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley. Chapter 8 on domain events.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley. Chapter 8 on domain events.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media. Chapter 11 on domain events.
