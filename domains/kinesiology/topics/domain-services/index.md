# Domain Services - Kinesiology Domain

## Overview

Domain services are stateless operations that encapsulate domain logic which does not naturally belong to any single entity or value object. When a significant domain operation requires coordination across multiple aggregates, involves complex calculations that span entity boundaries, or represents a domain concept that is inherently an action rather than a thing, a domain service is the appropriate modeling choice.

In the kinesiology domain, domain services handle operations like generating exercise prescriptions from assessment data, calculating composite risk scores from multiple screening inputs, and matching clinical practice guidelines to rehabilitation scenarios. These operations involve multiple aggregates and embody domain expertise that transcends individual object boundaries.

## Movement Assessment Context Services

### ComprehensiveAssessmentService

This service coordinates the creation of a comprehensive movement assessment that integrates results from multiple individual test batteries. When a clinician performs a full evaluation that includes gait analysis, range of motion testing, manual muscle testing, and functional movement screening, this service orchestrates the assembly of a complete MovementAssessment aggregate from the individual test results.

The service applies clinical logic to derive composite scores, identify movement compensation patterns that only become apparent when multiple test results are considered together, and classify overall functional status based on the totality of findings.

### NormativeComparisonService

This service compares individual assessment results against age-appropriate, sex-appropriate, and activity-level-appropriate normative databases. It takes assessment value objects (JointAngle, MuscleGrade, MovementScore, GaitParameter) and returns percentile rankings and clinical significance classifications. This operation spans the individual assessment aggregate and the normative database, making it a natural domain service.

### AssessmentScoringService

This service calculates composite scores from standardized assessment batteries using the specific scoring algorithms defined by each battery's developers. For example, the Functional Movement Screen composite score requires specific rules for handling bilateral tests, pain scores, and clearing tests that are too complex to embed in individual test entities.

## Exercise Prescription Context Services

### ProgramGenerationService

This service generates an initial exercise program based on assessment findings, training goals, available equipment, and scheduling constraints. It represents the core clinical reasoning that translates "what we found" into "what we should do." The service applies periodization principles, exercise selection algorithms, and dosage calculation rules that require knowledge spanning the ExerciseProgram aggregate, the ExerciseCatalog aggregate, and the assessment summary data from the Movement Assessment Context.

### ProgressionCalculationService

This service determines appropriate training progressions based on current performance data, adherence history, and progression rules defined in the program. It evaluates whether progression criteria have been met across multiple training sessions and calculates the new dosage parameters. This operation spans multiple TrainingSession entities within the ExerciseProgram aggregate.

### LoadAssignmentService

This service translates relative load prescriptions (such as 75 percent of one-repetition maximum) into absolute load values based on the individual's current tested or estimated maximal values. It applies prediction equations, accounts for daily readiness fluctuations when readiness data is available, and rounds to available equipment increments.

## Rehabilitation Planning Context Services

### ReturnToPlayEvaluationService

This service evaluates whether an individual meets all return-to-play criteria defined in their rehabilitation plan. It aggregates current assessment data, milestone achievement status, and clinical judgment criteria to produce a clearance decision. This operation spans the RehabilitationPlan aggregate, current assessment data, and clinical practice guidelines from the Research and Education Context.

### PhaseAdvancementService

This service evaluates whether a rehabilitation plan should advance to the next phase based on milestone achievement, healing timeline, and clinical assessment. It enforces the clinical logic that determines when phase-specific criteria are sufficiently met to justify progression, including mandatory versus optional milestone requirements.

### TherapeuticDosageService

This service calculates appropriate therapeutic exercise dosages based on the current healing phase, pain response, functional status, and evidence-based dosage guidelines. Therapeutic dosage calculation differs from general exercise prescription because it must account for tissue healing constraints and clinical precautions.

## Performance Analysis Context Services

### BiomechanicalAnalysisService

This service processes raw instrumented data (force plate recordings, motion capture data) to derive clinically meaningful biomechanical variables. It applies signal processing algorithms, biomechanical models, and statistical analyses that span multiple trial entities within a PerformanceAssessment aggregate.

### PerformanceProfilingService

This service generates comprehensive performance profiles by integrating data from multiple assessment modalities (force plate, motion capture, field testing) and comparing results against sport-specific normative data. This cross-aggregate analysis identifies strengths, weaknesses, and performance-limiting factors.

## Injury Prevention Context Services

### RiskCalculationService

This service calculates composite injury risk scores by aggregating risk factors from screening results, workload data, movement quality indicators, and injury history. The calculation algorithm applies evidence-based weighting to different risk factors and produces an overall risk classification with contributing factor breakdown.

### WorkloadMonitoringService

This service continuously monitors training load patterns and calculates workload metrics including acute-to-chronic workload ratios, monotony indices, and strain calculations. It operates across WorkloadEntry entities and applies epidemiological thresholds to identify dangerous workload patterns.

### Prehabilitation RecommendationService

This service generates prehabilitation exercise recommendations based on identified risk factors, movement deficits, and sport-specific injury patterns. It matches risk profiles to evidence-based preventive interventions.

## Research and Education Context Services

### EvidenceMatchingService

This service matches clinical scenarios to relevant research evidence and clinical practice guidelines. Given a clinical question (injury type, population, intervention), it retrieves and ranks applicable evidence from the EvidenceBase aggregate.

### CompetencyAssessmentService

This service evaluates practitioner competency against defined competency frameworks and identifies continuing education needs based on gaps between current competency and required standards.

## Domain Service Design Principles

Domain services should be stateless. They receive input, perform calculations or coordination, and return results without retaining any state between invocations.

Domain services should be named using verbs or verb phrases that reflect the domain operation being performed. They should use the ubiquitous language and be recognizable to domain experts.

Domain services should not replace entity behavior. If an operation naturally belongs to a single entity, it should be a method on that entity. Domain services are reserved for operations that genuinely span multiple objects.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 5 on domain services.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 7 on domain service patterns.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 7 on domain services.
