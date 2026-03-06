# Domain Services: Attention-Deficit Domain

Domain services encapsulate stateless operations that involve multiple aggregates or do not naturally belong to any single entity or value object. In the ADHD management domain, domain services coordinate complex clinical workflows, cross-aggregate calculations, and business logic that spans multiple domain objects.

## Purpose

Many ADHD management operations span multiple aggregates or require coordination logic that does not belong to any single entity. Determining whether a patient meets DSM-5 diagnostic criteria requires evaluating data across multiple rating scales, clinical interviews, and observational reports. Generating a multimodal treatment recommendation requires considering assessment results, patient history, comorbidity profile, and available treatment resources. Domain services provide a home for this cross-cutting business logic while keeping it within the domain layer, free from infrastructure concerns.

## Clinical Assessment Context Services

### DiagnosticEvaluationService

Coordinates the comprehensive diagnostic evaluation process. This service:

- Evaluates whether collected assessment data meets the threshold for a formal DSM-5 ADHD diagnosis by checking symptom count criteria across informant sources.
- Determines the ADHD presentation type (inattentive, hyperactive-impulsive, combined) based on symptom dimension analysis.
- Identifies potential comorbidities by cross-referencing symptom patterns against differential diagnosis criteria for anxiety, depression, oppositional defiant disorder, and learning disabilities.
- Generates a diagnostic impression summary that integrates rating scale scores, clinical interview findings, and neuropsychological test results.

This service operates across multiple Assessment Session entities and Rating Scale Score value objects within the Patient Assessment aggregate.

### RatingScaleInterpretationService

Provides standardized interpretation of ADHD rating scale results. This service:

- Converts raw scores to T-scores or percentiles using age- and gender-appropriate normative tables.
- Classifies scores against clinical significance thresholds (e.g., T-score above 65 on Conners ADHD Index).
- Compares multi-informant scores to assess cross-setting consistency (parent vs. teacher ratings).
- Identifies subscale patterns that suggest specific ADHD presentation types or comorbid conditions.

## Treatment Management Context Services

### TreatmentRecommendationService

Generates evidence-based treatment recommendations based on assessment findings and patient characteristics. This service:

- Selects appropriate treatment modalities based on ADHD presentation type, severity level, age, comorbidity profile, and prior treatment history.
- Applies clinical practice guideline algorithms (AAP, NICE) to determine first-line and second-line intervention recommendations.
- Sequences treatment interventions based on evidence-based prioritization (e.g., behavioral therapy before medication for preschool-age children per AAP guidelines).
- Estimates treatment intensity (session frequency, duration) based on symptom severity and functional impairment level.

### TreatmentCoordinationService

Manages the coordination of multiple concurrent treatment modalities within a multimodal plan. This service:

- Detects scheduling conflicts between therapy sessions, coaching appointments, and medication follow-ups.
- Ensures that behavioral objectives are consistent across all active treatment modalities.
- Coordinates treatment episode transitions (e.g., stepping down from intensive behavioral therapy to maintenance coaching).

## Behavioral Monitoring Context Services

### SymptomTrendAnalysisService

Analyzes longitudinal behavioral monitoring data to identify clinically meaningful trends. This service:

- Calculates symptom frequency trends across defined observation periods using statistical methods appropriate for behavioral data.
- Detects significant changes in symptom patterns that warrant clinical attention (e.g., sustained increase in inattention symptoms over two weeks).
- Compares symptom patterns across settings (home vs. school) to identify environment-specific triggers.
- Correlates symptom changes with treatment modifications (medication adjustments, therapy session changes) to support treatment response evaluation.

### ExecutiveFunctionAssessmentService

Evaluates executive function performance based on monitoring data. This service:

- Aggregates working memory, cognitive flexibility, planning, and inhibitory control measurements into an executive function profile.
- Compares current performance against the individual's baseline and age-appropriate norms.
- Identifies specific executive function deficits that may require targeted intervention.

## Medication Management Context Services

### TitrationProtocolService

Manages the medication dosage optimization process. This service:

- Generates titration schedules based on medication type, starting dosage, and target therapeutic range.
- Evaluates titration step outcomes by correlating dosage changes with symptom response and side effect reports.
- Recommends dosage adjustments based on the balance between symptom improvement and adverse effects.
- Determines when a medication trial should be concluded due to insufficient response or intolerable side effects.

### MedicationInteractionService

Evaluates potential interactions between ADHD medications and other prescribed medications. This service:

- Screens for known drug-drug interactions between stimulant/non-stimulant ADHD medications and medications prescribed for comorbid conditions.
- Assesses contraindication risk based on the patient's medical history and current medication list.
- Generates interaction alerts with clinical significance classifications.

## Outcomes Tracking Context Services

### TreatmentEffectivenessService

Evaluates the overall effectiveness of ADHD management interventions. This service:

- Calculates treatment response by comparing pre-treatment baseline measurements with post-treatment outcome measurements using validated statistical methods.
- Classifies treatment response as full response, partial response, or non-response based on standardized criteria.
- Generates comparative effectiveness analysis across treatment modalities for the same patient.
- Produces longitudinal outcome summaries for clinical review and treatment planning.

## Design Principles

**Stateless operations.** Domain services do not maintain state between invocations. All required data is passed as parameters or retrieved through repositories.

**Domain logic, not infrastructure.** Domain services contain business rules and clinical logic. They do not handle database access, API calls, or user interface concerns.

**Named after domain operations.** Service names use ubiquitous language: DiagnosticEvaluationService, not AssessmentProcessor.

## References

- Evans, E. (2003). *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media.
