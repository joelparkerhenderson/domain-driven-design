# Outcomes Tracking Context

## Overview

The Outcomes Tracking Context provides longitudinal measurement and reporting of patient outcomes across the rheumatology domain. It aggregates data from all other bounded contexts to build comprehensive patient trajectories, supporting treat-to-target monitoring, quality measure reporting, research data collection, and clinical decision support. This context sits downstream of all other contexts, receiving events and data to construct a unified longitudinal view of each patient's disease course and treatment response.

## Context Boundaries

The Outcomes Tracking Context owns the definition of what constitutes a validated outcome measure, the storage and retrieval of longitudinal measurement series, the comparison of outcomes across time points, and the generation of quality reports. It does not perform clinical assessments (owned by the Clinical Assessment Context), make treatment decisions (owned by the Autoimmune Disease and Joint Disease contexts), or manage therapy administration (owned by the Biologic Therapy Context).

This context has conformist relationships with its upstream suppliers. It accepts data in the formats produced by other contexts and translates them into its own normalized outcome model using anti-corruption layers where necessary.

## Aggregate: OutcomeReport

The OutcomeReport aggregate root maintains the longitudinal outcome record for a patient across one or more measurement instruments. It contains OutcomeRecord entities (individual measurement events), DAS28Trajectory value objects (disease activity trend over time), HAQDIAssessment value objects (functional disability measurements), RadiographicScore value objects (joint damage progression), and PatientReportedOutcome value objects (PROMIS, EQ-5D scores).

The aggregate enforces invariants including: outcome records must reference validated instruments with documented psychometric properties; radiographic scores must include comparison with a baseline study; and treat-to-target assessments must document whether the target was achieved and what clinical action was taken.

## Disease Activity Tracking (DAS28)

The DAS28 trajectory is a core capability of this context. Each DAS28 measurement from the Clinical Assessment Context is recorded as a data point in a longitudinal series. The trajectory captures the score, date, interpretation (remission, low, moderate, high), and the treatment regimen active at the time of measurement.

The context calculates trend metrics including the change from baseline, the change from previous measurement, the time spent in each disease activity category, and the proportion of time in remission or low disease activity. These metrics support treat-to-target monitoring by revealing whether therapy adjustments are achieving the desired response.

The TreatToTargetReviewDue event is generated when a scheduled assessment interval elapses. The interval is disease-specific and configurable: typically 1-3 months for active disease, 3-6 months for patients in stable remission.

## Functional Disability (HAQ-DI)

The HAQ-DI (Health Assessment Questionnaire Disability Index) measures functional disability across eight domains: dressing and grooming, arising, eating, walking, hygiene, reach, grip, and activities. Each domain is scored 0 (without difficulty) to 3 (unable to do). The overall score is the mean of the eight domain scores, adjusted for use of aids/devices and help from others.

The HAQDIAssessment value object captures individual domain scores, aid/device usage, assistance requirements, and the computed overall score. Longitudinal tracking of HAQ-DI reveals functional trajectory. A change of 0.22 or more is considered the minimum clinically important difference. The context tracks whether functional disability is stable, improving, or worsening over time.

## Radiographic Progression

Radiographic progression in inflammatory arthritis is tracked using the Sharp/van der Heijde scoring method. This method scores erosions and joint space narrowing separately in hands/wrists (scored 0-5 per joint for erosions, 0-4 for narrowing) and feet (scored 0-10 per joint for erosions, 0-4 for narrowing). Maximum possible scores are 280 for erosions and 168 for joint space narrowing, totaling 448.

The RadiographicScore value object captures the erosion score, joint space narrowing score, total Sharp score, and the change from baseline (annualized progression rate). Radiographic progression is a key outcome because it represents irreversible joint damage. A change of more than 0.5 units per year is considered significant progression.

For osteoarthritis, the Kellgren-Lawrence grading system is tracked longitudinally to detect structural progression.

## Patient-Reported Outcomes

Patient-reported outcome measures (PROMs) capture the patient's perspective on disease impact. The context manages PROMIS (Patient-Reported Outcomes Measurement Information System) domains including physical function, pain interference, fatigue, and social participation. EQ-5D captures health-related quality of life across five dimensions (mobility, self-care, usual activities, pain/discomfort, anxiety/depression) plus a visual analog scale.

The PatientReportedOutcome value object records the instrument, domain scores, and overall summary score. Longitudinal PROM tracking reveals whether improvements in disease activity translate into improvements in patient-perceived health status, which is not always the case.

## Quality Measure Reporting

The context generates quality measure reports for regulatory and payer requirements. CMS MIPS (Merit-based Incentive Payment System) measures relevant to rheumatology include disease activity assessment and classification, functional status assessment, and appropriate tuberculosis screening before biologic therapy. HEDIS measures include medication management for people with rheumatoid arthritis.

The OutcomeRepository supports population-level queries that aggregate individual outcome records into measure performance rates (numerator/denominator) for submission.

## Domain Events

OutcomeRecorded is published when a validated outcome measure is documented. TreatToTargetReviewDue is published when a scheduled reassessment interval elapses. SignificantProgressionDetected is published when radiographic or functional measures show clinically meaningful worsening.

## Integration Points

This context consumes events from all upstream contexts: AssessmentCompleted from Clinical Assessment, DiseaseFlareDetected and RemissionAchieved from Autoimmune Disease, InjectionAdministered from Joint Disease, TherapyStarted from Biologic Therapy, and PainPlanCreated from Pain Management. It integrates with external quality reporting systems through an anti-corruption layer.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- van der Heijde, D. "How to Read Radiographs According to the Sharp/van der Heijde Method." Journal of Rheumatology, 2000.
- Bruce, B., and J.F. Fries. "The Stanford Health Assessment Questionnaire." Clinical and Experimental Rheumatology, 2005.
- OMERACT Handbook. Outcome Measures in Rheumatology. https://omeract.org/.
