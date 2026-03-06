# Bounded Contexts - Sleep Health Metrics

## Overview

Bounded contexts define explicit boundaries within which a particular domain model is consistent and unambiguous. In the sleep health metrics domain, six bounded contexts partition the problem space along natural clinical and analytical seams. Each context maintains its own ubiquitous language, its own model of sleep-related concepts, and its own invariants. Concepts that share the same name across contexts -- such as "sleep episode" or "patient" -- may carry different semantics and attributes within each boundary.

## Sleep Data Collection Context

This context owns the acquisition, validation, and storage of raw sleep data from multiple sources. It models polysomnography (PSG) recording sessions with their constituent EEG, EOG, EMG, ECG, and respiratory channels. It handles actigraphy data streams from wrist-worn accelerometers, continuous data feeds from consumer and clinical-grade wearable sensors, and structured sleep diary entries completed by patients.

Key responsibilities include signal quality validation, channel synchronization, artifact flagging, and data completeness verification. The context defines what constitutes a valid recording and enforces rules about minimum recording duration, required channels, and acceptable signal-to-noise ratios.

The boundary is drawn at raw, unscored data. Once signals leave this context for analysis, they are transmitted as validated epoch segments. This context does not perform sleep staging or metric calculation.

## Sleep Stage Analysis Context

This context owns the classification of sleep data into standardized stages following the AASM scoring manual. It operates on 30-second epochs, applying rules for Wake (W), N1, N2, N3 (slow wave sleep), and REM (R) stage classification. The context models sleep architecture -- the temporal structure and distribution of sleep stages across the recording period.

Key responsibilities include epoch-by-epoch scoring, arousal detection and indexing, respiratory event identification (apneas, hypopneas), periodic limb movement detection, and hypnogram generation. The context supports both manual scoring by registered polysomnographic technologists and automated algorithmic scoring.

The boundary separates scoring from raw data management (upstream) and from quality metric computation (downstream). Scored epoch data and sleep architecture summaries are published for consumption by the Sleep Quality Assessment Context.

## Sleep Quality Assessment Context

This context owns the computation of standardized sleep quality metrics and composite indices. It calculates sleep efficiency (total sleep time divided by time in bed), sleep onset latency, REM latency, wake after sleep onset (WASO), total sleep time, and number of awakenings. It also manages subjective assessment instruments including the Pittsburgh Sleep Quality Index (PSQI) and the Epworth Sleepiness Scale (ESS).

Key responsibilities include metric computation from scored data, normative comparison against age-adjusted reference ranges, longitudinal trend tracking, and composite index scoring with clinical interpretation thresholds.

The boundary ensures that this context does not perform sleep staging directly but consumes staged data. It publishes quality metrics for use by the Intervention Tracking and Clinical Integration Contexts.

## Circadian Rhythm Context

This context owns the modeling of circadian biology as it relates to sleep timing and quality. It tracks chronotype classifications (morning, intermediate, evening types), light exposure patterns (lux intensity and spectral composition over time), dim light melatonin onset (DLMO) measurements, core body temperature rhythms, and shift work schedule impacts on circadian alignment.

Key responsibilities include circadian phase estimation, social jet lag calculation (the discrepancy between biological and social clocks), phase advance and delay quantification, and light exposure dosimetry. The context models how circadian misalignment affects sleep quality and informs optimal intervention timing.

The boundary separates circadian biology from sleep staging and quality metrics. The Circadian Rhythm Context operates as a partner context, enriching quality assessments with phase alignment data without owning the quality metrics themselves.

## Intervention Tracking Context

This context owns the management of therapeutic interventions for sleep disorders. It models CBT-I (Cognitive Behavioral Therapy for Insomnia) treatment protocols with their component modules (stimulus control, sleep restriction, cognitive restructuring, relaxation training, sleep hygiene education). It tracks medication prescriptions (hypnotics, melatonin agonists, orexin receptor antagonists) with dosing schedules and side effect monitoring.

Key responsibilities include CPAP therapy adherence tracking (usage hours, mask leak, residual AHI), device settings management, intervention outcome assessment, and treatment plan adjustment based on quality metric feedback. The context supports multi-modal treatment plans combining pharmacological, behavioral, and device-based approaches.

The boundary ensures that intervention logic does not leak into data collection, staging, or quality assessment. This context consumes quality metrics and circadian data to evaluate treatment efficacy but does not recalculate them.

## Clinical Integration Context

This context owns the exchange of sleep health data with external clinical systems. It maps internal domain concepts to FHIR R4 resources (Observation, DiagnosticReport, Condition, Procedure, Device), generates standardized sleep study reports, manages provider-to-provider referral workflows, and handles insurance authorization documentation.

Key responsibilities include EHR data import and export, FHIR resource serialization and deserialization, clinical document generation following sleep study reporting standards, and patient demographic synchronization. The context translates between the rich internal domain model and the constrained external clinical data models.

The boundary contains all external system interaction complexity. Other bounded contexts never directly communicate with EHR systems, FHIR servers, or insurance platforms.

## Context Boundary Enforcement

Each bounded context enforces its boundary through explicit interfaces. Data crossing boundaries is translated through published contracts or anti-corruption layers. No bounded context directly accesses another context's internal model. This ensures that changes within one context -- such as adopting a new AASM scoring version in the Sleep Stage Analysis Context -- do not cascade unpredictably into other contexts.

## References

- Evans, E. (2003). *Domain-Driven Design*. Addison-Wesley. Chapter 14: Maintaining Model Integrity.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley. Chapter 2: Domains, Subdomains, and Bounded Contexts.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media. Chapter 6: Bounded Contexts.
- Berry, R. B. et al. (2020). *AASM Manual for the Scoring of Sleep and Associated Events*. Version 2.6.
