# Outcomes Measurement Context

## Overview

The Outcomes Measurement Context tracks treatment effectiveness over time
by collecting standardized symptom measures, performing functional
assessments, aggregating patient-reported outcomes, and generating
effectiveness reports. This bounded context is classified as a generic
subdomain because the measurement methodology applies established
psychometric science and standardized reporting frameworks. However, it
provides the critical feedback loop that informs clinical decision-making
across all other contexts.

## Purpose and Scope

This context answers the evaluative question: is the treatment working, and
how do we know? It encompasses:

- Repeated administration of standardized symptom measures at regular
  intervals throughout treatment (e.g., PHQ-9 every 2-4 weeks).
- Functional assessment to evaluate real-world impact of treatment on the
  patient's ability to work, maintain relationships, and perform daily
  activities.
- Patient-reported outcome collection that captures the patient's own
  perspective on their health status and quality of life.
- Clinically significant change detection using psychometric methods to
  distinguish genuine improvement from measurement noise.
- Treatment effectiveness reporting at individual, clinician, program, and
  population levels.
- Benchmarking against established norms and quality measures.

## Ubiquitous Language

Within this context, the following terms have precise meanings:

- Outcome Record: a longitudinal collection of measurement data points for
  a patient across a treatment episode.
- Measurement Point: a single administration of a validated instrument at a
  specific time, producing a score.
- Clinically Significant Change: improvement (or deterioration) that exceeds
  the threshold for reliable change as determined by the instrument's
  psychometric properties.
- Reliable Change Index: a statistical method for determining whether the
  change between two scores exceeds what would be expected from measurement
  error alone.
- Patient-Reported Outcome: health status information provided directly by
  the patient through validated questionnaires.
- Functional Assessment: evaluation of a patient's capacity to perform
  roles and activities in daily life.

## Aggregate: OutcomeRecord

The OutcomeRecord aggregate is the root of this context. It encapsulates:

- OutcomeRecordId (unique identity)
- Patient reference (by identity)
- Treatment episode reference (TreatmentPlanId)
- Measurement points (MeasurementPoint entities)
- Instruments used (instrument type, version)
- Calculated change metrics (ChangeMetric value objects)
- Functional domain assessments (FunctionalDomain value objects)
- Baseline scores
- Most recent scores
- Treatment response classification (responding, not responding,
  deteriorating, recovered, reliably improved)

Invariants enforced by the aggregate:

- Measurement points must use validated instruments with known psychometric
  properties.
- Scores must fall within the instrument's valid range.
- Measurement points must be chronologically ordered.
- Change metrics can only be calculated when at least two measurement points
  exist for the same instrument.
- Baseline scores must be established before change detection is meaningful.

## Domain Events Published

- OutcomeMeasured: signals that a new measurement has been collected and
  scored. Consumed by Treatment Planning for progress review.
- ClinicallySignificantChange: signals that a patient's scores have crossed
  the threshold for reliable improvement or deterioration. May trigger
  treatment plan review if deterioration is detected.
- TreatmentEffectivenessReported: signals that an effectiveness report has
  been generated for a treatment episode, clinician caseload, or program.

## Domain Services

- ChangeDetectionService: calculates whether score changes are clinically
  significant using the Reliable Change Index and normative comparisons.
- EffectivenessReportingService: aggregates outcome data across patients,
  instruments, and time periods to produce program-level effectiveness
  reports.

## Integration Points

- Upstream: subscribes to events from all five other bounded contexts.
  AssessmentCompleted provides baseline data. SessionCompleted provides
  treatment dose data. MedicationPrescribed and DosageAdjusted provide
  medication timeline data. CrisisInitiated and CrisisResolved provide
  crisis event data. TreatmentPlanCreated and TreatmentPlanRevised provide
  treatment context.
- Downstream: publishes outcome events that Treatment Planning may consume
  to trigger plan reviews when deterioration is detected.
- External: exports data to quality reporting systems (HEDIS, NQF measures)
  through anti-corruption layers. May feed into clinical decision support
  dashboards.

## Measurement Instruments

This context supports validated instruments including:

- PHQ-9 (depression severity, range 0-27)
- GAD-7 (anxiety severity, range 0-21)
- PCL-5 (PTSD symptom severity, range 0-80)
- WHODAS 2.0 (functional disability)
- OQ-45 (general outcome measure)
- CORE-OM (Clinical Outcomes in Routine Evaluation)
- BASIS-24 (Behavior and Symptom Identification Scale)

## Clinical Standards

This context implements:

- Measurement-Based Care principles (Fortney et al., 2017).
- HEDIS behavioral health quality measures.
- NQF-endorsed outcome measures for mental health.
- ICHOM Standard Sets for mental health conditions.
- Patient-Reported Outcomes Measurement Information System (PROMIS).

## Regulatory Considerations

- Outcome data used for quality reporting must meet CMS measure
  specifications.
- Patient-reported outcome data is protected health information under HIPAA.
- Outcome data used for research requires IRB approval and may require
  additional patient consent.
- Parity compliance reporting may require outcome data demonstrating
  equivalent treatment standards for mental health and medical conditions.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- Jacobson, N.S. and Truax, P. "Clinical Significance: A Statistical Approach to Defining Meaningful Change." Journal of Consulting and Clinical Psychology, 1991.
- Fortney, J.C. et al. "A Tipping Point for Measurement-Based Care." Psychiatric Services, 2017.
