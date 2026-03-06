# Cognitive Health Context

## Overview

The Cognitive Health Context addresses dementia screening, cognitive testing using standardized instruments, cognitive decline tracking over time, and memory care planning. Cognitive impairment is one of the most consequential conditions in geriatric care, affecting an estimated 10-15% of adults over 65 and rising sharply with advancing age. The progressive nature of most dementias requires longitudinal tracking and adaptive care planning that this context models.

This context is classified as a supporting subdomain. While critical to geriatric care, cognitive testing instruments are well-standardized, and clinical workflows follow established protocols. The context supports the core Geriatric Assessment and Care Coordination contexts by providing specialized cognitive data that informs care intensity, safety planning, medication review, and advance care planning decisions.

## Ubiquitous Language

Key terms include Mini-Mental State Examination (MMSE), Montreal Cognitive Assessment (MoCA), mild cognitive impairment (MCI), dementia, Alzheimer's disease, vascular dementia, Lewy body dementia, frontotemporal dementia, Global Deterioration Scale, Clinical Dementia Rating, behavioral and psychological symptoms of dementia (BPSD), wandering, agitation, sundowning, memory care plan, cognitive trajectory, neuropsychological assessment, clock drawing test, and decision-making capacity.

## Aggregates

### CognitiveScreening

The primary aggregate root for cognitive evaluation. Contains CognitiveTestResult value objects for each administered instrument (MMSE, MoCA, Clock Drawing Test) and a CognitiveStatusDetermination entity that synthesizes results into an overall cognitive classification. Enforces the invariant that the cognitive status determination must be consistent with the objective test scores. Publishes CognitiveDeclineDetected when scores indicate significant decline.

### MemoryCarePlan

Models the specialized care approach for individuals with diagnosed cognitive impairment. Contains BehavioralIntervention entities (for managing BPSD), CaregiverEducationPlan entities (for training caregivers in dementia care techniques), and SafetyPrecaution value objects (wandering prevention, medication safety, kitchen safety). Enforces the invariant that the plan addresses both patient safety and caregiver support.

### DementiaStaging

Models the longitudinal tracking of dementia severity. Contains sequential DementiaStage value objects with dates, creating a record of disease progression. Enforces the invariant that staging follows the ordinal progression of the staging instrument. Publishes DementiaStageProgressed when a patient advances to a more severe stage.

## Entities

DementiaStaging entity records a specific severity classification at a point in time, including the instrument used, stage assigned, clinician, and date. BehavioralIncident entity records specific BPSD events with type, triggers, interventions, and outcomes. CognitiveScreening entity tracks a specific evaluation session with patient, clinician, instruments used, and results.

## Value Objects

CognitiveTestScore captures the instrument name, raw score, maximum score, and interpretation. DementiaStage captures the staging instrument, stage number, and descriptive label. BPSDSeverity captures the type of behavioral symptom and its severity classification. DecisionMakingCapacity captures the assessed ability to understand, appreciate, reason, and communicate treatment choices.

## Domain Events

CognitiveDeclineDetected is published when sequential test scores show significant decline beyond normal test-retest variability. DementiaStageProgressed is published when staging advances. MemoryCarePlanEstablished is published when a new memory care plan is created. BPSDEpisodeRecorded is published when a significant behavioral incident occurs, informing medication review and care plan adjustment.

## Domain Services

CognitiveTrajectoryService analyzes sequential cognitive test results, accounting for practice effects and test-retest reliability, to determine whether decline is within normal aging or suggestive of pathological impairment. DementiaScreeningRecommendationService determines whether a patient should receive detailed cognitive screening based on age, risk factors, and prior screening results.

## Clinical Workflow

Cognitive screening typically begins with a brief screening during the comprehensive geriatric assessment. If the screening suggests cognitive concerns, a CognitiveDeclineDetected event (or an initial cognitive concern flag) triggers detailed evaluation within this context. The detailed evaluation uses multiple instruments and may include referral for neuropsychological testing.

When cognitive impairment is confirmed, dementia staging establishes the severity level, and a memory care plan is developed. The plan addresses safety precautions, behavioral management strategies, caregiver education, and environmental modifications. As the disease progresses, the plan is updated in response to staging changes and behavioral incident patterns.

Longitudinal tracking compares sequential assessments to detect the rate and pattern of cognitive decline, informing prognostic discussions and care intensity adjustments. Cognitive trajectory analysis helps distinguish between stable mild cognitive impairment, slowly progressive decline, and rapidly progressive conditions requiring urgent intervention.

## Integration Points

This context consumes AssessmentCompleted events from the Geriatric Assessment Context when initial cognitive screening suggests concerns. It publishes CognitiveDeclineDetected and DementiaStageProgressed events consumed by Care Coordination (for care plan adjustment), Medication Management (for review of cognitive-impairing medications), End of Life Planning (for timely advance care planning while capacity remains), and Functional Independence (for safety assessment).

The context integrates with neuropsychology testing systems through an anti-corruption layer that translates external test data into the domain's value objects.

## Regulatory Considerations

Cognitive screening in Medicare-covered annual wellness visits is supported by CMS guidelines. Documentation of cognitive status is required for certain care settings, including the MDS for nursing homes. Dementia diagnosis reporting requirements vary by jurisdiction.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media.
- Folstein, M.F., Folstein, S.E., & McHugh, P.R. (1975). Mini-Mental State: A Practical Method for Grading the Cognitive State of Patients for the Clinician. Journal of Psychiatric Research.
- Nasreddine, Z.S. et al. (2005). The Montreal Cognitive Assessment, MoCA: A Brief Screening Tool for Mild Cognitive Impairment. JAGS.
