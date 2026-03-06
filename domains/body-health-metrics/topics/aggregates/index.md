# Aggregates for Body Health Metrics

## Overview

Aggregates in the body health metrics domain define consistency boundaries around clusters of related domain objects that must be treated as a single transactional unit. Each aggregate enforces domain invariants -- the business rules that must always hold true within the boundary. In body health measurement, aggregates ensure that measurement data remains internally consistent, that composition profiles reflect valid physiological states, and that health scores are calculated from complete and compatible input data.

## Key Aggregates

### MeasurementSession Aggregate

**Context**: Biometric Collection

The MeasurementSession aggregate is the primary entry point for body health data. It groups all measurements taken during a single assessment event, ensuring temporal consistency and device attribution.

- **Root entity**: MeasurementSession (identified by MeasurementSessionId)
- **Child entities**: Individual Measurement records (weight, height, waist circumference, vital signs)
- **Value objects**: Timestamp, MeasurementDevice, MeasurementConditions
- **Invariants**: All measurements within a session share the same timestamp and assessment conditions. No two measurements of the same type exist within a single session. Each measurement must pass physiological plausibility validation (e.g., weight between 0.5 kg and 635 kg, height between 50 cm and 280 cm).

### CompositionProfile Aggregate

**Context**: Body Composition

The CompositionProfile aggregate represents the complete body composition analysis for an individual at a point in time.

- **Root entity**: CompositionProfile (identified by CompositionProfileId)
- **Child value objects**: FatMass, LeanMass, BoneMineralDensity, HydrationLevel, BasalMetabolicRate
- **Invariants**: Fat mass plus lean mass must equal total body weight within measurement precision tolerance. All component percentages must sum to 100% within rounding tolerance. Hydration level must fall between 35% and 75% of total body weight.

### FitnessAssessment Aggregate

**Context**: Fitness Assessment

The FitnessAssessment aggregate encapsulates a complete fitness evaluation session following a specific assessment protocol.

- **Root entity**: FitnessAssessment (identified by FitnessAssessmentId)
- **Child entities**: Individual test results (VO2MaxResult, StrengthResult, FlexibilityResult, EnduranceResult)
- **Value objects**: AssessmentProtocol, EnvironmentalConditions, EffortVerification
- **Invariants**: Each test result must reference a valid assessment protocol. Environmental conditions must be recorded for tests sensitive to altitude, temperature, or humidity. VO2 max values must fall between 10 and 100 ml/kg/min.

### HealthScore Aggregate

**Context**: Health Scoring

The HealthScore aggregate represents a composite health assessment derived from multiple measurement inputs.

- **Root entity**: HealthScore (identified by HealthScoreId)
- **Child value objects**: ComponentScore (one per contributing metric), NormativeComparison, AgeAdjustment
- **Invariants**: A composite score cannot be calculated unless all required component measurements are present. Each component score must reference a specific measurement source. The composite score must fall within the defined scoring range (e.g., 0-100).

### HealthGoal Aggregate

**Context**: Progress Tracking

The HealthGoal aggregate manages an individual's health improvement objective from creation through completion.

- **Root entity**: HealthGoal (identified by HealthGoalId)
- **Child entities**: Milestone definitions, progress checkpoints
- **Value objects**: TargetMetric, GoalTimeline, CurrentProgress
- **Invariants**: Target values must be physiologically achievable. Goal timeline must have a start date before or equal to the target date. Milestones must be ordered and non-overlapping. Progress percentage must be between 0% and 100%.

### ClinicalReport Aggregate

**Context**: Clinical Integration

The ClinicalReport aggregate packages health metrics data for transmission to external clinical systems.

- **Root entity**: ClinicalReport (identified by ClinicalReportId)
- **Child value objects**: ClinicalObservation entries, ClinicalCoding (LOINC, SNOMED CT)
- **Invariants**: Every observation must carry a valid clinical code. Report must reference a verified person identity. All measurements must be converted to clinically standard units before inclusion.

## Aggregate Design Principles

**Small aggregates**: Each aggregate is kept as small as possible while maintaining consistency. A MeasurementSession groups only the measurements taken together; it does not include historical measurements or derived scores.

**Reference by identity**: Aggregates reference other aggregates by identity, not by direct object reference. A HealthScore references the MeasurementSessionId from which its component data originated, not the MeasurementSession object itself.

**Eventual consistency between aggregates**: When a MeasurementSession is completed, a domain event triggers asynchronous updates to CompositionProfile, FitnessAssessment, and HealthScore aggregates. These downstream aggregates achieve consistency eventually, not within the same transaction.

**Domain event publication**: Each aggregate publishes domain events when significant state changes occur. These events communicate changes across aggregate and context boundaries.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 6 on aggregates.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 10 on aggregate design rules.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 5 on aggregate boundaries.
