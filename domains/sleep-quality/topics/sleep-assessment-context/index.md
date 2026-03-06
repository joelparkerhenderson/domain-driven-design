# Sleep Assessment Context

## Overview

The Sleep Assessment Context is responsible for evaluating sleep quality through standardized clinical instruments and measurement technologies. It manages the administration, scoring, and interpretation of sleep diaries, validated questionnaires (PSQI, ESS, ISI), actigraphy data, and polysomnography studies. This context serves as the primary entry point for clinical data into the sleep quality domain, producing structured assessment results that inform diagnosis, treatment planning, and outcomes tracking.

## Core Responsibilities

This context owns the domain logic for sleep quality measurement. It manages the lifecycle of assessments from scheduling through completion and review. It enforces correct scoring algorithms for validated instruments. It normalizes data from different measurement modalities (self-report, actigraphy, polysomnography) into comparable formats. It publishes assessment results as domain events for consumption by downstream contexts.

## Key Aggregates

### Sleep Assessment

The primary aggregate groups a collection of measurement instruments for a single evaluation period. It maintains the assessment status lifecycle (scheduled, in-progress, completed, reviewed) and enforces invariants such as instrument scoring consistency and evaluation period alignment.

### Sleep Diary

The longitudinal self-report aggregate maintains daily entries capturing bedtime, lights-out time, estimated sleep onset latency, nighttime awakenings, WASO, final wake time, rise time, and subjective quality rating. It computes derived metrics including total sleep time, time in bed, and sleep efficiency for each entry and for configurable rolling windows.

## Assessment Instruments

### Pittsburgh Sleep Quality Index (PSQI)

The PSQI is a 19-item self-report questionnaire developed by Buysse et al. (1989) that measures sleep quality over a one-month retrospective period. The context models the 19 items, the seven component scoring algorithm (subjective sleep quality, sleep latency, sleep duration, habitual sleep efficiency, sleep disturbances, use of sleeping medication, daytime dysfunction), and the global score computation (sum of components, range 0-21). A global score greater than 5 distinguishes poor from good sleepers with 89.6 percent sensitivity and 86.5 percent specificity.

### Epworth Sleepiness Scale (ESS)

The ESS is an 8-item questionnaire developed by Johns (1991) that measures the general level of daytime sleepiness. Each item rates the chance of dozing in a specific situation on a scale of 0-3. The context models the eight situations, the scoring algorithm (sum of all items, range 0-24), and the classification scheme (normal 0-10, mild excessive sleepiness 11-14, moderate 15-17, severe 18-24).

### Insomnia Severity Index (ISI)

The ISI is a 7-item self-report instrument that measures insomnia severity over the past two weeks. The context models the seven items, the total score computation (range 0-28), and the severity classification (no clinically significant insomnia 0-7, subthreshold 8-14, moderate 15-21, severe 22-28).

## Measurement Technologies

### Actigraphy

Actigraphy uses wrist-worn accelerometers to estimate sleep-wake patterns over extended periods (typically 1-2 weeks). The context receives normalized actigraphy data from the Device Integration Context and computes standard sleep metrics: total sleep time, sleep onset latency, WASO, sleep efficiency, and fragmentation index. Actigraphy data serves as an objective complement to self-report diary data.

### Polysomnography

Polysomnography (PSG) is the gold standard clinical sleep study. The context models PSG study coordination (referral, scheduling, completion) and receives scored PSG results including sleep staging (N1, N2, N3, REM percentages), sleep architecture metrics, respiratory event indices (AHI, RDI), periodic limb movement index, and arousal index. PSG data provides the most detailed assessment but is limited to single-night or few-night studies.

## Domain Events Published

The context publishes AssessmentCompleted when a full assessment is scored and reviewed, SleepDiaryEntryRecorded when a new diary entry is validated, and ActigraphyDataProcessed when a period of actigraphy data is analyzed. These events carry sufficient detail for downstream contexts to incorporate assessment results without querying back.

## Relationships with Other Contexts

The Sleep Assessment Context acts as a supplier to the Sleep Disorders Context (providing assessment data for diagnostic evaluation), to the Outcomes Tracking Context (providing baseline and periodic measurement data), and to the Behavioral Interventions Context (providing sleep diary data for treatment parameter calibration). It consumes normalized device data from the Device Integration Context in a conformist relationship.

## Clinical Workflow Integration

The context supports multiple clinical workflows: initial intake assessment (comprehensive battery of instruments), follow-up assessment (periodic readministration during treatment), and continuous monitoring (ongoing diary and actigraphy). Each workflow type determines which instruments are administered and how results are aggregated.

## Data Quality

The context enforces data quality rules for each measurement type. Questionnaire responses must be complete (no missing items for validated instruments). Diary entries must have logically consistent times (lights-out after bedtime, wake time after lights-out). Actigraphy data must meet minimum recording duration thresholds (typically 5 or more valid nights). The context flags quality issues but does not reject data unilaterally, as clinical judgment may override quality thresholds.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley.
- Buysse, D.J. et al. (1989). The Pittsburgh Sleep Quality Index. Psychiatry Research, 28(2), 193-213.
- Johns, M.W. (1991). A New Method for Measuring Daytime Sleepiness: The Epworth Sleepiness Scale. Sleep, 14(6), 540-545.
- Bastien, C.H. et al. (2001). Validation of the Insomnia Severity Index. Sleep Medicine, 2(4), 297-307.
