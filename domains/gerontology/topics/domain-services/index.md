# Domain Services in the Gerontology Domain

## Overview

Domain services are stateless operations that encapsulate domain logic which does not naturally belong to a single entity or value object. When an operation involves multiple aggregates, spans bounded contexts, or represents a domain concept that is inherently procedural rather than object-oriented, a domain service is the appropriate modeling choice. In the gerontology domain, domain services handle complex clinical decision-making processes, cross-aggregate calculations, and coordination logic that transcends individual entities.

Evans (2003) distinguishes domain services from application services and infrastructure services. Domain services contain business logic expressed in the ubiquitous language, have no side effects on persistent state (they delegate persistence to repositories), and operate on domain objects passed as parameters. Vernon (2013) emphasizes keeping domain services thin, delegating as much logic as possible to entities and value objects.

## Geriatric Assessment Context Services

### ComprehensiveAssessmentScoringService

This domain service calculates an overall geriatric risk profile by combining scores from multiple assessment dimensions: frailty, fall risk, nutritional status, cognitive screening, and functional status. No single aggregate owns this cross-dimensional analysis. The service accepts individual assessment scores as value objects and produces a GeriatricRiskProfile value object that synthesizes findings into an actionable risk stratification. The service applies clinical algorithms based on established geriatric assessment protocols.

### FrailtyTrajectoryAnalysisService

This domain service analyzes longitudinal frailty data to determine whether a patient's frailty trajectory is improving, stable, or declining. It accepts a collection of FrailtyScore value objects with associated dates and produces a FrailtyTrajectory value object containing the trend direction, rate of change, and projected status. This analysis spans multiple FrailtyScreening aggregates and thus belongs in a service rather than an entity.

## Care Coordination Context Services

### CareTeamAssemblyService

This domain service recommends the composition of an interdisciplinary care team based on a patient's assessment results and care needs. It accepts assessment findings from the Geriatric Assessment Context and produces a recommended team composition with role assignments. The service applies rules about which professional disciplines are required based on the patient's risk profile, cognitive status, and functional needs.

### CareTransitionRiskService

This domain service evaluates the risk associated with a proposed care transition. It accepts the current care setting, proposed destination, patient acuity level, medication count, cognitive status, and caregiver availability. It produces a TransitionRiskAssessment value object identifying potential gaps in care continuity and recommending mitigation strategies. This service spans multiple aggregates and applies transition-of-care best practices from geriatric medicine.

## Cognitive Health Context Services

### CognitiveTrajectoryService

This domain service analyzes sequential cognitive test results to determine the trajectory of cognitive function over time. It accepts a series of CognitiveTestScore value objects, accounts for test-retest variability and practice effects, and produces a CognitiveTrajectory value object indicating whether decline is within normal aging parameters or suggestive of pathological cognitive impairment. The service applies neuropsychological norms adjusted for age and education.

### DementiaScreeningRecommendationService

This domain service determines whether a patient should receive detailed cognitive screening based on available clinical data. It accepts age, medical history, medication list (to identify cognitive-impairing medications), family history, and any prior screening results. It produces a screening recommendation with the suggested instrument and urgency level. This service integrates data from multiple contexts to make a cross-cutting clinical recommendation.

## Functional Independence Context Services

### FunctionalDeclineDetectionService

This domain service compares sequential functional assessments to detect clinically significant decline. It accepts previous and current ADLScore and IADLScore value objects and applies established minimal clinically important difference (MCID) thresholds to determine whether changes represent true functional decline versus measurement variability. The service produces a FunctionalChangeAssessment value object with clinical significance classification.

### HomeEnvironmentRiskService

This domain service evaluates the safety of a patient's home environment based on fall risk, mobility limitations, cognitive status, and existing modifications. It accepts assessment data from multiple contexts and produces a HomeEnvironmentRiskProfile with prioritized modification recommendations. This cross-aggregate analysis spans functional assessment, fall risk, and cognitive health data.

## Medication Management Context Services

### BeersScreeningService

This domain service screens a patient's medication regimen against the current Beers criteria. It accepts the MedicationRegimen aggregate and patient clinical data (age, renal function, diagnoses) and identifies potentially inappropriate medications. The service produces a BeersScreeningResult containing flagged medications, the specific Beers category, and recommended alternatives. The Beers criteria logic is complex enough to warrant a dedicated service rather than embedding it in the MedicationRegimen aggregate.

### PolypharmacyRiskService

This domain service calculates the cumulative risk associated with a patient's medication burden. It accepts the medication regimen, patient age, renal and hepatic function, and known drug interactions. It produces a PolypharmacyRiskAssessment value object containing the total medication count, interaction risk score, anticholinergic burden score, and deprescribing priority recommendations. This holistic risk assessment spans multiple medication entities and external reference data.

## End of Life Planning Context Services

### AdvanceCarePlanningEligibilityService

This domain service determines whether a patient should be offered advance care planning based on clinical criteria. It accepts the patient's age, diagnosis list, frailty status, cognitive status, and existing advance directive status. It produces an eligibility recommendation with suggested timing and the type of conversation recommended (initial versus review). This service applies clinical guidelines for advance care planning triggers.

### GoalsOfCareAlignmentService

This domain service evaluates whether a patient's current care plan is aligned with their documented goals of care. It accepts the active care plan, advance directives, and recent goals-of-care conversation outcomes. It produces an AlignmentAssessment identifying any discrepancies between stated patient preferences and active treatment plans, enabling clinicians to address misalignment proactively.

## Design Principles

Domain services in the gerontology domain are stateless, operate on domain objects passed as parameters, and return domain objects (typically value objects). They do not access repositories directly but receive the data they need from the application layer. They are named using the ubiquitous language and express clinical concepts rather than technical operations.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media.
