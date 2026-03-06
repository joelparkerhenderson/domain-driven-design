# Clinical Assessment Context

## Overview

The Clinical Assessment Context is responsible for the full lifecycle of
mental health evaluation, from initial screening through comprehensive
diagnostic assessment. This bounded context owns the instruments, scoring
algorithms, diagnostic criteria, and clinical interpretation processes that
determine a patient's diagnoses and severity levels. It is classified as a
core subdomain because the quality of diagnostic assessment directly
determines treatment outcomes and represents a primary differentiator in
mental health care delivery.

## Purpose and Scope

This context answers the fundamental clinical question: what is the patient's
current mental health status, and which diagnoses apply? It encompasses:

- Intake screening using brief validated instruments to identify patients
  who need further evaluation.
- Standardized assessment administration using instruments such as the PHQ-9
  (Patient Health Questionnaire for depression), GAD-7 (Generalized Anxiety
  Disorder scale), PCL-5 (PTSD Checklist), and AUDIT (Alcohol Use Disorders
  Identification Test).
- Structured and semi-structured diagnostic interviews based on DSM-5 criteria.
- Clinical formulation that integrates assessment data into a coherent
  explanatory narrative using the biopsychosocial model.
- Severity rating and functional impairment assessment.

## Ubiquitous Language

Within this context, the following terms have precise meanings:

- Assessment: a structured clinical evaluation that produces a scored result
  and clinical interpretation.
- Instrument: a validated questionnaire or interview protocol with defined
  psychometric properties.
- Screening: a brief assessment used to identify individuals who may need
  comprehensive evaluation.
- Score: the numerical result of applying an instrument's scoring algorithm
  to a patient's responses.
- Interpretation: the clinical meaning assigned to a score, including severity
  band and diagnostic implications.
- Clinical Formulation: the integrative narrative that explains the patient's
  presentation in biopsychosocial terms.

## Aggregate: Assessment

The Assessment aggregate is the root of this context. It encapsulates:

- AssessmentId (unique identity)
- Patient reference (by identity)
- Instrument type and version
- Clinician who administered the assessment
- Assessment items and patient responses
- Scoring results (AssessmentScore value object)
- Severity rating (SeverityRating value object)
- Diagnostic codes (DiagnosticCode value objects)
- Clinical formulation narrative
- Status (initiated, in-progress, scored, interpreted, finalized)

Invariants enforced by the aggregate:

- All required items must be completed before scoring.
- Scoring must use the instrument's validated algorithm.
- Diagnostic codes must reference valid DSM-5 or ICD-11 entries.
- Finalized assessments are immutable.

## Domain Events Published

- AssessmentInitiated: signals that a new evaluation has begun.
- AssessmentCompleted: signals that an assessment has been finalized with
  diagnostic results. This is the primary event consumed by the Treatment
  Planning Context.
- ScreeningFlagged: signals that a screening score exceeded the clinical
  threshold and warrants further evaluation.

## Domain Services

- AssessmentScoringService: applies instrument-specific scoring algorithms.
- DiagnosticMatchingService: evaluates symptom profiles against DSM-5 criteria.

## Integration Points

- Upstream: receives patient demographic and historical data from EHR systems
  through an anti-corruption layer.
- Downstream: publishes assessment results to the Treatment Planning Context
  (customer-supplier relationship) and to the Outcomes Measurement Context
  (published language subscriber).
- External: integrates with the 988 Suicide & Crisis Lifeline screening
  protocol when assessments indicate acute risk.

## Clinical Standards

This context implements:

- DSM-5 diagnostic criteria (APA, 2013).
- PHQ-9 scoring and interpretation (Kroenke et al., 2001).
- GAD-7 scoring and interpretation (Spitzer et al., 2006).
- Columbia Suicide Severity Rating Scale (Posner et al., 2011).
- Measurement-Based Care principles (Fortney et al., 2017).

## Regulatory Considerations

- Assessment records are protected health information under HIPAA.
- Substance use disorder assessments receive additional protections under
  42 CFR Part 2.
- Assessment results used for involuntary commitment proceedings must comply
  with state-specific mental health statutes.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- American Psychiatric Association. DSM-5. APA Publishing, 2013.
- Kroenke, K. et al. "The PHQ-9." Journal of General Internal Medicine, 2001.
- Spitzer, R.L. et al. "The GAD-7." Archives of Internal Medicine, 2006.
