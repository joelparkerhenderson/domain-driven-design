# Domain Services in Oncology

## Overview

Domain services are stateless operations that encapsulate domain logic which does not naturally belong to a single entity or value object. They represent actions or computations that span multiple aggregates or require coordination between domain concepts. In oncology, domain services capture the clinical reasoning processes, eligibility determinations, and cross-aggregate validations that are central to cancer care but cannot be attributed to any single domain object.

## Characteristics of Oncology Domain Services

Domain services in oncology are stateless: they do not hold any state between invocations. They operate on domain objects passed as parameters and return domain objects as results. They are named using verbs that reflect domain operations in the ubiquitous language. They contain pure business logic with no infrastructure dependencies.

Domain services are distinct from application services, which orchestrate use cases and handle infrastructure concerns. A domain service that calculates chemotherapy dose adjustment based on toxicity grade and protocol rules is a pure domain operation. An application service that loads the chemotherapy order from a repository, invokes the domain service, persists the result, and publishes domain events is an orchestration concern.

## Key Domain Services by Bounded Context

### Diagnosis and Staging Context

**StagingService**: Computes the overall cancer stage group from individual TNM category values, applying the staging rules defined by the AJCC for specific cancer types. This service encapsulates the complex lookup tables and staging algorithms that map T, N, and M combinations to stage groups. The service takes a cancer type and TNM classification as input and returns the computed stage group as a value object.

**MolecularProfileInterpretationService**: Evaluates a set of molecular test results to identify clinically actionable findings. This service applies current clinical knowledge about which mutations are actionable for which cancer types, producing a summary of treatment-relevant molecular findings. It spans the molecular test result entities and the cancer diagnosis aggregate.

### Treatment Planning Context

**ClinicalTrialMatchingService**: Evaluates a patient's clinical profile against the eligibility criteria of available clinical trials. This service takes a cancer diagnosis, staging information, molecular profile, treatment history, and patient demographics, and returns a ranked list of potentially eligible trials with preliminary eligibility assessments. The matching logic spans multiple aggregates and applies complex inclusion and exclusion criteria.

**GuidelineRecommendationService**: Maps a patient's diagnosis, stage, and molecular profile to the recommended treatment options according to NCCN guidelines. This service encapsulates the guideline decision trees that specify first-line, second-line, and subsequent therapy options based on clinical and molecular characteristics. It produces a set of guideline-concordant treatment recommendations.

**TreatmentSequencingService**: Determines the optimal sequencing of treatment modalities (surgery, chemotherapy, radiation) based on the cancer type, stage, and treatment intent. This service applies clinical rules such as neoadjuvant chemotherapy before surgery for locally advanced breast cancer, or concurrent chemoradiation for certain head and neck cancers. The service evaluates the interdependencies between modalities.

### Chemotherapy Management Context

**DoseCalculationService**: Computes patient-specific drug doses from protocol-defined dose rates and patient parameters. This service takes a regimen protocol, the patient's body surface area or weight, organ function values (creatinine clearance for renally cleared drugs), and any dose modification history. It returns calculated dose values for each drug in the regimen, applying dose caps, rounding rules, and any required dose reductions.

**DoseModificationService**: Determines the appropriate dose modification when toxicity is observed during treatment. This service takes the current CTCAE grade for each observed toxicity, the regimen's dose modification guidelines, and the patient's dose modification history. It returns the recommended dose adjustment percentage and any drugs that should be held or discontinued.

**RegimenInteractionService**: Checks for clinically significant drug interactions within a chemotherapy regimen and between the regimen and the patient's concurrent medications. This service spans the chemotherapy order aggregate and external medication data, producing a list of identified interactions with severity ratings and clinical recommendations.

### Radiation Therapy Context

**DoseConstraintEvaluationService**: Evaluates a radiation treatment plan against defined dose-volume constraints for organs at risk. This service takes the planned dose distribution and the set of organ-at-risk constraints, and returns a compliance assessment indicating which constraints are met, which are marginally met, and which are violated. This evaluation spans the treatment plan aggregate and the constraint definitions.

**FractionationRecommendationService**: Recommends an appropriate fractionation scheme based on the treatment site, treatment intent, and clinical factors. This service encapsulates the clinical evidence supporting different fractionation approaches (conventional, hypofractionated, stereotactic) and returns a recommended fractionation scheme with supporting rationale.

### Surgical Oncology Context

**ResectionAdequacyAssessmentService**: Evaluates whether a surgical resection has achieved adequate margins based on the margin assessment results, the cancer type, and established margin adequacy criteria. This service takes the margin assessment value objects and the clinical context, and returns an adequacy determination with any recommended follow-up actions.

**ReconstructionEligibilityService**: Assesses a patient's eligibility for reconstructive surgery following oncologic resection. This service considers the resection extent, the patient's overall health status, planned adjuvant therapies, and patient preferences to produce an eligibility assessment and reconstruction option recommendations.

### Survivorship Care Context

**FollowUpScheduleGenerationService**: Creates a personalized follow-up surveillance schedule based on the cancer type, stage, treatments received, and current clinical guidelines. This service synthesizes recommendations from multiple guideline sources (ASCO, NCCN) to produce a comprehensive surveillance calendar with specified tests and examination intervals.

**LateEffectRiskAssessmentService**: Evaluates a survivor's risk for specific late treatment effects based on the therapies received, doses administered, and individual risk factors. This service produces a risk profile that informs the survivorship care plan's monitoring recommendations.

## Domain Service Design Guidelines

Domain services in oncology should be designed with clear input and output contracts using domain value objects and entities. They should not access repositories directly; instead, the calling application service should load the required aggregates and pass them as parameters. Domain services should be thoroughly tested with clinical scenarios, including edge cases that represent unusual but valid clinical situations.

When a domain service's logic becomes complex, it may indicate the need for a domain model refinement. If the StagingService becomes unwieldy, it may benefit from introducing a Staging Strategy pattern that encapsulates the staging rules for each cancer type separately.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 5: A Model Expressed in Software.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 7: Services.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 8: Domain Model Patterns.
