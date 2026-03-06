# Entities - Kinesiology Domain

## Overview

Entities are domain objects distinguished by a unique identity that persists over time, even as their attributes change. Unlike value objects, which are defined by their attributes, an entity is defined by a continuity of identity. A patient undergoing movement assessment remains the same patient even as their assessment results, functional capacity, and training history change. The identity, not the current state, determines equality.

In the kinesiology domain, entities represent the people, sessions, programs, and plans that evolve through time and carry historical significance. Understanding which domain concepts are entities versus value objects is a fundamental modeling decision that shapes how the system tracks change and maintains consistency.

## Movement Assessment Context Entities

### Subject

The Subject entity represents the individual being assessed. It carries a unique identifier that persists across all assessments, enabling longitudinal tracking of movement quality over time. Attributes include demographic information, injury history, activity level classification, and assessment history references. The Subject entity does not change identity when their attributes change; a subject who improves their functional movement score is still the same Subject.

### Evaluator

The Evaluator entity represents the clinician or practitioner performing the assessment. It carries credentials, specialization areas, and assessment certification records. The Evaluator identity is important for traceability, inter-rater reliability analysis, and credentialing compliance.

### GaitAnalysis

The GaitAnalysis entity represents a specific gait evaluation within a broader assessment session. It has a unique identity because the same subject may undergo multiple gait analyses over time, and each must be individually trackable. Attributes include the analysis date, conditions (overground versus treadmill, barefoot versus shod), collected trials, and derived parameters.

### FunctionalMovementScreen

The FunctionalMovementScreen entity represents a single administration of a standardized functional movement battery. It tracks individual test scores, composite scores, and clinical interpretations. Its identity enables comparison across repeated administrations to track movement quality changes.

## Exercise Prescription Context Entities

### ExerciseProgram

The ExerciseProgram entity represents a complete training plan assigned to an individual. It evolves over time as phases progress, loads increase, and exercises are modified based on performance feedback. The identity of the program persists even as its internal structure changes through periodization phase transitions.

### TrainingSession

The TrainingSession entity represents a single planned or completed workout within a program. It has a lifecycle from planned to in-progress to completed, and its attributes (actual loads, actual repetitions, session feedback) change during execution. Each session retains its identity to enable retrospective analysis of training adherence and response.

### Mesocycle

The Mesocycle entity represents a training block within a periodized program, typically spanning three to six weeks. It has a distinct identity because mesocycles are individually evaluated for effectiveness, and their outcomes inform subsequent mesocycle design.

## Rehabilitation Planning Context Entities

### RehabilitationPlan

The RehabilitationPlan entity represents the complete recovery trajectory for an individual. It has a lifecycle from creation through active treatment to discharge, with status transitions governed by clinical milestone achievement. Its identity persists throughout the recovery process.

### ClinicalMilestone

The ClinicalMilestone entity represents a specific recovery benchmark that must be achieved before advancing to the next rehabilitation phase. Examples include achieving a defined range of motion threshold, demonstrating single-leg balance for a specified duration, or completing a functional test battery at a required level. Each milestone has a lifecycle from defined to targeted to achieved.

### TherapeuticExerciseSet

The TherapeuticExerciseSet entity represents a collection of exercises prescribed for a specific rehabilitation phase. It has a lifecycle and evolves as the clinician modifies exercises based on patient response. Its identity enables tracking of which exercise sets were prescribed at each stage of recovery.

## Performance Analysis Context Entities

### ForcePlateTrial

The ForcePlateTrial entity represents a single instrumented measurement session on a force plate. It carries a unique identity because individual trials may be included or excluded from analysis based on data quality assessment. Attributes include raw force-time data, sampling rate, trial conditions, and quality indicators.

### MotionCaptureSession

The MotionCaptureSession entity represents a complete motion capture recording session. It tracks the marker set used, capture conditions, processing status, and derived kinematic variables. Its identity enables linking of specific motion data to the assessment in which it was collected.

## Injury Prevention Context Entities

### ScreeningRecord

The ScreeningRecord entity represents a specific administration of an injury risk screening protocol. It carries identity for longitudinal tracking of risk factor evolution and for correlating screening results with subsequent injury outcomes.

### WorkloadEntry

The WorkloadEntry entity represents a single training load observation, recording what an individual did on a specific date. Aggregated over time, workload entries form the basis for acute-to-chronic workload ratio calculations. Each entry's identity enables audit trails and data correction.

## Research and Education Context Entities

### CurriculumModule

The CurriculumModule entity represents a unit of educational content that evolves over time as evidence changes and pedagogical approaches improve. Its identity persists across revisions to enable tracking of curriculum evolution and version management.

### ContinuingEducationRecord

The ContinuingEducationRecord entity tracks a practitioner's completion of continuing education activities. It has a lifecycle from registered to completed to verified and must be individually identifiable for credentialing audits.

## Entity Design Principles

Every entity must have a unique, immutable identifier assigned at creation. This identifier may be a natural key (such as a professional license number for an Evaluator) or a surrogate key (such as a generated UUID for a TrainingSession).

Entity equality is determined by identity, not by attribute comparison. Two TrainingSession objects with identical attributes but different identifiers are different entities.

Entities should encapsulate their state changes through domain-meaningful methods rather than exposing raw setters. A RehabilitationPlan advances through phases via an advancePhase method that enforces milestone criteria, not through direct status field modification.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 5 on entities.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 5 on entity modeling.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 6 on entity patterns.
