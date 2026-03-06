# Health Scoring Context

## Overview

The Health Scoring Context synthesizes measurements and assessments from multiple upstream bounded contexts into composite health scores, wellness indices, and age-adjusted metrics. It transforms raw data into meaningful, actionable health assessments that communicate overall health status through intuitive numeric and categorical representations. This context represents a core subdomain, as the scoring algorithms, normalization methods, and composite index formulations are the primary sources of competitive differentiation.

## Scope and Responsibilities

This context manages the following scoring capabilities:

- **Composite Health Scores**: Multi-variable scores combining biometric, composition, fitness, and vital sign data into a single health assessment value
- **Wellness Indices**: Normalized scores that integrate physical fitness, body composition, and lifestyle factors
- **Age-Adjusted Metrics**: Measurement values normalized against population expectations for the individual's chronological age and sex
- **Percentile Rankings**: Individual measurement positions relative to reference population distributions
- **Z-Scores**: Standardized deviation scores expressing how far an individual measurement falls from the population mean
- **Trend-Aware Scoring**: Scores that incorporate measurement trajectory, rewarding improvement and flagging decline

## Scoring Algorithms

### Composite Health Score

A weighted aggregate of component metrics producing a score on a 0-100 scale:

- Cardiorespiratory fitness (VO2 max percentile): weighted contribution
- Body composition (body fat percentage, lean mass index): weighted contribution
- Vital signs (resting heart rate, blood pressure): weighted contribution
- Anthropometric indicators (BMI, waist circumference, waist-to-hip ratio): weighted contribution
- Flexibility and mobility scores: weighted contribution

The weighting scheme reflects evidence-based associations between each component and overall health outcomes. Cardiorespiratory fitness receives the highest weight given its strong association with all-cause mortality.

### Wellness Index

A broader assessment incorporating both objective measurements and functional capacity:

- Physical fitness components from Fitness Assessment Context
- Body composition metrics from Body Composition Context
- Vital sign trends from Biometric Collection Context
- Progress trajectory from Progress Tracking Context

### Age-Adjusted Metrics

Each raw measurement is normalized against age-sex reference populations to produce an age-adjusted value that answers the question: "How does this individual's measurement compare to what is expected for their demographic group?"

## Aggregate Design

### HealthScore (Aggregate Root)

The HealthScore aggregate represents a complete composite health assessment calculated at a specific point in time.

**Components**:
- CompositeScore (value object): the overall health score value and scale
- ComponentScores: individual scores for each contributing metric category
- NormativeComparisons: percentile and z-score values for each component
- AgeAdjustments: age-normalized values for each contributing metric
- ScoringAlgorithmVersion: identifier for the specific algorithm version used
- InputMeasurementReferences: identifiers linking to the source measurements from upstream contexts

**Invariants**:
- A composite score requires all mandatory component measurements to be present
- Component weights must sum to 1.0 (100%)
- The composite score must fall within the defined scale range (0-100)
- Each component score must reference a specific, identifiable source measurement
- Scoring algorithm version must be recorded for reproducibility
- Input measurements must be temporally compatible (within a defined recency window)

## Domain Events Published

- **HealthScoreCalculated**: Raised when a new composite score is computed, carrying the full score breakdown
- **HealthThresholdCrossed**: Raised when a score or metric crosses a predefined threshold (e.g., moving from "fair" to "good" health category, or crossing a clinical risk boundary)
- **ScoreImprovementDetected**: Raised when trend analysis shows statistically significant score improvement over a defined period
- **ScoreDeclineDetected**: Raised when trend analysis shows statistically significant score decline, potentially triggering clinical notification

## Domain Services

- **CompositeHealthScoreService**: Orchestrates the multi-step scoring process, gathering input data from upstream context repositories, applying the scoring algorithm, and producing the composite score
- **NormativeComparisonService**: Calculates percentile rankings and z-scores against age-sex-specific reference population data
- **AgeAdjustmentService**: Normalizes raw measurement values to age-sex expectations using published reference standards
- **ScoreTrendService**: Analyzes composite score history to detect improvement, decline, or plateau patterns

## Integration Points

- **Upstream**: Consumes measurement events from Biometric Collection, composition profiles from Body Composition, and assessment results from Fitness Assessment
- **Downstream**: Publishes scoring events consumed by Progress Tracking Context and Clinical Integration Context

## Ubiquitous Language (Context-Specific)

- **Composite Score**: A single value synthesized from multiple component metrics using a weighted algorithm
- **Component Weight**: The relative contribution of a single metric category to the composite score
- **Normative Reference**: Population-based statistical data against which individual values are compared
- **Scoring Algorithm Version**: An immutable identifier for a specific set of weights, thresholds, and calculation rules
- **Recency Window**: The maximum age of input measurements that are acceptable for inclusion in a current score calculation
- **Health Category**: A qualitative classification (poor, fair, good, very good, excellent) derived from the composite score

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- American College of Sports Medicine. *ACSM's Guidelines for Exercise Testing and Prescription*. 11th ed. Wolters Kluwer, 2021.
- Warburton, D.E.R. et al. "Health benefits of physical activity: the evidence." *Canadian Medical Association Journal*, 174(6), 2006.
