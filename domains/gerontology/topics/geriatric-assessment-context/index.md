# Geriatric Assessment Context

## Overview

The Geriatric Assessment Context is the foundational bounded context in the gerontology domain. It encompasses the comprehensive geriatric assessment (CGA), a multidimensional, interdisciplinary diagnostic process that determines the medical, psychological, functional, and social capabilities and limitations of an older adult. The CGA has been demonstrated to improve outcomes for older adults, including reduced mortality, decreased institutionalization, and improved functional status (Rubenstein et al., 1991).

This context serves as the primary upstream context in the domain's context map, producing assessment findings that drive decisions in all other bounded contexts. It establishes the clinical baseline from which care plans, cognitive evaluations, functional interventions, medication reviews, and end-of-life planning proceed.

## Ubiquitous Language

Key terms within this context include comprehensive geriatric assessment, frailty phenotype, frailty index, Clinical Frailty Scale, fall risk assessment, Morse Fall Scale, Timed Up and Go test, nutritional screening, Mini Nutritional Assessment, geriatric syndrome, sarcopenia, orthostatic hypotension, gait assessment, balance evaluation, grip strength, sensory assessment, and multimorbidity. These terms carry precise meanings within this context that may differ from their usage in other clinical settings.

## Aggregates

### GeriatricAssessment

The primary aggregate root. Contains FrailtyScreening, FallRiskEvaluation, NutritionalAssessment, and SensoryAssessment as child entities. Enforces the invariant that all required assessment dimensions must be completed before the assessment can be finalized. Publishes the AssessmentCompleted domain event upon finalization.

### FrailtyScreening

Models the evaluation of frailty using standardized instruments. Contains GripStrengthMeasurement, GaitSpeedMeasurement, WeightLoss, Exhaustion, and PhysicalActivityLevel value objects when using the Fried Phenotype. The aggregate enforces instrument-specific scoring rules and produces a FrailtyClassification value object.

## Entities

The GeriatricAssessment entity tracks a specific assessment event with a unique identifier, associated patient, assessing clinician, assessment date, and status. The FallRiskEvaluation entity captures a structured fall risk assessment with the instrument used, score, and risk classification. The NutritionalAssessment entity records nutritional screening results including BMI, weight change, and appetite evaluation.

## Value Objects

FrailtyScore captures the instrument name, numeric score, and classification. FallRiskScore captures the instrument, score, and risk level. BloodPressureReading captures systolic and diastolic values for orthostatic hypotension assessment. GripStrengthMeasurement captures the hand tested and force in kilograms. GaitSpeedMeasurement captures the distance, time, and calculated speed.

## Domain Events

AssessmentCompleted is published when a CGA is finalized, containing summary findings for all downstream contexts. FrailtyStatusChanged is published when a patient's frailty classification differs from the previous assessment. FallRiskIdentified is published when moderate or high fall risk is detected. NutritionalDeficiencyDetected is published when malnutrition risk is identified.

## Domain Services

ComprehensiveAssessmentScoringService synthesizes scores across multiple assessment dimensions into an overall risk profile. FrailtyTrajectoryAnalysisService analyzes longitudinal frailty data to determine trajectory. These services span multiple aggregates and apply clinical algorithms.

## Clinical Workflow

The geriatric assessment workflow begins with a referral or scheduled screening. The assessing clinician initiates a GeriatricAssessment aggregate, then conducts evaluations across required dimensions. Each evaluation produces scores and classifications captured as value objects within the assessment. When all dimensions are complete, the clinician finalizes the assessment, triggering the AssessmentCompleted event. Downstream contexts consume this event to initiate their own processes.

Assessment frequency varies by clinical need and care setting. Community-dwelling older adults may be assessed annually, while residents of skilled nursing facilities may be assessed quarterly or upon significant clinical change.

## Integration Points

This context integrates with hospital EHR systems through an anti-corruption layer that translates between the EHR's encounter-based model and the domain's assessment-based model. It integrates with external laboratory systems to receive lab results that inform nutritional and metabolic assessment. Assessment results are published as domain events consumed by all other bounded contexts.

## Regulatory Considerations

The CGA is mandated by CMS for certain settings, including the Minimum Data Set (MDS) for nursing homes and the Outcome and Assessment Information Set (OASIS) for home health agencies. The assessment must comply with documentation requirements for Medicare reimbursement and quality reporting.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media.
- Rubenstein, L.Z., Stuck, A.E., Siu, A.L., & Wieland, D. (1991). Impacts of Geriatric Evaluation and Management Programs on Defined Outcomes. JAGS.
- Fried, L.P. et al. (2001). Frailty in Older Adults: Evidence for a Phenotype. The Journals of Gerontology.
