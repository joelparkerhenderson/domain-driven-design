# Aggregates - Kinesiology Domain

## Overview

Aggregates are clusters of related domain objects that are treated as a single unit for the purpose of data consistency. Each aggregate has a root entity, known as the aggregate root, which controls all access to objects within the aggregate. External objects may only reference the aggregate root, never its internal components. This pattern ensures that all business invariants within the aggregate are enforced consistently.

In the kinesiology domain, aggregates represent the natural groupings of clinical, training, and analytical data that must remain consistent. A movement assessment must maintain consistency between its individual test results, overall scores, and clinical interpretations. An exercise program must keep its periodization structure, exercise selections, and progression rules internally consistent.

## Movement Assessment Context Aggregates

### MovementAssessment Aggregate

The MovementAssessment is the aggregate root. It contains the complete record of an evaluation session, including the subject identity, assessment date, evaluator identity, and the collection of individual tests performed. Internal entities include GaitAnalysis, RangeOfMotionTest, ManualMuscleTest, and FunctionalMovementScreen. Value objects include JointAngle, MuscleGrade, and MovementScore.

The aggregate enforces invariants such as: all tests within an assessment must reference the same subject, assessment status transitions follow a defined sequence (scheduled, in-progress, completed, reviewed), and composite scores are recalculated whenever individual test results change.

### GaitAnalysisSession Aggregate

The GaitAnalysisSession aggregate root manages a complete gait evaluation, including multiple gait trials, stride parameter calculations, and deviation analysis. Internal entities include GaitTrial and GaitCycle. Value objects include StrideLength, CadenceRate, StancePhasePercentage, and GaitDeviationIndex.

The aggregate ensures that derived gait parameters are always consistent with the raw trial data and that statistical summaries across trials accurately reflect the collected data.

## Exercise Prescription Context Aggregates

### ExerciseProgram Aggregate

The ExerciseProgram is the aggregate root. It contains the complete specification of a training plan, from the macrocycle level down to individual exercise parameters. Internal entities include Macrocycle, Mesocycle, Microcycle, and TrainingSession. Value objects include ExerciseDosage, LoadPrescription, RestInterval, and ProgressionRule.

Invariants include: periodization phases must form a contiguous sequence, training volume across a mesocycle must not exceed prescribed limits, and exercise selections must be compatible with the subject's assessed functional capacity.

### ExerciseCatalog Aggregate

The ExerciseCatalog aggregate root manages the library of available exercises with their classifications, contraindications, and regression/progression hierarchies. Internal entities include ExerciseDefinition and ExerciseVariation. Value objects include MuscleGroupTarget, EquipmentRequirement, and DifficultyLevel.

## Rehabilitation Planning Context Aggregates

### RehabilitationPlan Aggregate

The RehabilitationPlan is the aggregate root. It governs the entire recovery trajectory from initial injury through discharge. Internal entities include RehabilitationPhase, ClinicalMilestone, and TherapeuticExerciseSet. Value objects include HealingPhaseClassification, WeightBearingStatus, PainLevel, and ReturnToPlayCriterion.

The aggregate enforces that phase transitions only occur when phase-specific criteria are met, that therapeutic exercises are appropriate for the current healing phase, and that return-to-play clearance requires all mandatory criteria to be satisfied.

## Performance Analysis Context Aggregates

### PerformanceAssessment Aggregate

The PerformanceAssessment is the aggregate root. It encapsulates a complete instrumented testing session. Internal entities include ForcePlateTrial, MotionCaptureTrial, and PerformanceReport. Value objects include GroundReactionForce, JointMoment, AngularVelocity, RateOfForceDevelopment, and PowerOutput.

Invariants ensure that derived metrics are always consistent with raw measurement data and that measurement quality indicators (signal-to-noise ratio, marker dropout rate) meet minimum thresholds.

## Injury Prevention Context Aggregates

### InjuryRiskProfile Aggregate

The InjuryRiskProfile is the aggregate root. It aggregates all relevant risk factors for an individual and tracks risk level changes over time. Internal entities include ScreeningResult, WorkloadHistory, and RiskFactorAssessment. Value objects include AcuteChronicWorkloadRatio, RiskScore, and AlertThreshold.

The aggregate enforces that risk calculations are based on current, valid screening data and that alert thresholds trigger appropriate notifications when risk levels exceed defined limits.

## Research and Education Context Aggregates

### EvidenceBase Aggregate

The EvidenceBase is the aggregate root. It organizes research evidence by clinical topic and strength. Internal entities include EvidenceSummary and ClinicalPracticeGuideline. Value objects include LevelOfEvidence, EffectSize, and ConfidenceInterval.

The aggregate ensures that evidence grading follows established hierarchies and that guideline recommendations are traceable to their supporting evidence.

## Aggregate Design Principles

Keep aggregates small and focused on consistency boundaries. A common mistake is making aggregates too large, which creates contention and performance problems. Each aggregate should protect the minimum set of invariants necessary for domain correctness.

Reference other aggregates by identity rather than direct object reference. An ExerciseProgram references a MovementAssessment by its identifier, not by containing the assessment object. This keeps aggregates independent and enables them to exist in different bounded contexts.

Design aggregate boundaries around transactional consistency requirements. Objects that must be changed together in a single transaction belong in the same aggregate. Objects that can tolerate eventual consistency belong in separate aggregates connected by domain events.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 6 on aggregates.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 10 on aggregate design rules.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 7 on aggregate patterns.
