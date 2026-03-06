# Functional Independence Context

## Overview

The Functional Independence Context covers the assessment of an older adult's ability to perform daily activities, the prescription and management of mobility aids, home modification planning, and assistive technology deployment. Functional independence is a central concern in gerontology because the ability to perform activities of daily living (ADL) and instrumental activities of daily living (IADL) directly determines an older adult's quality of life, care needs, living arrangement options, and healthcare costs.

Functional status is one of the strongest predictors of outcomes in older adults, surpassing even diagnosis-based measures in its ability to forecast hospitalization, institutionalization, and mortality. This context models the assessment, intervention, and monitoring processes that support maintaining and restoring functional independence.

## Ubiquitous Language

Key terms include activities of daily living (ADL), instrumental activities of daily living (IADL), Katz Index of Independence in ADL, Lawton Instrumental Activities of Daily Living Scale, functional status, functional decline, mobility assessment, gait analysis, balance assessment, assistive technology, mobility aid, walker, wheelchair, cane, grab bar, home modification, environmental hazard assessment, fall prevention, occupational therapy assessment, physical therapy evaluation, and adaptive equipment.

## Aggregates

### FunctionalAssessment

The primary aggregate root. Contains ADLScore and IADLScore value objects capturing individual domain scores and totals, MobilityEvaluation entities recording gait and balance assessment results, and EnvironmentalHazardAssessment entities documenting home safety findings. Enforces the invariant that both ADL and IADL evaluations must be completed before a functional status determination is finalized. Publishes FunctionalDeclineDetected when scores decline beyond clinically significant thresholds.

### HomeModificationPlan

Models recommended changes to a patient's living environment to improve safety and accessibility. Contains ModificationRecommendation entities (each with type, location, priority, and status) and SafetyPriority value objects. Enforces the invariant that recommendations are prioritized by fall risk severity and functional need. Publishes HomeModificationCompleted when modifications are installed.

### AssistiveTechnologyPlan

Models the assessment, recommendation, and deployment of assistive devices. Contains DeviceRecommendation entities, FittingRecord entities, and TrainingSchedule entities. Enforces the invariant that each device recommendation includes a training plan for the patient and caregiver.

## Entities

FunctionalAssessment entity tracks a specific evaluation event with patient, clinician, date, and assessment results. MobilityAidPrescription entity records the prescription of a specific device with type, prescribing clinician, fitting date, and usage status. HomeModificationRecommendation entity tracks a specific recommended change with type, priority, status, and completion date. EnvironmentalHazardAssessment entity documents identified hazards in the home environment.

## Value Objects

ADLScore captures scores across six domains (bathing, dressing, toileting, transferring, continence, feeding) using the Katz Index, with individual domain scores and a total score from 0 to 6. IADLScore captures scores across eight domains (telephone, shopping, food preparation, housekeeping, laundry, transportation, medication management, finances) using the Lawton Scale. MobilityLevel captures the overall mobility classification (independent, assisted, wheelchair-dependent, bed-bound). EnvironmentalRiskLevel captures the assessed level of home safety risk.

## Domain Events

FunctionalDeclineDetected is published when ADL or IADL scores show clinically significant decrease from previous assessment. HomeModificationCompleted is published when a recommended modification is installed. AssistiveTechnologyDeployed is published when a device is prescribed and set up. MobilityAidPrescribed is published when a new mobility device is recommended for a patient.

## Domain Services

FunctionalDeclineDetectionService compares sequential functional assessments, applying minimal clinically important difference (MCID) thresholds to distinguish true decline from measurement variability. HomeEnvironmentRiskService evaluates overall home safety by combining fall risk data, mobility limitations, cognitive status, and environmental hazard findings to produce a prioritized modification plan.

## Clinical Workflow

Functional assessment typically follows the comprehensive geriatric assessment and is triggered by the AssessmentCompleted event. An occupational therapist or trained clinician administers the Katz ADL and Lawton IADL scales, conducts a mobility evaluation, and performs an environmental hazard assessment if the patient is community-dwelling.

When functional decline is detected, the context generates recommendations for interventions. These may include mobility aid prescription, home modifications, assistive technology deployment, and referral to physical or occupational therapy. Each intervention is tracked from recommendation through implementation and follow-up assessment of effectiveness.

The environmental hazard assessment evaluates the home for fall risks including loose rugs, poor lighting, lack of grab bars, stairs without railings, and bathroom accessibility. Findings drive the home modification plan, which is prioritized based on the patient's fall risk level and functional limitations.

## Integration Points

This context consumes AssessmentCompleted events from the Geriatric Assessment Context to establish functional baselines. It consumes CognitiveDeclineDetected events from Cognitive Health to adjust safety assessments for patients with cognitive impairment. It publishes FunctionalDeclineDetected events consumed by Care Coordination (for care plan updates), Geriatric Assessment (for reassessment triggers), and End of Life Planning (when decline indicates approaching end of life).

## Regulatory Considerations

Functional assessment documentation is required for Medicare home health certification (OASIS), skilled nursing facility care (MDS), and disability determination. Home modification recommendations must comply with ADA accessibility standards and local building codes.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media.
- Katz, S. et al. (1963). Studies of Illness in the Aged: The Index of ADL. JAMA.
- Lawton, M.P. & Brody, E.M. (1969). Assessment of Older People: Self-Maintaining and Instrumental Activities of Daily Living. The Gerontologist.
