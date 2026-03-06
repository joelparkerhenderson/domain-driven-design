# Domain Events - Kinesiology Domain

## Overview

Domain events represent significant occurrences within the domain that other parts of the system need to know about. A domain event captures the fact that something meaningful happened, expressed in the ubiquitous language of the domain. Domain events are immutable records of past occurrences; once an event has happened, it cannot be un-happened.

In the kinesiology domain, domain events serve as the primary mechanism for communication between bounded contexts. When a movement assessment is completed, when an exercise program is prescribed, when a rehabilitation milestone is achieved, or when an injury risk threshold is breached, the relevant bounded context publishes an event that other contexts may subscribe to and react upon.

## Movement Assessment Context Events

### AssessmentCompleted

Published when a movement assessment session is finalized and all test results have been recorded and reviewed. This event carries the assessment identifier, the subject identifier, the date, the evaluator identifier, and a summary of key findings including composite scores and identified movement compensations. Exercise Prescription and Rehabilitation Planning contexts consume this event to inform program design.

### MovementDeficitIdentified

Published when the assessment process identifies a significant movement deficit that may require intervention. This event carries the deficit type, the affected joint or body region, the severity classification, and any relevant measurement values. The Injury Prevention Context consumes this event to update risk profiles.

### ReassessmentScheduled

Published when a follow-up assessment is scheduled based on the results of a current assessment. This event carries the scheduled date, the assessment type, and the clinical rationale for reassessment. This enables downstream contexts to anticipate upcoming assessment data.

## Exercise Prescription Context Events

### ProgramPrescribed

Published when a new exercise program is finalized and assigned to an individual. This event carries the program identifier, the subject identifier, the program type, the planned duration, and a summary of training parameters. The Injury Prevention Context consumes this event to begin monitoring training load.

### PhaseTransitioned

Published when a training program transitions from one periodization phase to the next. This event carries the program identifier, the previous phase, the new phase, and the transition criteria that were met. This enables monitoring of program progression across contexts.

### ProgramModified

Published when significant changes are made to an existing program, such as load adjustments, exercise substitutions, or volume modifications. This event carries the modification details and the rationale. The Injury Prevention Context monitors these modifications for their impact on workload patterns.

### TrainingSessionCompleted

Published when a training session is completed and actual performance data is recorded. This event carries the session identifier, the prescribed versus actual load and volume data, and any session feedback. Accumulated session data feeds workload monitoring in the Injury Prevention Context.

## Rehabilitation Planning Context Events

### RehabilitationPlanCreated

Published when a new rehabilitation plan is established for an individual following injury or surgery. This event carries the plan identifier, the subject identifier, the injury classification, the initial healing phase, and the projected timeline. The Movement Assessment Context may use this to schedule reassessments at appropriate recovery milestones.

### MilestoneAchieved

Published when a clinical milestone within a rehabilitation plan is achieved. This event carries the milestone identifier, the plan identifier, the milestone type, and the achievement date and metrics. This event can trigger phase advancement within the rehabilitation plan and inform reassessment scheduling.

### ReturnToPlayCleared

Published when all return-to-play criteria have been met and an individual is cleared for full activity. This event carries the clearance date, the clearing clinician, and a summary of all criteria met. The Exercise Prescription Context consumes this event to transition the individual from rehabilitation programming to standard training programming.

### PhaseAdvanced

Published when a rehabilitation plan advances from one healing phase to the next. This event carries the plan identifier, the previous phase, the new phase, and the clinical criteria that justified the advancement.

## Performance Analysis Context Events

### PerformanceAssessmentCompleted

Published when an instrumented performance assessment session is completed and data has been processed. This event carries the assessment identifier, the subject identifier, the assessment type, and key performance metrics. The Movement Assessment Context may integrate these metrics into the broader assessment picture.

### PerformanceThresholdBreached

Published when a performance metric falls below or exceeds a defined threshold, indicating potential concern or exceptional performance. This event carries the metric type, the threshold value, the actual value, and the direction of breach. The Injury Prevention Context monitors these events for fatigue or overtraining indicators.

## Injury Prevention Context Events

### RiskAlertRaised

Published when an individual's injury risk profile exceeds a defined alert threshold. This event carries the subject identifier, the risk score, the contributing factors, and the recommended actions. The Exercise Prescription Context consumes this event to trigger program modifications that reduce injury risk.

### WorkloadAlertRaised

Published when acute-to-chronic workload ratios move outside the recommended range. This event carries the current ratio, the trend direction, and the recommended load adjustment. This is a critical safety event that may trigger immediate program modifications.

### ScreeningCompleted

Published when an injury risk screening protocol is completed. This event carries the screening results, identified risk factors, and recommended follow-up actions. The Movement Assessment Context may consume this to schedule targeted assessments.

## Research and Education Context Events

### GuidelineUpdated

Published when a clinical practice guideline is updated based on new evidence. This event carries the guideline identifier, the previous version, the new version, and a summary of changes. All contexts may subscribe to this event to update their practices in accordance with current evidence.

### EvidenceReviewCompleted

Published when a systematic review or evidence summary is completed on a topic relevant to the domain. This event carries the topic, the key findings, and the implications for clinical practice.

## Event Design Principles

Events should be named in past tense, reflecting that they record something that already happened. Events are immutable and should carry sufficient data for consumers to react without querying back to the publishing context. Events should use the ubiquitous language of the publishing context.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 8 on domain events.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 8 on domain event patterns.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 8 on domain events and integration.
