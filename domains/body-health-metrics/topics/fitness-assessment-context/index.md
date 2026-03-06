# Fitness Assessment Context

## Overview

The Fitness Assessment Context evaluates physical performance capacity through standardized testing protocols. It manages the administration of fitness tests, maintains normative comparison databases, and produces fitness classifications across the major components of physical fitness: cardiorespiratory endurance, muscular strength, muscular endurance, flexibility, and body composition (as it relates to functional performance). This context translates raw performance data into meaningful fitness evaluations using evidence-based assessment methodologies.

## Scope and Responsibilities

This context manages the following assessment categories:

- **VO2 Max Testing**: Maximal oxygen uptake assessment through direct measurement (metabolic cart) or indirect estimation (submaximal protocols, field tests)
- **Strength Testing**: Muscular force production evaluation through one-repetition maximum (1RM) testing, grip strength dynamometry, or isometric assessments
- **Flexibility Assessment**: Range of motion evaluation through sit-and-reach testing, goniometric measurement, or functional movement screening
- **Endurance Benchmarks**: Sustained performance testing through timed distance events, repetition-to-failure protocols, or time-to-exhaustion tests

## Assessment Protocols

The context supports standardized protocols published by recognized organizations:

- **Cooper 12-Minute Run Test**: Field test for VO2 max estimation based on distance covered in 12 minutes
- **Bruce Treadmill Protocol**: Graded exercise test with increasing speed and incline for direct or estimated VO2 max
- **Rockport Walk Test**: Submaximal walking test for cardiorespiratory fitness estimation in lower-fitness populations
- **1RM Protocol**: Progressive loading protocol for maximal strength determination across major movement patterns
- **Sit-and-Reach Test**: Standardized hamstring and lower back flexibility assessment
- **YMCA Bench Press Test**: Upper body muscular endurance assessment using standardized cadence and load
- **Plank Hold Test**: Core muscular endurance assessment measuring time to failure in isometric hold position

## Aggregate Design

### FitnessAssessment (Aggregate Root)

The FitnessAssessment aggregate encapsulates a complete fitness evaluation session, grouping related test results with the assessment conditions and protocol specifications.

**Components**:
- AssessmentProtocol (value object): the standardized testing procedure followed
- EnvironmentalConditions (value object): temperature, humidity, altitude, time of day
- EffortVerification (value object): indicators confirming maximal or valid effort (e.g., respiratory exchange ratio, heart rate plateau)
- TestResults: collection of individual test result value objects
- NormativeComparisons: percentile rankings and fitness classifications for each result

**Invariants**:
- Each test result must reference a valid, recognized assessment protocol
- Environmental conditions must be recorded for tests sensitive to environmental factors
- VO2 max results require effort verification criteria to be met for the result to be classified as a true maximum
- All test results must fall within physiologically possible ranges for the assessed population
- Assessment date and time must be recorded with timezone awareness

### Key Value Objects

- **VO2MaxResult**: Value in ml/kg/min, test protocol used, effort verification status, normative percentile
- **StrengthResult**: Weight lifted or force produced, exercise type, repetition count, normative percentile
- **FlexibilityResult**: Distance or angle achieved, test type, measurement side (left/right/bilateral)
- **EnduranceResult**: Duration, distance, repetitions, or other protocol-specific metric, normative percentile
- **FitnessClassification**: Categorical rating (very poor, poor, fair, good, excellent, superior) derived from normative comparison

## Domain Events Published

- **FitnessAssessmentCompleted**: Raised when all protocol steps are finished and results are verified, carrying full assessment results and fitness classifications
- **VO2MaxMeasured**: Raised specifically for cardiorespiratory fitness results given their distinct clinical significance
- **FitnessClassificationChanged**: Raised when a reassessment produces a different fitness category than the previous assessment, indicating meaningful fitness change

## Domain Services

- **VO2MaxEstimationService**: Applies protocol-specific prediction equations to estimate VO2 max from submaximal test data (heart rate response, time to exhaustion, distance covered)
- **NormativeComparisonService**: Compares individual test results against age-sex-specific population norms from published reference databases (ACSM, Cooper Institute)
- **FitnessClassificationService**: Assigns categorical fitness ratings based on normative percentile position

## Integration Points

- **Upstream**: Consumes biometric data from Biometric Collection Context (resting heart rate, body weight needed for certain protocols)
- **Downstream**: Publishes assessment results consumed by Health Scoring Context and Progress Tracking Context

## Ubiquitous Language (Context-Specific)

- **Maximal Test**: An assessment protocol that requires the individual to perform to the point of volitional exhaustion
- **Submaximal Test**: An assessment protocol that estimates maximal capacity from performance at less-than-maximal intensities
- **Normative Database**: A collection of population reference data organized by age, sex, and fitness level against which individual results are compared
- **Effort Verification**: Objective physiological criteria confirming that an individual achieved sufficient effort for a valid test result
- **Field Test**: A fitness assessment conducted outside a laboratory setting using minimal equipment
- **Functional Capacity**: The maximal physical work an individual can perform, expressed in metabolic equivalents (METs) or oxygen consumption

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- American College of Sports Medicine. *ACSM's Guidelines for Exercise Testing and Prescription*. 11th ed. Wolters Kluwer, 2021.
- Cooper, Kenneth H. *The Aerobics Program for Total Well-Being*. Bantam Books, 1982.
