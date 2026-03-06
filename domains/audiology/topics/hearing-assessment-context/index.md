# Hearing Assessment Context

## Overview

The Hearing Assessment Context is the core bounded context of the audiology domain. It encompasses all diagnostic evaluation activities that determine a patient's hearing status, including pure tone audiometry, tympanometry, otoacoustic emissions (OAE) testing, auditory brainstem response (ABR), and speech recognition testing. This context produces the foundational clinical data upon which all other bounded contexts depend.

## Strategic Importance

As the core subdomain, the Hearing Assessment Context receives the highest investment in domain modeling and the deepest collaboration with clinical audiologists. Diagnostic accuracy directly determines the quality of device fitting, rehabilitation planning, and clinical outcomes. The domain model must faithfully represent the complexity of audiometric testing, including masking rules, cross-hearing physics, threshold search procedures, and calibration requirements.

## Ubiquitous Language

Key terms within this bounded context include: audiometry, pure tone threshold, air conduction, bone conduction, masking, interaural attenuation, speech recognition threshold (SRT), word recognition score (WRS), pure tone average (PTA), tympanometry, acoustic reflex, otoacoustic emissions, auditory brainstem response, audiogram, hearing loss degree, hearing loss type, and hearing loss configuration.

## Aggregates

### Audiometric Evaluation

The central aggregate is the Audiometric Evaluation, rooted by the AudiometricEvaluation entity. This aggregate contains all measurements from a single diagnostic session: pure tone thresholds (HearingThreshold value objects), test configuration parameters (TestConfiguration value object), and the resulting audiogram (Audiogram value object). The aggregate enforces that thresholds are measured at valid frequencies and intensities, masking is applied when clinically required, and the audiogram accurately reflects the measured data.

### Speech Recognition Evaluation

The Speech Recognition Evaluation aggregate captures speech testing results. It contains speech recognition thresholds, word recognition scores at specified presentation levels, and the word lists used. The aggregate validates that presentation levels are appropriate relative to the patient's pure tone thresholds.

### Immittance Evaluation

The Immittance Evaluation aggregate captures tympanometry and acoustic reflex results. It contains tympanogram value objects (peak pressure, compliance, ear canal volume) and acoustic reflex threshold value objects. The aggregate classifies tympanometric patterns (Type A, B, C, As, Ad) according to standard criteria.

## Value Objects

HearingThreshold captures a single threshold measurement (frequency, intensity, ear, conduction type, response indicator). Audiogram represents the complete set of thresholds. PureToneAverage calculates the three-frequency average. HearingLossClassification encapsulates degree, type, and configuration. MaskingParameters specifies masking conditions. Tympanogram records middle ear compliance measurements. AcousticReflexThreshold records reflex levels.

## Domain Events

EvaluationCompleted is the primary event, published when the audiologist finalizes a diagnostic evaluation. This event triggers device candidacy assessment, rehabilitation needs evaluation, and clinical report generation. HearingLossDetected is published when clinically significant hearing loss is identified. ThresholdChangeDetected is published when comparison to previous evaluations reveals significant change.

## Domain Services

The HearingLossClassificationService determines degree, type, and configuration from audiometric data. The MaskingCalculationService determines masking requirements and levels. The ThresholdComparisonService identifies significant changes between serial evaluations. The CalibrationVerificationService validates that audiometric equipment meets ANSI S3.6 standards.

## Business Rules

Several critical business rules are enforced within this context. Masking must be applied whenever the air conduction threshold of the test ear exceeds the bone conduction threshold of the non-test ear by more than the interaural attenuation value for the transducer in use. Bone conduction thresholds cannot be better than air conduction thresholds for the same ear and frequency (the air-bone gap must be zero or positive). Speech recognition thresholds must agree with pure tone averages within an acceptable range (typically plus or minus 6 dB). Equipment calibration must be current per ANSI S3.6 before test results are accepted.

## Workflow

The typical workflow begins with case history collection, followed by otoscopy (outside the domain model), then tympanometry, pure tone audiometry (air conduction, bone conduction with masking as needed), speech recognition testing, and any additional tests indicated by initial findings (OAE, ABR). The evaluation aggregate accumulates results as each test is performed. When the audiologist completes the evaluation, the aggregate is finalized and the EvaluationCompleted event is published.

## External Interfaces

This context interfaces with audiometer systems through an anti-corruption layer that normalizes manufacturer-specific data formats. It publishes evaluation results to downstream contexts through a published language that standardizes how audiometric data is represented for consumption by Device Management, Rehabilitation, and Clinical Documentation.

## Quality Assurance

The Hearing Assessment Context includes built-in quality checks. Test-retest reliability is tracked by comparing threshold measurements within a session. Inter-session reliability is monitored through the ThresholdComparisonService. Equipment calibration status is validated before test results are accepted into the domain model.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media.
- Katz, J. (Ed.) (2015). Handbook of Clinical Audiology. 7th Edition. Wolters Kluwer.
- American National Standards Institute (ANSI). S3.6 - Specification for Audiometers.
- American Speech-Language-Hearing Association (ASHA). Guidelines for Manual Pure-Tone Threshold Audiometry.
