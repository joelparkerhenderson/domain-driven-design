# Domain Services in Gastroenterology

## Overview

A domain service is a stateless operation that belongs to the domain layer but does not
naturally fit within a single entity or value object. Domain services encapsulate
business logic that spans multiple aggregates or requires coordination between domain
objects. In gastroenterology, domain services handle clinical logic that crosses
aggregate boundaries within a bounded context.

## Domain Service Characteristics

Domain services in the gastroenterology domain follow DDD principles:

1. Stateless: domain services hold no internal state between invocations.
2. Defined in domain terms: service methods use ubiquitous language.
3. Operate on domain objects: inputs and outputs are entities, value objects, or
   primitive types from the domain.
4. Contain business logic that does not belong to any single aggregate.
5. Do not depend on infrastructure concerns.

## Clinical Assessment Context Services

### DifferentialDiagnosisService

Generates a ranked differential diagnosis based on the combination of symptoms,
examination findings, and lab results from a PatientEncounter. This service operates
across the encounter's findings and lab order results to apply clinical reasoning
rules. It does not belong to either the PatientEncounter aggregate or the LabOrder
aggregate because it requires data from both.

### AlarmFeatureEvaluationService

Evaluates whether a combination of clinical findings constitutes an alarm feature
requiring urgent workup. While individual findings are recorded within the
PatientEncounter aggregate, the clinical rules for alarm feature identification
involve pattern matching across multiple finding types and patient demographics.
This cross-cutting evaluation logic belongs in a domain service.

### ReferralRoutingService

Determines the appropriate bounded context for patient referral based on clinical
assessment findings. When an assessment identifies potential IBD, liver disease,
motility disorder, or nutritional concern, this service applies routing rules to
generate the appropriate referral type. The routing logic spans the clinical
assessment model and the referral criteria of downstream contexts.

## Endoscopic Procedures Context Services

### QualityMetricsCalculationService

Calculates procedure quality metrics that depend on multiple aspects of the
EndoscopicProcedure aggregate and potentially SpecimenRecord aggregates. For
colonoscopy, this includes adenoma detection rate (requiring historical procedure
data), withdrawal time assessment, and bowel preparation adequacy scoring. The
calculation spans procedure and specimen data that may cross aggregate boundaries.

### SurveillanceIntervalService

Determines the recommended surveillance interval for follow-up endoscopy based on
current procedure findings, pathology results, patient history, and applicable
guidelines (e.g., multi-society post-polypectomy surveillance guidelines). This
service combines data from the current procedure, historical procedures, and
pathology results to apply guideline-based interval logic.

## Inflammatory Disease Context Services

### DiseaseActivityScoringService

Calculates disease activity scores (Harvey-Bradshaw Index for Crohn's, Mayo Score
for UC) using data from the IBDDiagnosis aggregate, recent lab results, and endoscopic
findings. The scoring logic combines clinical symptoms, lab inflammatory markers,
and endoscopic grading into a composite score. This multi-source calculation belongs
in a domain service.

### TherapyOptimizationService

Evaluates the current therapy regimen against disease activity trends, prior therapy
responses, and guideline recommendations to suggest therapy adjustments. This service
operates across the TherapyRegimen and IBDDiagnosis aggregates, applying treat-to-target
algorithms that consider both therapeutic history and current disease state.

## Hepatology Context Services

### MELDScoreCalculationService

Calculates and updates MELD scores when component lab values change. While the MELD
Score itself is a value object, the calculation service coordinates between the
LiverDiseaseDiagnosis aggregate and incoming lab results to produce updated scores
and determine if score changes trigger clinical actions (e.g., transplant listing
priority changes).

### TransplantCandidacyAssessmentService

Evaluates transplant candidacy by aggregating evaluation results across medical,
surgical, psychosocial, and financial domains. This service coordinates data from
the TransplantEvaluation aggregate and the LiverDiseaseDiagnosis aggregate to
produce a comprehensive candidacy determination.

## Motility Disorders Context Services

### ChicagoClassificationService

Applies the Chicago Classification algorithm to manometry data to diagnose esophageal
motility disorders. This service takes raw pressure measurements from the ManometryStudy
aggregate and applies the hierarchical classification algorithm to determine the
specific motility disorder diagnosis.

### RomeIVClassificationService

Evaluates whether a patient's symptom pattern satisfies Rome IV criteria for a specific
functional GI disorder. This service analyzes symptom type, duration, frequency, and
onset timing against the Rome IV diagnostic thresholds to determine if criteria are met.

## Nutrition Management Context Services

### MalnutritionRiskScoringService

Calculates malnutrition risk using validated screening tools (e.g., Malnutrition
Universal Screening Tool, Subjective Global Assessment) based on data from the
NutritionalAssessment aggregate. The scoring logic combines anthropometric, biochemical,
and clinical data according to the specific screening tool algorithm.

### NutrientRequirementCalculationService

Calculates individualized nutrient requirements based on patient weight, clinical
condition, metabolic stress, and disease-specific adjustments. This service operates
across the NutritionalAssessment aggregate and clinical condition data to produce
caloric, protein, and micronutrient targets for the NutritionPlan.

## References

- Evans, E. (2003). *Domain-Driven Design*. Addison-Wesley. Chapter 5: Domain Services.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley. Chapter 7: Domain Services.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media. Chapter 8: Domain Services.
