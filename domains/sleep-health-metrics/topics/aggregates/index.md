# Aggregates - Sleep Health Metrics

## Overview

Aggregates are clusters of related domain objects treated as a single unit for data consistency. Each aggregate has a root entity that controls access to its internal objects and enforces invariants. In the sleep health metrics domain, aggregates define transactional consistency boundaries around sleep studies, scoring sessions, quality assessments, circadian profiles, intervention plans, and clinical reports.

## Aggregate Design Principles

Aggregates in this domain follow Evans' and Vernon's guidance: keep aggregates small, protect invariants within the boundary, reference other aggregates by identity only, and use eventual consistency across aggregate boundaries. Sleep health data has natural consistency boundaries -- a single night's recording, a single scoring session, a single assessment period -- that map well to aggregate boundaries.

## Sleep Data Collection Context Aggregates

### SleepStudy Aggregate

The SleepStudy aggregate is the root of all recorded sleep data for a single study session. It contains the recording metadata (study date, recording start/end times, study type), a collection of RecordingChannel entities (EEG, EOG, EMG, ECG, respiratory, oximetry), and signal quality assessments for each channel. The aggregate enforces invariants such as: a valid PSG study must include at minimum the required AASM montage channels; recording duration must meet the minimum threshold for clinical interpretation; all channels must share synchronized timestamps.

### SleepDiaryEntry Aggregate

The SleepDiaryEntry aggregate captures a single diary submission by a patient. It includes bedtime, lights-out time, estimated sleep onset, nighttime awakenings (count and estimated duration), final awakening time, out-of-bed time, subjective sleep quality rating, and free-text notes about factors affecting sleep (caffeine, alcohol, exercise, stress). Invariants include: lights-out time must be after bedtime; final awakening must be before out-of-bed time.

## Sleep Stage Analysis Context Aggregates

### ScoringSession Aggregate

The ScoringSession aggregate represents the complete epoch-by-epoch scoring of a sleep study. It references the source SleepStudy by identity and contains a collection of ScoredEpoch value objects, each with a stage classification and confidence indicator. The aggregate computes sleep architecture metrics (stage percentages, stage durations, number of stage transitions) and maintains the hypnogram representation. Invariants include: every epoch in the recording period must be scored; epochs must be contiguous without gaps; stage transitions must follow AASM-permissible sequences.

### RespiratoryEventSummary Aggregate

The RespiratoryEventSummary aggregate captures all respiratory events detected during a scoring session: apneas (obstructive, central, mixed), hypopneas, and respiratory effort-related arousals (RERAs). It computes the AHI, RDI (Respiratory Disturbance Index), and oxygen desaturation index. The invariant is that every event must fall within the scored recording period.

## Sleep Quality Assessment Context Aggregates

### QualityAssessment Aggregate

The QualityAssessment aggregate computes and stores sleep quality metrics for a specific assessment period (single night or multi-night). It contains computed value objects for sleep efficiency, sleep latency, REM latency, WASO, total sleep time, number of awakenings, and arousal index. It references the source ScoringSession by identity. Invariants include: sleep efficiency must be between 0 and 100 percent; total sleep time cannot exceed time in bed.

### SubjectiveAssessment Aggregate

The SubjectiveAssessment aggregate captures patient-completed questionnaire results. It models PSQI responses (seven component scores yielding a global score of 0-21) and ESS responses (eight situation scores yielding a total of 0-24). Invariants include: component scores must fall within defined ranges; the global score must equal the sum of component scores.

## Circadian Rhythm Context Aggregates

### CircadianProfile Aggregate

The CircadianProfile aggregate models an individual's circadian characteristics over an assessment period. It contains chronotype classification, DLMO timing, light exposure log entries (timestamped lux measurements), estimated circadian phase angle, and social jet lag calculation. Invariants include: DLMO must fall within a physiologically plausible range (typically 19:00-03:00); chronotype classification must correspond to the validated assessment instrument used.

## Intervention Tracking Context Aggregates

### InterventionPlan Aggregate

The InterventionPlan aggregate represents the comprehensive treatment plan for a patient's sleep disorder. It contains one or more InterventionModule entities (CBT-I sessions, medication prescriptions, device therapy prescriptions, sleep hygiene protocols). Each module tracks its own status, schedule, and outcome measures. Invariants include: active medication interactions must be flagged; CPAP pressure settings must fall within device-supported ranges.

### AdherenceRecord Aggregate

The AdherenceRecord aggregate tracks therapy device usage for a defined period. For CPAP, it captures nightly usage hours, mask leak rates, residual AHI, and pressure delivery data. Invariants include: usage hours per night cannot exceed 24; adherence percentage calculation must use the standard compliance window definition.

## Clinical Integration Context Aggregates

### ClinicalReport Aggregate

The ClinicalReport aggregate assembles a complete sleep study report for clinical consumption. It references source data from other contexts by identity and contains narrative interpretation sections, tabular metric summaries, the hypnogram, and clinical impressions. Invariants include: all referenced source data must exist; the report must include all mandatory sections per clinical reporting standards.

## Cross-Aggregate References

Aggregates reference each other by identity (PatientId, StudyId, ScoringSessionId, AssessmentId) rather than by direct object reference. This ensures loose coupling between aggregates and enables eventual consistency through domain events.

## References

- Evans, E. (2003). *Domain-Driven Design*. Addison-Wesley. Chapter 6: The Life Cycle of a Domain Object.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley. Chapter 10: Aggregates.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media. Chapter 10: Aggregate Design.
