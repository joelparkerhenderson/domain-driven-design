# Domain Events for Body Health Metrics

## Overview

Domain events in the body health metrics domain represent significant occurrences that domain experts care about. They capture the fact that something meaningful has happened within the domain -- a measurement was recorded, a health score was calculated, a fitness milestone was achieved, or a clinical report was submitted. Domain events serve as the primary communication mechanism between bounded contexts, enabling loose coupling while maintaining semantic richness.

## Biometric Collection Events

### BiometricMeasurementRecorded

Raised when a new body measurement is captured during a measurement session. This is the foundational event that triggers downstream processing across multiple bounded contexts.

- **Payload**: MeasurementSessionId, PersonId, measurement type, measured value (with units and precision), timestamp, DeviceId
- **Consumers**: Body Composition Context, Fitness Assessment Context, Health Scoring Context, Progress Tracking Context
- **Business significance**: Represents the entry point for all body health data into the system

### MeasurementSessionCompleted

Raised when all planned measurements in a session have been recorded and the session is finalized.

- **Payload**: MeasurementSessionId, PersonId, list of completed measurement types, session timestamp
- **Consumers**: Health Scoring Context (triggers composite score recalculation), Progress Tracking Context (updates streak counters)
- **Business significance**: Signals that a complete set of temporally consistent measurements is available for analysis

### PhysiologicalPlausibilityViolation

Raised when a submitted measurement falls outside physiological plausibility ranges, indicating a potential data quality issue.

- **Payload**: MeasurementSessionId, measurement type, submitted value, plausible range, DeviceId
- **Consumers**: Device management, data quality monitoring
- **Business significance**: Protects data integrity by flagging measurements that may reflect device malfunction or data entry error

## Body Composition Events

### CompositionProfileUpdated

Raised when a body composition analysis produces a new or updated composition profile.

- **Payload**: CompositionProfileId, PersonId, fat mass, lean mass, bone mineral density, hydration level, metabolic rate, analysis method, timestamp
- **Consumers**: Health Scoring Context, Progress Tracking Context
- **Business significance**: Indicates that new body composition data is available for health scoring and progress evaluation

### MetabolicRateCalculated

Raised when a basal metabolic rate estimate is computed from composition data.

- **Payload**: PersonId, BMR value (kcal/day), calculation method, contributing measurements
- **Consumers**: Health Scoring Context
- **Business significance**: Provides metabolic baseline data for energy balance and health score calculations

## Fitness Assessment Events

### FitnessAssessmentCompleted

Raised when a fitness assessment session is completed with all protocol steps finished.

- **Payload**: FitnessAssessmentId, PersonId, assessment type, results summary, normative percentiles, fitness classification, timestamp
- **Consumers**: Health Scoring Context, Progress Tracking Context
- **Business significance**: New fitness evaluation data is available for composite scoring and goal progress evaluation

### VO2MaxMeasured

Raised specifically when a cardiorespiratory fitness test produces a VO2 max result.

- **Payload**: PersonId, VO2 max value (ml/kg/min), test protocol, effort verification status, normative percentile
- **Consumers**: Health Scoring Context
- **Business significance**: VO2 max is the gold standard cardiorespiratory metric and has distinct clinical significance

## Health Scoring Events

### HealthScoreCalculated

Raised when a composite health score is computed from available measurement data.

- **Payload**: HealthScoreId, PersonId, composite score value, component scores, scoring algorithm version, timestamp
- **Consumers**: Progress Tracking Context, Clinical Integration Context
- **Business significance**: A new actionable health assessment is available for progress tracking and potential clinical reporting

### HealthThresholdCrossed

Raised when a health score or individual metric crosses a predefined clinical or wellness threshold.

- **Payload**: PersonId, metric type, previous value, new value, threshold value, threshold direction (above/below), clinical significance level
- **Consumers**: Progress Tracking Context, Clinical Integration Context
- **Business significance**: May trigger alerts, goal adjustments, or clinical notifications depending on the threshold crossed

## Progress Tracking Events

### MilestoneAchieved

Raised when an individual reaches a predefined achievement point in their health improvement plan.

- **Payload**: HealthGoalId, PersonId, milestone description, achievement date, metric value at achievement
- **Consumers**: Notification services, engagement systems
- **Business significance**: Recognizes meaningful progress and supports behavioral engagement

### StreakBroken

Raised when an individual's consecutive measurement or activity streak is interrupted.

- **Payload**: PersonId, streak type, streak length (days), break date
- **Consumers**: Engagement systems, goal management
- **Business significance**: Triggers re-engagement workflows and potential goal recalibration

### GoalCompleted

Raised when an individual achieves their target metric value within the goal timeline.

- **Payload**: HealthGoalId, PersonId, target metric, target value, achieved value, completion date
- **Consumers**: Notification services, goal recommendation engine
- **Business significance**: Triggers next-goal recommendation and achievement recognition

## Clinical Integration Events

### ClinicalReportSubmitted

Raised when a health metrics report is successfully transmitted to an external clinical system.

- **Payload**: ClinicalReportId, PersonId, destination system, FHIR resource types included, submission timestamp
- **Consumers**: Audit logging, report tracking
- **Business significance**: Confirms that health data has been shared with clinical systems for physician review

## Event Design Principles

**Past tense naming**: All events are named in the past tense (MeasurementRecorded, not RecordMeasurement) to reflect that they represent facts that have already occurred.

**Self-contained payload**: Each event carries sufficient data for consumers to act without querying back to the originating context. This reduces coupling and improves resilience.

**Immutability**: Published events are immutable facts. They cannot be modified after publication. Corrections are represented as new compensating events.

**Ordering guarantees**: Events for a single PersonId are ordered by timestamp. Cross-person event ordering is not guaranteed.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 8 on domain events.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 8 on domain events.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 6 on domain events and integration.
