# Ubiquitous Language - Sleep Health Metrics

## Overview

Ubiquitous language is the shared vocabulary that domain experts, developers, and stakeholders use consistently when discussing the sleep health metrics domain. Every term carries a precise meaning agreed upon by all parties, eliminating ambiguity that arises when technical and clinical teams use different words for the same concept or the same word for different concepts. The ubiquitous language for this domain is grounded in sleep medicine, polysomnography, chronobiology, and clinical informatics.

## Purpose

In sleep medicine, terminology is already highly standardized through the AASM scoring manual and the ICSD-3 diagnostic taxonomy. The ubiquitous language for this domain leverages these existing standards rather than inventing new terms. When a sleep technologist says "N3 epoch" and a developer says "deep sleep segment," the ubiquitous language resolves this to the AASM-defined term: an epoch scored as stage N3 (slow wave sleep) based on the presence of high-amplitude delta activity exceeding 75 microvolts in at least 20 percent of the epoch.

## Language Boundaries

The ubiquitous language varies across bounded contexts. Within the Sleep Data Collection Context, "channel" refers to a single physiological signal stream (e.g., C3-M2 EEG derivation). Within the Clinical Integration Context, "channel" might refer to a communication pathway with an EHR system. The ubiquitous language respects these boundaries: each bounded context maintains its own dialect while sharing a core set of terms that have universal meaning.

## Core Terms

The following terms form the universal core of the ubiquitous language:

- **Sleep Study**: A formal recording session in which a patient's sleep is monitored using one or more measurement modalities.
- **Polysomnography (PSG)**: Multi-channel physiological recording during sleep, including EEG, EOG, EMG, ECG, respiratory, and oximetry channels.
- **Sleep Epoch**: A 30-second segment of recorded data serving as the atomic unit for sleep stage scoring.
- **Sleep Stage**: The classification assigned to an epoch: Wake (W), N1, N2, N3, or REM (R).
- **Sleep Architecture**: The overall structure and pattern of sleep stages across a recording period.
- **Hypnogram**: A time-series plot showing sleep stage progression through the night.
- **Sleep Latency**: Time from lights-out to the first scored sleep epoch.
- **Sleep Efficiency**: Ratio of total sleep time to time in bed, expressed as a percentage.
- **WASO (Wake After Sleep Onset)**: Total wake time between initial sleep onset and final awakening.
- **Total Sleep Time (TST)**: Sum of all epochs scored as any sleep stage.
- **Arousal**: A transient shift in EEG frequency lasting 3 seconds or more, indicating sleep fragmentation.

## Assessment Terms

- **PSQI (Pittsburgh Sleep Quality Index)**: A 19-item self-report instrument yielding a global score from 0 to 21 where scores above 5 indicate poor sleep quality.
- **ESS (Epworth Sleepiness Scale)**: An 8-item questionnaire measuring daytime sleepiness propensity, scored from 0 to 24.
- **AHI (Apnea-Hypopnea Index)**: The number of apnea and hypopnea events per hour of sleep, classifying obstructive sleep apnea severity.

## Circadian Terms

- **Chronotype**: An individual's innate preference for timing of sleep and wakefulness.
- **DLMO (Dim Light Melatonin Onset)**: The time at which salivary or plasma melatonin rises above threshold under dim light, marking circadian phase.
- **Social Jet Lag**: The discrepancy between an individual's biological clock and socially imposed sleep schedule.

## Intervention Terms

- **CBT-I (Cognitive Behavioral Therapy for Insomnia)**: A multi-component behavioral treatment program for chronic insomnia.
- **CPAP Adherence**: Patient compliance with continuous positive airway pressure therapy, measured in hours per night and percentage of nights used.
- **Sleep Hygiene**: Behavioral and environmental practices that promote consistent, restorative sleep.

## Clinical Integration Terms

- **FHIR Resource**: A standardized data structure defined by HL7 FHIR for representing clinical information.
- **Sleep Study Report**: A clinical document summarizing findings from a polysomnographic study, including staging, respiratory events, and clinical impressions.

## Language Governance

The ubiquitous language is governed collaboratively. Sleep medicine specialists validate clinical terminology. Developers ensure that code artifacts (class names, method names, variable names) mirror the ubiquitous language exactly. When a new term is proposed or an existing term is refined, all bounded contexts are reviewed for consistency. This governance prevents linguistic drift that would undermine the shared understanding.

## Language Evolution

As sleep medicine advances -- new scoring rules, new diagnostic categories, new device types -- the ubiquitous language evolves. Each update follows a formal process: the term is proposed, defined with clinical precision, validated by domain experts, and propagated across all bounded context documentation.

## References

- Evans, E. (2003). *Domain-Driven Design*. Addison-Wesley. Chapter 2: Communication and the Use of Language.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley. Chapter 2: Domains, Subdomains, and Bounded Contexts.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media. Chapter 2: Discovering Domain Knowledge.
- Berry, R. B. et al. (2020). *AASM Manual for the Scoring of Sleep and Associated Events*. Version 2.6.
- American Academy of Sleep Medicine. (2014). *ICSD-3*.
