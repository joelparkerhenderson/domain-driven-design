# Outcomes Measurement Context

## Overview

The Outcomes Measurement Context is a generic bounded context in the psychiatry domain responsible for tracking clinical outcomes, administering standardized measurement instruments, calculating clinically significant change, and producing quality metrics. This context consumes data from all other bounded contexts and serves as the system's primary mechanism for evaluating treatment effectiveness. It operates as a conformist downstream consumer, accepting data in the formats published by upstream contexts.

## Responsibility

This context answers the clinical and organizational question: "Is treatment working, and how do we know?" It encompasses the administration of validated outcome measures, computation of change scores, identification of clinically significant improvement or deterioration, patient-reported outcome collection, and quality indicator reporting. The context does not make treatment decisions; it provides the evidence base that informs treatment decisions made in other contexts.

## Ubiquitous Language

Within this context, the following terms have specific meanings:

- **Outcome Measure**: A validated psychometric instrument used at defined intervals to quantify changes in a patient's clinical status over time.
- **Measurement Battery**: A configured set of outcome measures selected for a patient based on diagnosis, treatment type, and reporting requirements.
- **Clinically Significant Change**: A change in measurement score that exceeds both the reliable change threshold (accounting for measurement error) and crosses the clinical significance boundary (moving from a clinical to a normative range, or vice versa).
- **Reliable Change Index (RCI)**: A statistical measure indicating whether a score change exceeds what could be attributed to measurement error alone.
- **Patient-Reported Outcome (PRO)**: A health outcome measured directly from the patient's perspective without clinician interpretation, capturing subjective experience of symptoms, functioning, and quality of life.
- **Quality Indicator**: A standardized metric with a defined numerator, denominator, and benchmark used to evaluate the quality of psychiatric care at a population level.
- **Measurement-Based Care**: The systematic use of outcome measurement data to guide clinical decision-making throughout treatment.

## Aggregates

### Outcome Report (Root Aggregate)

The central aggregate representing a synthesized outcome assessment for a patient over a defined period.

Components:
- Patient reference and reporting period (start date, end date)
- Included measurement administrations with scores
- Calculated change scores from baseline for each instrument
- Reliable change indices for each score comparison
- Clinical significance classifications (recovered, improved, unchanged, deteriorated)
- Treatment context references (active diagnoses, current medications, therapy modality)
- Report generation date and responsible analyst
- Report status (draft, reviewed, finalized, distributed)

Invariants:
- An outcome report must include at least one validated measurement administration.
- Change scores must be calculated using the reliable change index appropriate for the specific instrument.
- Clinical significance classification must use the Jacobson-Truax method or a specified alternative method with documented justification.
- A finalized report must be reviewed by a qualified outcomes analyst.

### Measurement Battery (Root Aggregate)

Defines the set of instruments and administration schedule for outcome monitoring.

Components:
- Battery name and description
- List of included instruments with administration frequency
- Target population criteria (diagnoses, treatment types, service settings)
- Administration schedule (baseline, monthly, quarterly, discharge)
- Battery status (active, suspended, retired)
- Psychometric references for each instrument (reliability coefficients, normative data, clinical cutoffs)

Invariants:
- Each instrument in the battery must have established psychometric properties published in peer-reviewed literature.
- The administration schedule must specify at minimum baseline and follow-up measurement points.
- Instruments must be appropriate for the target population (age range, reading level, cultural validation).

## Value Objects

- **Change Score**: Instrument, baseline score, follow-up score, raw change, reliable change index, clinical significance classification.
- **Quality Indicator**: Indicator name, numerator definition, denominator definition, calculated rate, benchmark comparison, reporting period.
- **Measurement Administration**: Instrument name, administration date, raw score, subscale scores, severity interpretation, administrator reference.
- **Psychometric Properties**: Instrument, internal consistency (Cronbach's alpha), test-retest reliability, clinical cutoff score, normative mean, normative standard deviation.
- **Outcome Classification**: Classification category (recovered, improved, unchanged, deteriorated), classification method, confidence level.

## Domain Events

- **Measurement Administered**: A standardized outcome measure has been completed and scored.
- **Outcome Report Generated**: A synthesized outcome report has been produced for a patient.
- **Clinical Deterioration Detected**: A patient's scores indicate clinically significant worsening, triggering alerts to the treatment team.
- **Treatment Response Achieved**: A patient's scores indicate clinically significant improvement meeting response criteria.
- **Remission Achieved**: A patient's scores fall within the normative range, indicating symptom remission.
- **Quality Report Generated**: Population-level quality metrics have been calculated for a reporting period.

## Domain Services

- **Clinically Significant Change Calculation Service**: Applies the Jacobson-Truax method to determine whether score changes are reliable and clinically meaningful.
- **Population Outcome Aggregation Service**: Aggregates individual patient outcomes into population-level quality metrics for reporting and benchmarking.
- **Measurement Battery Selection Service**: Recommends appropriate measurement batteries based on patient diagnosis and treatment context.

## Supported Instruments

The context supports administration and scoring of the following standardized instruments:

- **PHQ-9** (Patient Health Questionnaire-9): Depression severity, range 0-27, clinical cutoff 10.
- **GAD-7** (Generalized Anxiety Disorder-7): Anxiety severity, range 0-21, clinical cutoff 10.
- **PANSS** (Positive and Negative Syndrome Scale): Schizophrenia symptom severity, range 30-210.
- **YMRS** (Young Mania Rating Scale): Mania severity, range 0-60.
- **PCL-5** (PTSD Checklist for DSM-5): PTSD severity, range 0-80, clinical cutoff 33.
- **ORS** (Outcome Rating Scale): General functioning, range 0-40, clinical cutoff 25.
- **WHODAS 2.0** (WHO Disability Assessment Schedule): Functional disability across six domains.
- **SRS** (Session Rating Scale): Therapeutic alliance quality, range 0-40.

## Quality Indicators

The context tracks standardized quality indicators aligned with national behavioral health quality measures:

- Follow-up after hospitalization for mental illness (7-day and 30-day rates)
- Antidepressant medication management (acute and continuation phase adherence)
- Metabolic monitoring for patients on antipsychotic medications
- Depression remission at 12 months
- Screening for clinical depression with follow-up plan
- Suicide risk assessment completion rates

## Integration Points

- **Conformist downstream from Diagnostic Assessment**: Receives evaluation and diagnostic events.
- **Conformist downstream from Medication Management**: Receives prescription and adverse event data.
- **Conformist downstream from Psychotherapy**: Receives session and course completion events.
- **Conformist downstream from Crisis Management**: Receives crisis resolution and safety plan events.
- **Conformist downstream from Rehabilitation Services**: Receives milestone achievement events.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- Jacobson, Neil S., and Truax, Paula. "Clinical Significance: A Statistical Approach to Defining Meaningful Change." *Journal of Consulting and Clinical Psychology*, 1991.
- Lambert, Michael J. "Outcome in Psychotherapy: The Past and Important Advances." *Psychotherapy*, 2013.
- National Committee for Quality Assurance. *HEDIS Measures for Behavioral Health*. NCQA, 2023.
