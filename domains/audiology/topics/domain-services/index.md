# Domain Services in the Audiology Domain

## Overview

Domain services encapsulate business logic that does not naturally belong to any single entity or value object. A domain service is a stateless operation that operates across multiple aggregates or requires coordination that transcends aggregate boundaries. In the audiology domain, domain services handle clinical calculations, cross-aggregate validations, and complex business rules that span multiple domain objects.

## Domain Service Characteristics

Domain services in the audiology domain are stateless: they do not maintain internal state between invocations. They are defined in terms of the ubiquitous language, using domain concepts rather than technical terminology. They operate on domain objects (entities, value objects, aggregates) and return domain objects. They encapsulate domain logic that would be awkward or inappropriate to place within a single entity or value object.

## Hearing Assessment Context Services

### HearingLossClassificationService

The HearingLossClassificationService takes an Audiogram value object and produces a HearingLossClassification value object. This service encapsulates the clinical rules for determining degree of hearing loss (based on pure tone average according to Clark, 1981, or WHO classifications), type of hearing loss (comparing air and bone conduction thresholds), and configuration (analyzing the shape of the audiometric contour). This logic spans multiple threshold measurements and requires knowledge of classification standards, making it unsuitable for a single value object.

### MaskingCalculationService

The MaskingCalculationService determines whether masking is required for a given test condition and calculates the appropriate masking level. This service takes the test ear thresholds, non-test ear thresholds, interaural attenuation values for the transducer in use, and the current test frequency. It applies clinical masking rules (the plateau method or the Hood technique) to produce MaskingParameters value objects. The masking decision depends on data from both ears, making this a cross-aggregate concern within the evaluation.

### ThresholdComparisonService

The ThresholdComparisonService compares two audiograms from different evaluation dates and identifies clinically significant threshold changes. This service applies ASHA-defined criteria for significant threshold shifts (such as the standard threshold shift used in occupational hearing conservation) and produces a ThresholdChange value object summarizing the comparison. This service necessarily spans two evaluation aggregates.

## Device Management Context Services

### PrescriptiveTargetCalculationService

The PrescriptiveTargetCalculationService takes an Audiogram value object and a FittingFormula identifier (NAL-NL2, DSL v5, or others) and produces a FittingPrescription value object containing target gain values at each frequency. This service encapsulates the complex mathematical algorithms defined by prescriptive fitting formulae. The calculation depends on the patient's audiometric data, age, and device characteristics, crossing the boundary between patient and device data.

### FittingVerificationService

The FittingVerificationService compares RealEarMeasurement value objects against FittingPrescription value objects and produces a verification result indicating whether the device meets prescriptive targets within acceptable tolerances. This service applies clinical criteria for acceptable match-to-target (typically within 5 dB at each frequency) and flags deviations that require clinical attention.

### DeviceCandidacyService

The DeviceCandidacyService evaluates a patient's audiometric data, communication needs, and physical characteristics to determine candidacy for different device types (hearing aids, cochlear implants, bone-anchored devices). This service applies clinical candidacy criteria defined by FDA indications and professional guidelines, producing a CandidacyAssessment value object.

## Rehabilitation Context Services

### OutcomeAnalysisService

The OutcomeAnalysisService analyzes a series of OutcomeMeasurement value objects and determines whether a rehabilitation plan is achieving its goals. This service applies statistical criteria for clinically significant change (such as the critical difference for the APHAB or HHIE instruments) and produces a ProgressAssessment value object. The analysis spans multiple measurements over time.

### GoalSettingService

The GoalSettingService assists in establishing appropriate rehabilitation goals based on the patient's audiometric profile, device status, and communication needs. This service applies evidence-based guidelines for goal setting and produces candidate RehabilitationGoal value objects that clinicians can refine.

## Vestibular Services Context Services

### UnilateralWeaknessCalculationService

The UnilateralWeaknessCalculationService applies the Jongkees formula to caloric test responses and produces a UnilateralWeakness value object indicating the percentage difference between left and right vestibular function. This calculation spans multiple caloric response measurements and applies standardized interpretation criteria.

### FallRiskAssessmentService

The FallRiskAssessmentService combines vestibular evaluation findings, patient age, mobility status, and medical history to produce a FallRiskScore value object. This service applies validated fall risk assessment criteria and identifies patients who require safety counseling or referral to fall prevention programs.

## Pediatric Audiology Context Services

### DevelopmentalMonitoringService

The DevelopmentalMonitoringService compares a pediatric patient's current developmental milestones against age-appropriate expectations and identifies delays or deviations. This service applies developmental norms from speech-language pathology and audiology to produce milestone status assessments.

### ScreeningFollowUpService

The ScreeningFollowUpService tracks newborn hearing screening results and ensures timely diagnostic follow-up per EHDI guidelines (screening by one month, diagnosis by three months, intervention by six months). This service monitors compliance timelines and generates alerts for overdue follow-up.

## Clinical Documentation Context Services

### ReportGenerationService

The ReportGenerationService assembles clinical data from evaluation results, fitting records, and rehabilitation outcomes into a structured clinical report. This service applies documentation standards and regulatory requirements to produce report templates populated with clinical data.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley. Chapter 5 on domain services.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley. Chapter 7 on domain services.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media. Chapter 8 on service patterns.
