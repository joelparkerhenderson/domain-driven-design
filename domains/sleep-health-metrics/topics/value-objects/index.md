# Value Objects - Sleep Health Metrics

## Overview

Value objects are immutable domain objects defined entirely by their attributes rather than by a unique identity. Two value objects with identical attributes are considered equal and interchangeable. In the sleep health metrics domain, value objects represent measurements, classifications, scores, and computed metrics that describe aspects of sleep without requiring individual tracking over time.

## Value Object Design Principles

Value objects in this domain are immutable once created. They encapsulate validation logic to ensure they always represent valid domain concepts. They are freely shareable and comparable by structural equality. They often carry measurement units as part of their definition. Value objects are used extensively in sleep medicine because much of the domain involves classifying, measuring, and scoring.

## Sleep Stage and Scoring Value Objects

### SleepStage

The SleepStage value object represents a single AASM-defined sleep stage classification. Valid values are Wake (W), NREM Stage 1 (N1), NREM Stage 2 (N2), NREM Stage 3 (N3), and REM (R). It encapsulates the rules about what each stage means and what EEG patterns characterize it. Two SleepStage value objects representing N2 are identical and interchangeable.

### ScoredEpoch

The ScoredEpoch value object combines an epoch index (its position in the recording), a timestamp, the assigned SleepStage, and a confidence level (manual-definitive, automated-high, automated-low). It is immutable once scoring is finalized. The epoch duration is always 30 seconds per AASM standard and is not stored as an attribute.

### SleepArchitectureSummary

The SleepArchitectureSummary value object captures the structural distribution of sleep stages: percentage and duration of each stage, number of sleep cycles, average cycle duration, and total number of stage transitions. It is computed from a complete set of ScoredEpoch objects.

### HypnogramPoint

The HypnogramPoint value object represents a single data point in a hypnogram visualization: a timestamp paired with a SleepStage. A collection of HypnogramPoints constitutes the full hypnogram for a recording.

## Metric Value Objects

### SleepEfficiency

The SleepEfficiency value object represents the ratio of total sleep time to time in bed, constrained to a value between 0.0 and 1.0 (or 0 to 100 percent). It is created with validation ensuring the numerator does not exceed the denominator.

### SleepLatency

The SleepLatency value object represents the duration from lights-out to first sleep onset, expressed in minutes. It is constrained to non-negative values. A latency exceeding 30 minutes is clinically significant per ICSD-3 insomnia criteria.

### WASODuration

The WASODuration value object represents total wake time after initial sleep onset and before final awakening, expressed in minutes. It is constrained to non-negative values not exceeding the total recording time.

### TotalSleepTime

The TotalSleepTime value object represents the cumulative duration of all sleep epochs, expressed in minutes. It includes validation that the value does not exceed time in bed.

### ApneaHypopneaIndex

The ApneaHypopneaIndex value object represents the number of apnea and hypopnea events per hour of sleep. It includes a severity classification method: normal (AHI less than 5), mild (5-14.9), moderate (15-29.9), severe (30 or greater), following AASM clinical guidelines.

### ArousalIndex

The ArousalIndex value object represents the number of cortical arousals per hour of sleep. Normal values are typically below 10 for adults, with age-adjusted normative references.

## Assessment Score Value Objects

### PSQIScore

The PSQIScore value object captures the Pittsburgh Sleep Quality Index result. It contains seven component scores (subjective sleep quality, sleep latency, sleep duration, habitual sleep efficiency, sleep disturbances, use of sleeping medication, daytime dysfunction), each ranging from 0 to 3, and a global score (0-21). A global score greater than 5 indicates clinically poor sleep quality.

### EpworthScore

The EpworthScore value object captures the Epworth Sleepiness Scale result. It contains eight situation scores (0-3 each) and a total score (0-24). Scores of 10 or above suggest excessive daytime sleepiness warranting clinical evaluation.

### ChronotypeClassification

The ChronotypeClassification value object represents an individual's circadian preference: definitely morning type, moderately morning type, intermediate type, moderately evening type, or definitely evening type. It is derived from validated instruments such as the Morningness-Eveningness Questionnaire (MEQ).

## Circadian Value Objects

### CircadianPhaseMarker

The CircadianPhaseMarker value object captures a dim light melatonin onset (DLMO) measurement: the clock time at which melatonin concentration exceeds the threshold (typically 3 pg/mL in saliva or 10 pg/mL in plasma) under dim light conditions. It includes the measurement method and threshold used.

### SocialJetLag

The SocialJetLag value object quantifies the discrepancy between biological and social clocks, expressed as the absolute difference in hours between midpoint of sleep on work days and midpoint of sleep on free days.

### LightExposureDose

The LightExposureDose value object represents cumulative light exposure over a defined period, expressed in lux-hours, with optional spectral weighting for melanopic sensitivity.

## Intervention Value Objects

### CPAPSettings

The CPAPSettings value object captures prescribed CPAP parameters: therapy mode (fixed, auto-titrating, bilevel), pressure or pressure range (cm H2O), ramp time, expiratory pressure relief level, and humidification setting.

### AdherenceMetric

The AdherenceMetric value object represents CPAP compliance for a defined period: average usage hours per night, percentage of nights with usage exceeding 4 hours, total nights used, and overall compliance status (compliant or non-compliant per CMS criteria).

### MedicationDosage

The MedicationDosage value object captures a sleep medication prescription: drug name, dose amount, dose unit, administration route, timing relative to bedtime, and frequency.

## References

- Evans, E. (2003). *Domain-Driven Design*. Addison-Wesley. Chapter 5: A Model Expressed in Software -- Value Objects.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley. Chapter 6: Value Objects.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media. Chapter 9: Value Objects.
- Buysse, D. J. et al. (1989). The Pittsburgh Sleep Quality Index. *Psychiatry Research*, 28(2), 193-213.
