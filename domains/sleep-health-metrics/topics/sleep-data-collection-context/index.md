# Sleep Data Collection Context - Sleep Health Metrics

## Overview

The Sleep Data Collection Context is responsible for the acquisition, validation, normalization, and storage of raw sleep data from multiple measurement modalities. It serves as the upstream data source for all analytical bounded contexts in the sleep health metrics domain. This context owns the canonical representation of recorded physiological signals and patient-reported sleep information.

## Scope and Boundaries

This context encompasses four primary data sources:

- **Polysomnography (PSG)**: Multi-channel recordings including EEG (electroencephalography), EOG (electrooculography), EMG (electromyography), ECG (electrocardiography), respiratory effort (thoracic and abdominal bands), nasal airflow (pressure transducer and thermistor), pulse oximetry (SpO2), body position, and snoring microphone. A standard diagnostic PSG includes 16-25 simultaneous channels recorded at sampling rates from 10 Hz (SpO2) to 500 Hz (EEG).

- **Actigraphy**: Wrist-worn accelerometer data recording rest-activity patterns continuously over days to weeks. The actigraph produces activity counts per epoch (typically 30 or 60 seconds), from which sleep-wake states are algorithmically estimated.

- **Wearable Sensors**: Consumer and clinical-grade wearable devices (wristbands, rings, headbands, under-mattress sensors) that collect simplified sleep metrics including movement, heart rate, heart rate variability, skin temperature, and blood oxygen estimates.

- **Sleep Diary**: Patient-completed daily logs recording subjective sleep parameters: bedtime, lights-out time, estimated sleep onset, nighttime awakenings, final awakening, out-of-bed time, naps, caffeine and alcohol intake, exercise, and subjective quality rating.

## Key Aggregates

The **SleepStudy** aggregate is the primary aggregate root. It encapsulates all recorded channel data for a single study session, enforcing invariants about channel completeness, signal quality thresholds, and recording duration minimums.

The **SleepDiaryEntry** aggregate captures a single patient diary submission with temporal validity rules.

## Entities and Value Objects

Key entities include RecordingChannel (individual signal streams with type, derivation, and quality metadata) and SensorDevice (the physical instrument producing the data).

Key value objects include SignalQualityAssessment (impedance levels, artifact percentage, signal-to-noise ratio), ChannelDerivation (e.g., C3-M2, O1-M2 per the 10-20 system), and RecordingMetadata (study type, technologist, environmental conditions).

## Domain Events Published

- **SleepStudyRecorded**: Emitted when a PSG recording is completed and validated, signaling the Sleep Stage Analysis Context that data is ready for scoring.
- **SleepDiarySubmitted**: Emitted when a patient diary entry is received.
- **WearableDataSynced**: Emitted when wearable device data synchronization is complete.

## Anti-Corruption Layer

This context maintains an anti-corruption layer at its device integration boundary. Each device manufacturer provides data in proprietary formats (EDF, EDF+, MFERI, vendor-specific APIs). The ACL translates these formats into the context's canonical channel model with AASM-standard derivation names, normalized sampling rates, and standardized units. This insulates the domain from vendor lock-in and firmware update impacts.

## Validation Rules

The context enforces validation rules derived from clinical standards:

- A diagnostic PSG must include mandatory AASM montage channels (minimum: frontal, central, and occipital EEG; bilateral EOG; chin EMG; ECG; respiratory effort; nasal airflow; SpO2).
- Recording duration must meet a minimum threshold (typically 6 hours for a diagnostic study, 2 hours for a split-night study).
- Signal impedance must fall below acceptable thresholds (typically below 5 kOhms for EEG channels).
- Artifact-contaminated segments must be flagged but not discarded, preserving data completeness.

## Integration Points

Upstream: Device manufacturers, wearable APIs, patient-facing diary applications.
Downstream: Sleep Stage Analysis Context (primary consumer), Sleep Quality Assessment Context (diary data).

## References

- Evans, E. (2003). *Domain-Driven Design*. Addison-Wesley.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media.
- Berry, R. B. et al. (2020). *AASM Manual for the Scoring of Sleep and Associated Events*. Version 2.6.
- Kemp, B., Olivan, J. (2003). European data format 'plus' (EDF+). *Clinical Neurophysiology*, 114(9), 1755-1761.
- Ancoli-Israel, S. et al. (2015). The SBSM Guide to Actigraphy Monitoring. *Behavioral Sleep Medicine*, 13(sup1), S4-S38.
