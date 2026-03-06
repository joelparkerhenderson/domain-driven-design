# Aggregates in the Gerontology Domain

## Overview

Aggregates are clusters of related domain objects treated as a single unit for the purpose of data changes and consistency enforcement. Each aggregate has a root entity that serves as the sole entry point for external access, and the aggregate enforces invariants across all its constituent objects. In the gerontology domain, aggregates model the clinical processes, assessments, care plans, and treatment decisions that form the core of geriatric care.

Evans (2003) defines the aggregate as the fundamental unit of transactional consistency. Vernon (2013) emphasizes designing small aggregates that protect invariants efficiently. In geriatric care, aggregates must balance clinical completeness with transactional simplicity, ensuring that complex multidimensional assessments can be managed without requiring overly large consistency boundaries.

## Geriatric Assessment Context Aggregates

### GeriatricAssessment Aggregate

The GeriatricAssessment aggregate is the root of the comprehensive geriatric assessment process. It contains the FrailtyScreening, FallRiskEvaluation, NutritionalAssessment, and SensoryAssessment entities. The aggregate enforces the invariant that a CGA must include evaluations across all required dimensions before it can be finalized. The aggregate root controls the lifecycle of the assessment, from initiation through completion, and publishes an AssessmentCompleted domain event when all evaluations are recorded.

### FrailtyScreening Aggregate

The FrailtyScreening aggregate models the evaluation of an older adult's frailty status using instruments such as the Clinical Frailty Scale or Fried Frailty Phenotype. It contains GripStrengthMeasurement, GaitSpeedMeasurement, and other physical performance value objects. The invariant ensures that a frailty determination requires a minimum set of measurements before a frailty classification can be assigned.

## Care Coordination Context Aggregates

### CarePlan Aggregate

The CarePlan aggregate is the central artifact of care coordination. It contains CareGoal entities, CareActivity entities, and ProviderAssignment value objects. The aggregate enforces the invariant that every care activity must be linked to at least one care goal, and every care goal must have an assigned responsible provider. The care plan evolves over time through versioning, with each version capturing the state of goals and activities at a point in time.

### CareTransition Aggregate

The CareTransition aggregate models the movement of a patient between care settings. It contains TransferSummary, ReceivingProviderNotification, and MedicationReconciliationRecord entities. The invariant ensures that a care transition cannot be completed without medication reconciliation and a transfer summary.

## Cognitive Health Context Aggregates

### CognitiveScreening Aggregate

The CognitiveScreening aggregate models a cognitive evaluation session. It contains CognitiveTestResult value objects for individual instruments (MMSE, MoCA, Clock Drawing Test) and a CognitiveStatusDetermination entity that synthesizes test results into an overall cognitive classification. The invariant ensures that the cognitive status determination is consistent with the test scores.

### MemoryCarePlan Aggregate

The MemoryCarePlan aggregate models the care approach for individuals with cognitive impairment. It contains BehavioralIntervention entities, CaregiverEducationPlan entities, and SafetyPrecaution value objects. The invariant ensures that the memory care plan addresses both patient safety and caregiver support.

## Functional Independence Context Aggregates

### FunctionalAssessment Aggregate

The FunctionalAssessment aggregate models the evaluation of an older adult's ability to perform daily activities. It contains ADLScore and IADLScore value objects, MobilityEvaluation entities, and EnvironmentalHazardAssessment entities. The invariant ensures that both ADL and IADL evaluations are completed before a functional status determination is made.

### HomeModificationPlan Aggregate

The HomeModificationPlan aggregate models recommended changes to a patient's living environment. It contains ModificationRecommendation entities and SafetyPriority value objects. The invariant ensures that modifications are prioritized by fall risk severity and functional need.

## Medication Management Context Aggregates

### MedicationRegimen Aggregate

The MedicationRegimen aggregate models the complete set of medications prescribed to an older adult. It contains PrescribedMedication entities and DrugInteractionAlert value objects. The invariant ensures that new medications are checked against the Beers criteria and existing medications for interactions before being added to the regimen.

### DeprescribingPlan Aggregate

The DeprescribingPlan aggregate models the systematic reduction of medications. It contains DeprescribingCandidate entities, TaperingSchedule value objects, and MonitoringCheckpoint entities. The invariant ensures that each deprescribing candidate has a monitoring plan for withdrawal effects.

## End of Life Planning Context Aggregates

### AdvanceDirective Aggregate

The AdvanceDirective aggregate models a patient's documented treatment preferences. It contains TreatmentPreference value objects, SurrogateDecisionMaker entities, and LegalWitness value objects. The invariant ensures that the directive is legally valid by requiring appropriate witness and notarization records.

### GoalsOfCareConversation Aggregate

The GoalsOfCareConversation aggregate models the structured discussion between clinicians, patients, and families. It contains DiscussionTopic entities, PatientPreference value objects, and ConversationOutcome entities. The invariant ensures that the conversation outcome is documented with both the patient's stated preferences and the clinical team's recommendations.

## Design Principles

Aggregates in the gerontology domain follow Vernon's (2013) guidance to keep aggregates small and reference other aggregates by identity rather than direct object reference. A CarePlan references a PatientId rather than embedding the entire patient object. This approach supports eventual consistency between bounded contexts and reduces the complexity of transactional boundaries.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media.
