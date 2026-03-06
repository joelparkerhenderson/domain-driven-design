# Domain Services in the Rheumatology Domain

## Overview

Domain services are stateless operations that encapsulate domain logic which does not naturally belong to any single entity or value object. In the rheumatology domain, domain services handle operations that span multiple aggregates, implement complex clinical algorithms, coordinate cross-entity business rules, or perform calculations that require inputs from several domain objects. Domain services are defined using ubiquitous language and contain pure business logic without infrastructure dependencies.

## Clinical Assessment Context Services

### ClinicalScoringService

The ClinicalScoringService calculates composite disease activity indices from component measurements. It computes DAS28-ESR using the formula that combines square root of tender joint count, square root of swollen joint count, natural logarithm of ESR, and patient global assessment with appropriate weights. It computes DAS28-CRP using the CRP-adjusted formula. It also calculates CDAI (sum of TJC28, SJC28, patient global, evaluator global) and SDAI (CDAI plus CRP).

This service is stateless because it takes component values as input and returns a calculated score value object. The calculation logic is clinically complex and used across multiple assessment workflows, making it inappropriate to embed in a single entity. The service validates that all input components are within acceptable ranges before performing calculations.

### SerologyInterpretationService

The SerologyInterpretationService applies clinical interpretation rules to raw laboratory values. It determines RF positivity based on the assay method and reference range. It interprets anti-CCP levels using manufacturer-specific cutoff values. It interprets ANA patterns and titers in the context of clinical presentation. It categorizes complement levels (C3, C4) as normal, low, or critically low.

This service encapsulates interpretation logic that depends on laboratory-specific reference ranges and clinical context, making it unsuitable for a simple value object constructor.

## Autoimmune Disease Context Services

### DiseaseClassificationService

The DiseaseClassificationService applies standardized classification criteria to determine disease diagnosis. For rheumatoid arthritis, it applies the 2010 ACR/EULAR classification criteria by scoring joint involvement (0-5 points), serology (0-3 points), acute phase reactants (0-1 point), and symptom duration (0-1 point), then determines whether the total score of 6 or greater is met.

For systemic lupus erythematosus, it applies SLICC/ACR criteria requiring at least 4 of 11 clinical and immunologic criteria or lupus nephritis with supporting serologic evidence. For vasculitis, it applies ACR classification criteria or Chapel Hill Consensus definitions.

This service spans multiple entities and value objects and applies complex multi-criteria decision logic, making it a natural domain service.

### FlareDetectionService

The FlareDetectionService monitors disease activity changes to identify flares. It compares the current disease activity score with the previous stable measurement and applies disease-specific thresholds to determine whether a clinically significant worsening has occurred. For RA, a DAS28 increase of greater than 1.2 from remission or greater than 0.6 from low disease activity constitutes a flare. The service considers the minimum clinically important difference for each scoring instrument.

When a flare is detected, the service produces a DiseaseFlareDetected domain event with the prior and current disease activity levels, enabling downstream contexts to respond.

### TreatToTargetService

The TreatToTargetService evaluates whether a patient's current treatment is achieving the predefined disease activity target. It takes the current disease activity measurement, the target level, the treatment duration, and the minimum assessment interval as inputs. It determines whether the target is met, whether sufficient time has elapsed to evaluate treatment effect, and whether treatment escalation should be recommended. This service implements the treat-to-target algorithm endorsed by ACR and EULAR guidelines.

## Joint Disease Context Services

### SynovialFluidAnalysisService

The SynovialFluidAnalysisService interprets synovial fluid analysis results to support joint disease diagnosis. It classifies the fluid as non-inflammatory (WBC below 2000), inflammatory (WBC 2000-50000), or septic (WBC above 50000) based on white blood cell count. It interprets crystal findings to distinguish gout (negatively birefringent needle-shaped crystals) from pseudogout (weakly positively birefringent rhomboid crystals). It integrates cell count, crystal analysis, and Gram stain results to produce a diagnostic interpretation.

### UrateTargetService

The UrateTargetService evaluates gout management against serum urate targets. It determines the appropriate urate target (below 6 mg/dL for most patients, below 5 mg/dL for tophaceous gout), compares current serum urate levels against the target, and recommends dose adjustments for urate-lowering therapy based on the difference between current and target levels.

## Biologic Therapy Context Services

### BiosimilarSwitchingService

The BiosimilarSwitchingService evaluates and facilitates transitions between reference biologic products and biosimilars. It assesses patient eligibility for switching based on current disease stability, prior adverse events, and contraindications. It generates a switching protocol including patient notification requirements, monitoring schedule, and outcome assessment timeline. It ensures regulatory and institutional requirements for biosimilar switching are met.

### PreTreatmentScreeningService

The PreTreatmentScreeningService determines required pre-treatment evaluations based on the selected biologic agent. It generates a screening checklist (tuberculosis test, hepatitis B/C panels, vaccination review, pregnancy test if applicable) and evaluates completed screening results to determine therapy clearance. It identifies contraindications that prevent therapy initiation.

## Pain Management Context Services

### MultimodalPainService

The MultimodalPainService formulates multimodal pain management plans by integrating pain assessment results, diagnosis, functional limitations, and patient preferences. It recommends a combination of pharmacologic interventions, physical therapy goals, occupational therapy needs, and patient education components. The service ensures that plans address both acute and chronic pain components.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 5.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 7.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 5.
- Smolen, J.S., et al. "Treating Rheumatoid Arthritis to Target: 2014 Update." Annals of the Rheumatic Diseases, 2016.
