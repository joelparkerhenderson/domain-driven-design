# Aggregates in the Sleep Quality Domain

## Overview

Aggregates are clusters of related domain objects treated as a single unit for the purpose of data consistency. Each aggregate has a root entity that serves as the sole entry point for external access, and the aggregate boundary defines a consistency boundary within which all invariants must be satisfied before and after every operation. In the sleep quality domain, aggregates model the natural groupings of clinical, behavioral, and measurement data.

## Sleep Assessment Aggregate

The Sleep Assessment aggregate is the primary aggregate in the Sleep Assessment Context. Its root entity is the SleepAssessment, which groups a set of assessment instruments administered during an evaluation period. The aggregate contains value objects for individual instrument results (PSQIResult, ESSResult, ISIResult), actigraphy summaries, and polysomnography findings. The aggregate enforces invariants such as: a PSQI global score must be the correct sum of its seven component scores; an ESS total must not exceed 24; and all instruments within an assessment must reference the same evaluation period.

## Sleep Diary Aggregate

The Sleep Diary aggregate maintains a longitudinal self-report record of sleep patterns. The root entity is the SleepDiary, which contains an ordered collection of SleepDiaryEntry value objects. Each entry records bedtime, lights-out time, estimated sleep onset latency, number and duration of nighttime awakenings, final wake time, rise time, and subjective sleep quality rating. The aggregate enforces temporal ordering (entries must not overlap) and validates that derived metrics (sleep efficiency, total sleep time) are computed consistently from reported values.

## Disorder Diagnosis Aggregate

The Disorder Diagnosis aggregate in the Sleep Disorders Context models a clinician's diagnostic determination. The root entity is the DisorderDiagnosis, which references the specific ICSD-3 disorder classification, records which diagnostic criteria have been met, captures severity classification, and tracks the diagnosis lifecycle (suspected, confirmed, resolved). The aggregate enforces that a diagnosis cannot be confirmed unless the minimum required criteria are met according to the relevant diagnostic standard.

## Intervention Protocol Aggregate

The Intervention Protocol aggregate in the Behavioral Interventions Context models a structured treatment plan. The root entity is the InterventionProtocol, which contains an ordered sequence of treatment components (sessions, homework assignments, behavioral prescriptions). For a CBT-I protocol, the aggregate enforces the correct sequencing of components: psychoeducation before sleep restriction, sleep restriction parameters validated against diary data, and stimulus control instructions consistent with the protocol variant. The aggregate tracks completion status and adherence metrics.

## Device Feed Aggregate

The Device Feed aggregate in the Device Integration Context manages the relationship between an external device and the domain. The root entity is the DeviceFeed, which maintains device registration details, connection status, data format specifications, and the anti-corruption layer configuration for that device type. The aggregate contains a collection of DataIngestion entities that represent individual data retrieval sessions. The aggregate enforces that data is not accepted from unregistered or deactivated devices.

## Outcomes Record Aggregate

The Outcomes Record aggregate in the Outcomes Tracking Context maintains longitudinal outcome data for an individual. The root entity is the OutcomesRecord, which contains a time-ordered series of OutcomeMeasurement value objects. Each measurement captures a point-in-time snapshot of sleep quality metrics (sleep efficiency, WASO, total sleep time, subjective quality score, daytime functioning indicators). The aggregate computes trend statistics and enforces that measurements are chronologically ordered and free of duplicates.

## Environment Assessment Aggregate

The Environment Assessment aggregate in the Sleep Environment Context groups environmental readings and recommendations for a sleeping space. The root entity is the EnvironmentAssessment, which contains EnvironmentalReading value objects (light level, noise level, temperature, humidity) and associated Recommendation value objects. The aggregate enforces that recommendations are consistent with readings: for example, a light reduction recommendation should only be generated when the measured lux exceeds the evidence-based threshold.

## Aggregate Design Principles

Aggregates in the sleep quality domain follow several design principles. Aggregates are kept small, containing only the objects necessary to enforce consistency. References between aggregates use identity (IDs) rather than direct object references. Operations that span multiple aggregates are coordinated through domain events rather than transactional coupling. Aggregate boundaries are aligned with clinical workflow boundaries to minimize the need for cross-aggregate transactions.

## Concurrency and Consistency

Each aggregate defines a consistency boundary within which all changes are atomic. When a sleep diary entry is added, the aggregate recomputes derived metrics and validates invariants within a single transaction. Cross-aggregate consistency is achieved through eventual consistency via domain events. For example, when an assessment is completed, the event is published and the outcomes record is updated asynchronously.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley. Chapter 6 on aggregates.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley. Chapter 10 on aggregate design.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media. Chapter 10 on designing aggregates.
