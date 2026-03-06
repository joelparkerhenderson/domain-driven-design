# Domain Services for Body Health Metrics

## Overview

Domain services in the body health metrics domain encapsulate stateless operations that do not naturally belong to a single entity or value object. When a domain operation requires coordination across multiple aggregates, applies complex calculation logic that spans entity boundaries, or implements a process that the ubiquitous language describes as an action rather than a thing, a domain service is the appropriate modeling construct. Domain services contain pure business logic with no infrastructure dependencies.

## Key Domain Services

### BMICalculationService

**Context**: Biometric Collection

Calculates Body Mass Index from weight and height measurements, applying the standard formula and classification rules.

- **Operation**: calculateBMI(Weight, Height) returns BodyMassIndex
- **Logic**: Applies the WHO formula (weight in kg divided by height in meters squared), determines classification category, and handles edge cases such as measurements in different unit systems
- **Rationale**: BMI calculation spans two distinct value objects (Weight and Height) and produces a third (BodyMassIndex). Neither Weight nor Height should be responsible for this cross-cutting calculation.

### WaistToHipRatioService

**Context**: Biometric Collection

Calculates the waist-to-hip ratio and determines risk classification based on sex-specific thresholds.

- **Operation**: calculateRatio(WaistCircumference, HipCircumference, BiologicalSex) returns WaistToHipRatio
- **Logic**: Computes the ratio and classifies cardiometabolic risk using WHO thresholds (greater than 0.90 for males, greater than 0.85 for females indicates elevated risk)
- **Rationale**: Risk classification requires knowledge of both measurements and demographic context that neither circumference value object should own.

### BodyCompositionAnalysisService

**Context**: Body Composition

Derives a complete body composition profile from raw biometric measurements using a specified analytical methodology.

- **Operation**: analyzeComposition(MeasurementSession, AnalysisMethod) returns CompositionProfile
- **Logic**: Applies BIA prediction equations, DEXA scan interpretation algorithms, or skinfold caliper formulas depending on the specified method. Validates that required input measurements are present for the chosen method.
- **Rationale**: Composition analysis coordinates data from multiple measurements and applies complex calculation models that do not belong to any single value object.

### BasalMetabolicRateService

**Context**: Body Composition

Estimates basal metabolic rate from body composition and demographic data using validated prediction equations.

- **Operation**: estimateBMR(CompositionProfile, Person) returns BasalMetabolicRate
- **Logic**: Applies Mifflin-St Jeor, Harris-Benedict, or Katch-McArdle equations depending on available data. The Katch-McArdle equation requires lean body mass; others use weight, height, age, and sex.
- **Rationale**: BMR estimation requires data from both the composition profile and person demographics, spanning aggregate boundaries.

### CompositeHealthScoreService

**Context**: Health Scoring

Calculates composite health scores by aggregating component metrics from multiple bounded contexts.

- **Operation**: calculateCompositeScore(PersonId, ScoringAlgorithm) returns HealthScore
- **Logic**: Retrieves current measurements from Biometric Collection, composition data from Body Composition, and fitness results from Fitness Assessment. Applies weighted scoring algorithm with age-sex normalization. Validates completeness of input data before calculation.
- **Rationale**: Composite scoring is the defining cross-aggregate operation in the health scoring context. It synthesizes data from multiple upstream contexts through a complex, multi-step calculation process.

### NormativeComparisonService

**Context**: Health Scoring

Compares individual measurements against population norms to produce percentile rankings and z-scores.

- **Operation**: compareToNorms(measurement value, MeasurementType, AgeGroup, BiologicalSex) returns NormativeComparison
- **Logic**: Looks up appropriate reference population data, calculates percentile rank and z-score, determines classification category
- **Rationale**: Normative comparison requires access to reference population databases and statistical calculation logic that do not belong to individual measurement value objects.

### TrendAnalysisService

**Context**: Progress Tracking

Analyzes longitudinal measurement data to detect trends, calculate rates of change, and project future values.

- **Operation**: analyzeTrend(PersonId, MeasurementType, DateRange) returns ProgressTrend
- **Logic**: Retrieves measurement history, applies statistical trend detection (linear regression, moving averages), filters measurement noise from meaningful change, and projects trajectory toward goal targets
- **Rationale**: Trend analysis spans multiple measurement sessions and applies statistical methods that belong to the progress tracking context rather than individual measurement aggregates.

### GoalRecommendationService

**Context**: Progress Tracking

Recommends health improvement goals based on current measurements, assessment results, and population health guidelines.

- **Operation**: recommendGoals(PersonId) returns list of recommended HealthGoals
- **Logic**: Evaluates current health metrics against guideline-based targets, considers individual measurement history and trends, prioritizes goals by potential health impact, and calibrates target values for achievability
- **Rationale**: Goal recommendation requires synthesis of data across multiple measurement types and application of health guideline knowledge that spans aggregate boundaries.

### FhirTranslationService

**Context**: Clinical Integration

Translates between internal domain model representations and FHIR resource formats for clinical data exchange.

- **Operation**: toFhirObservation(Measurement) returns FHIR Observation; fromFhirObservation(FHIR Observation) returns Measurement
- **Logic**: Maps internal measurement types to LOINC codes, converts units to UCUM format, constructs FHIR resource structures, validates FHIR conformance
- **Rationale**: FHIR translation involves complex mapping logic across multiple value objects and clinical coding systems that does not belong to any single domain entity.

## Design Principles

**Statelessness**: Domain services hold no state between operations. All required data is passed as parameters or retrieved through repository interfaces. This makes services inherently thread-safe and testable.

**Domain language**: Service operations are named using ubiquitous language terms. calculateBMI, analyzeComposition, and analyzeTrend express domain actions, not technical operations.

**Pure business logic**: Domain services contain no infrastructure concerns. Database access, API calls, and message publishing are handled by application services that coordinate domain service invocations with infrastructure operations.

**Explicit dependencies**: Domain services declare their dependencies explicitly through constructor parameters. A CompositeHealthScoreService depends on repositories from multiple contexts, and these dependencies are visible in its interface.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 5 on domain services.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 7 on domain services.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 5 on domain services.
