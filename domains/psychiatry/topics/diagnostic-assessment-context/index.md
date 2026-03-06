# Diagnostic Assessment Context

## Overview

The Diagnostic Assessment Context is a core bounded context in the psychiatry domain responsible for managing the complete psychiatric evaluation process. This context owns the workflows for conducting structured clinical interviews, applying DSM-5-TR diagnostic criteria, administering and scoring standardized rating scales, formulating differential diagnoses, and maintaining the patient's longitudinal diagnostic profile. It serves as the upstream supplier of diagnostic information to the Medication Management and Psychotherapy contexts.

## Responsibility

This context answers the fundamental clinical question: "What is the patient's psychiatric diagnosis?" It encompasses all activities from the initial evaluation request through diagnostic formulation and ongoing diagnostic reassessment. The context does not prescribe treatment; it provides the diagnostic foundation upon which treatment decisions are made in downstream contexts.

## Ubiquitous Language

Within this context, the following terms have specific meanings:

- **Evaluation**: A comprehensive psychiatric assessment conducted by a qualified clinician, encompassing clinical interview, mental status examination, and diagnostic formulation.
- **Diagnostic Criteria**: The specific symptom, duration, and impairment requirements defined in DSM-5-TR that must be met for a diagnosis to be assigned.
- **Rating Scale**: A validated psychometric instrument used to quantify symptom severity; always refers to a standardized, published instrument with established reliability and validity.
- **Differential Diagnosis**: The systematic process of distinguishing between diagnostic possibilities; always a clinician-driven analytical process, not a simple lookup.
- **Diagnostic Profile**: The complete set of active and historical diagnoses for a patient, maintained as the authoritative diagnostic record.

## Aggregates

### Psychiatric Evaluation (Root Aggregate)

The central aggregate representing a single evaluation episode. It enforces rules about evaluation completeness, clinical documentation requirements, and diagnostic assignment validity.

Components:
- Evaluation metadata (date, type, setting, referral source)
- Presenting complaint and history of present illness
- Mental status examination findings (appearance, behavior, mood, affect, thought process, thought content, perception, cognition, insight, judgment)
- Psychiatric history, medical history, substance use history, family history, social history
- Rating scale administrations with scores
- Clinical impressions and diagnostic formulation
- Assigned diagnoses with specifiers and confidence levels
- Evaluation status (initiated, in progress, pending review, finalized)

Invariants:
- An evaluation cannot be finalized without at least one clinical impression.
- Rating scale scores must fall within the valid range for the instrument.
- Diagnoses cannot be assigned until the clinical interview portion is documented.
- A finalized evaluation must be signed by the responsible clinician.

### Diagnostic Profile (Root Aggregate)

Maintains the patient's longitudinal diagnostic record across all evaluations.

Components:
- Active diagnoses with assignment dates and source evaluation references
- Resolved diagnoses with resolution dates and reasons
- Provisional diagnoses under evaluation
- Rule-out diagnoses being actively excluded

Invariants:
- A diagnosis cannot be simultaneously active and resolved.
- Each active diagnosis must reference the evaluation in which it was assigned.
- Provisional diagnoses must be resolved (confirmed or excluded) within a defined timeframe.

## Value Objects

- **Diagnosis Code**: DSM-5-TR or ICD-10-CM code with display name and specifiers.
- **Rating Scale Score**: Instrument name, total score, subscale scores, severity interpretation.
- **Mental Status Finding**: Examination domain, finding description, clinical significance.
- **Diagnostic Specifier**: Specifier type (severity, course, features) and value.
- **Clinical Impression**: Formulation narrative, diagnostic hypothesis, supporting evidence.

## Domain Events

- **Evaluation Initiated**: Signals that a new psychiatric evaluation has begun.
- **Diagnosis Assigned**: Signals that a diagnosis has been assigned to a patient, consumed by Medication Management, Psychotherapy, and Outcomes Measurement contexts.
- **Diagnosis Revised**: Signals that a previously assigned diagnosis has been changed.
- **Diagnosis Resolved**: Signals that a diagnosis is no longer active.
- **Rating Scale Completed**: Signals completion of a standardized assessment, consumed by Outcomes Measurement.

## Integration Points

- **Upstream to Medication Management**: Publishes diagnostic information that informs prescribing indications.
- **Upstream to Psychotherapy**: Publishes diagnostic information that informs modality and protocol selection.
- **Shared Kernel with Crisis Management**: Provides real-time access to current diagnoses during psychiatric emergencies.
- **Downstream from Outcomes Measurement**: Receives outcome data that may prompt diagnostic reassessment.

## Clinical Workflow

1. Evaluation is initiated by referral, self-presentation, or scheduled reassessment.
2. Clinician conducts structured clinical interview gathering presenting complaint, psychiatric history, and relevant background.
3. Mental status examination is performed and documented.
4. Standardized rating scales are administered based on presenting symptoms.
5. Clinician formulates differential diagnosis considering all gathered information.
6. Diagnoses are assigned with appropriate specifiers and confidence levels.
7. Evaluation is finalized with clinician signature.
8. Diagnostic profile is updated and domain events are published.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- American Psychiatric Association. *DSM-5-TR*. APA Publishing, 2022.
- First, Michael B., et al. *SCID-5-CV: Structured Clinical Interview for DSM-5 Disorders*. APA Publishing, 2016.
- Kroenke, Kurt, et al. "The PHQ-9: Validity of a Brief Depression Severity Measure." *Journal of General Internal Medicine*, 2001.
