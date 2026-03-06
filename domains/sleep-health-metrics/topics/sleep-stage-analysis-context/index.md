# Sleep Stage Analysis Context - Sleep Health Metrics

## Overview

The Sleep Stage Analysis Context is responsible for classifying sleep recordings into standardized sleep stages, computing sleep architecture, detecting arousals and respiratory events, and generating hypnograms. This context applies the AASM scoring rules to 30-second epochs of polysomnographic data, producing the scored data that downstream contexts use for quality assessment, clinical reporting, and intervention evaluation.

## Scope and Boundaries

This context receives validated epoch data from the Sleep Data Collection Context and produces scored sleep stages, arousal indices, respiratory event summaries, and sleep architecture computations. It does not own raw signal data (that belongs upstream) and does not compute quality metrics or composite indices (that belongs downstream in the Sleep Quality Assessment Context).

## Sleep Stage Classification

The context classifies each 30-second epoch into one of five stages defined by the AASM scoring manual:

- **Wake (W)**: Characterized by alpha rhythm (8-13 Hz) over occipital regions with eyes closed, or low-amplitude mixed-frequency activity with eye blinks and reading eye movements when eyes are open.
- **N1 (NREM Stage 1)**: Transition from wakefulness with attenuation of alpha rhythm and appearance of low-amplitude mixed-frequency activity (4-7 Hz theta range), slow eye movements, and vertex sharp waves.
- **N2 (NREM Stage 2)**: Defined by the presence of K-complexes (high-amplitude biphasic waves) and/or sleep spindles (11-16 Hz bursts lasting 0.5-2 seconds) against a background of relatively low-amplitude mixed-frequency activity.
- **N3 (NREM Stage 3 / Slow Wave Sleep)**: Characterized by high-amplitude (greater than 75 microvolts) slow-wave activity in the delta frequency range (0.5-2 Hz) present in 20 percent or more of the epoch.
- **REM (R)**: Characterized by low-amplitude mixed-frequency EEG activity, rapid eye movements, and low chin EMG tone (skeletal muscle atonia).

## Sleep Architecture Computation

Sleep architecture describes the structural organization of sleep across the recording period. The context computes:

- Total duration and percentage of each sleep stage.
- Number of NREM-REM sleep cycles (typically 4-6 per night in healthy adults, each lasting approximately 90-120 minutes).
- Cycle-by-cycle analysis showing how stage composition changes across the night (early cycles are N3-dominant; later cycles are REM-dominant).
- Stage transition counts and transition probability matrices.
- Sleep period time (from first sleep onset to final awakening).

## Arousal Detection

The context detects cortical arousals defined by the AASM as abrupt shifts in EEG frequency (including alpha, theta, or frequencies greater than 16 Hz but not sleep spindles) lasting at least 3 seconds, with at least 10 seconds of stable sleep preceding the event. During REM sleep, arousals additionally require a concurrent increase in chin EMG amplitude.

Arousals are classified by probable cause: spontaneous, respiratory event-related, periodic limb movement-related, or technician-induced. The arousal index (total arousals per hour of sleep) is a key marker of sleep fragmentation.

## Respiratory Event Scoring

The context identifies and classifies respiratory events:

- **Obstructive apnea**: Cessation or near-cessation of airflow for at least 10 seconds with continued respiratory effort.
- **Central apnea**: Cessation of airflow for at least 10 seconds without respiratory effort.
- **Mixed apnea**: An event beginning as central and transitioning to obstructive.
- **Hypopnea**: Reduction in airflow of at least 30 percent for at least 10 seconds, associated with either a 3 percent or greater oxygen desaturation or an arousal (AASM recommended rule).
- **RERA (Respiratory Effort-Related Arousal)**: A sequence of breaths with increasing respiratory effort leading to an arousal, not meeting apnea or hypopnea criteria.

## Key Aggregates

The **ScoringSession** aggregate contains all scored epochs, computes sleep architecture, and maintains the hypnogram. The **RespiratoryEventSummary** aggregate captures all respiratory events and computes the AHI, RDI, and oxygen desaturation index.

## Domain Events Published

- **ScoringSessionCompleted**: Signals that epoch-by-epoch scoring is finalized.
- **RespiratoryEventsScored**: Signals that respiratory event classification and AHI computation are complete.
- **ArousalIndexComputed**: Signals that arousal detection and indexing are finalized.

## Scoring Modalities

The context supports manual scoring by registered polysomnographic technologists (RPSGT), automated scoring by validated algorithms, and hybrid approaches where automated scoring is reviewed and corrected by human scorers. Inter-scorer reliability metrics (Cohen's kappa, epoch-by-epoch agreement percentage) can be computed when multiple scoring sessions exist for the same study.

## References

- Evans, E. (2003). *Domain-Driven Design*. Addison-Wesley.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media.
- Berry, R. B. et al. (2020). *AASM Manual for the Scoring of Sleep and Associated Events*. Version 2.6.
- Rechtschaffen, A., Kales, A. (1968). *A Manual of Standardized Terminology, Techniques and Scoring System for Sleep Stages of Human Subjects*. UCLA Brain Information Service.
