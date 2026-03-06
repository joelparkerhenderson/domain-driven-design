# Domain Services in the Psychiatry Domain

## Overview

A domain service encapsulates business logic that does not naturally belong to a single entity or value object. Domain services are stateless operations that coordinate across multiple aggregates or express domain concepts that are not the responsibility of any one aggregate. In the psychiatry domain, domain services handle cross-aggregate clinical logic such as drug interaction checking, treatment protocol recommendation, risk score calculation, and outcome analysis.

## Characteristics

### Statelessness

Domain services do not hold state between invocations. They receive input, perform domain logic, and return results. A Drug Interaction Checking Service receives a list of medications, evaluates interactions, and returns interaction results without retaining any internal state about previous checks.

### Domain Language

Domain services are named using ubiquitous language terms that domain experts recognize. A service called "DiagnosticFormulationService" is meaningful to a psychiatrist; a service called "DataProcessor" is not. The naming reflects clinical operations.

### No Infrastructure Dependencies

Domain services contain pure business logic. They do not depend on databases, external APIs, or user interfaces. A Treatment Protocol Recommendation Service applies clinical guideline rules to produce recommendations; it does not query a database or call an external clinical decision support API.

## Domain Services by Bounded Context

### Diagnostic Assessment Context

**Differential Diagnosis Service**
- Purpose: Evaluates a set of clinical findings against diagnostic criteria to produce a ranked list of possible diagnoses with supporting and contradicting evidence for each.
- Input: Clinical findings from the psychiatric evaluation (symptoms, history, mental status examination results, rating scale scores).
- Output: Ranked list of diagnostic hypotheses with confidence levels and recommended confirmatory assessments.
- Domain logic: Applies DSM-5-TR diagnostic criteria systematically, considering inclusion and exclusion criteria, duration requirements, and differential rules.

**Diagnostic Severity Assessment Service**
- Purpose: Determines the severity classification for a diagnosis based on symptom count, functional impairment, and rating scale scores.
- Input: Diagnosis code, symptom inventory, functional impairment indicators, relevant rating scale scores.
- Output: Severity specifier (mild, moderate, severe) with supporting justification.
- Domain logic: Applies DSM-5-TR severity criteria specific to each diagnostic category.

### Medication Management Context

**Drug Interaction Checking Service**
- Purpose: Evaluates a proposed medication against a patient's current medication regimen to identify drug-drug and drug-diagnosis interactions.
- Input: Proposed medication, current medication list, current diagnoses, patient characteristics (age, weight, renal function, hepatic function).
- Output: List of interactions with severity classifications, mechanisms, and clinical recommendations.
- Domain logic: Applies pharmacological interaction rules based on drug mechanisms, metabolic pathways, and receptor profiles.

**Polypharmacy Risk Assessment Service**
- Purpose: Evaluates the overall risk profile of a patient's complete medication regimen, considering the cumulative burden of multiple psychotropic medications.
- Input: Complete medication regimen, patient age, comorbidities.
- Output: Polypharmacy risk score, specific risk factors identified, simplification recommendations.
- Domain logic: Applies polypharmacy assessment criteria including anticholinergic burden scoring, QTc prolongation risk summation, and metabolic syndrome risk factors.

**Pharmacogenomic Recommendation Service**
- Purpose: Integrates pharmacogenomic test results with prescribing decisions to recommend dose adjustments or alternative medications.
- Input: Pharmacogenomic profile (metabolizer statuses), proposed or current medication.
- Output: Dosing recommendation (standard, reduced, increased, avoid), alternative medication suggestions, clinical evidence level.
- Domain logic: Applies Clinical Pharmacogenetics Implementation Consortium (CPIC) guidelines for gene-drug pairs.

### Psychotherapy Context

**Treatment Protocol Recommendation Service**
- Purpose: Recommends evidence-based psychotherapy protocols based on the patient's primary diagnosis, comorbidities, patient preferences, and available therapist expertise.
- Input: Primary diagnosis, comorbid diagnoses, patient preferences, available modalities.
- Output: Ranked list of recommended treatment protocols with evidence levels and estimated durations.
- Domain logic: Applies clinical practice guideline mappings from diagnoses to evidence-based treatments, considering comorbidity-specific protocol modifications.

**Therapeutic Alliance Assessment Service**
- Purpose: Evaluates the quality of the therapeutic relationship based on session data and patient feedback to identify alliance ruptures.
- Input: Session outcome ratings over time, patient feedback scores, therapist observations.
- Output: Alliance quality assessment, rupture indicators, recommended repair strategies.
- Domain logic: Applies alliance monitoring criteria using validated alliance measurement thresholds.

### Crisis Management Context

**Suicide Risk Stratification Service**
- Purpose: Integrates multiple risk assessment data points to produce a comprehensive risk stratification.
- Input: Columbia Suicide Severity Rating Scale results, historical risk factors, current protective factors, current clinical status, recent life events.
- Output: Risk stratification level (imminent, acute, chronic, low), recommended interventions, follow-up timeline.
- Domain logic: Applies structured risk stratification algorithms that weight static and dynamic risk factors.

**De-escalation Protocol Selection Service**
- Purpose: Recommends appropriate de-escalation interventions based on the patient's current agitation level, diagnosis, and history.
- Input: Current agitation assessment, patient diagnosis, medication history, de-escalation history, environmental factors.
- Output: Recommended de-escalation interventions in priority order (verbal, environmental, pharmacological).

### Rehabilitation Services Context

**Service Matching Service**
- Purpose: Matches a patient's rehabilitation needs with available service providers and programs.
- Input: Rehabilitation plan goals, patient location, functional assessment results, patient preferences.
- Output: Ranked list of matching service providers with availability and suitability scores.

### Outcomes Measurement Context

**Clinically Significant Change Calculation Service**
- Purpose: Determines whether a patient's score change on a measurement instrument represents clinically meaningful improvement, deterioration, or no change.
- Input: Baseline score, follow-up score, instrument psychometric properties (reliability, normative data).
- Output: Reliable change index, clinical significance classification (recovered, improved, unchanged, deteriorated).
- Domain logic: Applies the Jacobson-Truax method for determining reliable and clinically significant change.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 5 on domain services.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 7 on domain services.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 5 on domain services.
