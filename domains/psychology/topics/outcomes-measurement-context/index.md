# Outcomes Measurement Context

## Overview

The Outcomes Measurement Context is the bounded context responsible for tracking, analyzing, and reporting the effectiveness of psychological services. It encompasses the administration of standardized outcome measures, the collection of patient-reported outcomes (PROs), the calculation of reliable and clinically significant change, and the benchmarking of individual and program-level outcomes against established standards.

Outcomes measurement serves multiple stakeholders. Clinicians use outcome data to monitor individual client progress and adjust treatment accordingly. Program administrators use aggregate outcome data for quality improvement. Payers and accreditors require outcomes data to evaluate service effectiveness. Researchers use outcomes data to establish evidence bases for treatments. The domain model must serve all of these needs while maintaining the integrity of the measurement process.

## Core Aggregate: Outcome Record

The Outcome Record aggregate is the primary unit of consistency. It is rooted in the Outcome Record entity and contains Measurement Point entities, Outcome Score value objects, and Change Index value objects.

The Outcome Record aggregate enforces invariants including: reliable change calculations require at least two measurement points using the same instrument; clinical significance determinations must reference appropriate normative data; the measurement schedule must be consistent enough to support valid longitudinal comparison; and outcome scores must be linked to their measurement context (pre-treatment, during-treatment, post-treatment, follow-up).

## Key Entities

Outcome Record: The root entity tracking an individual client's outcome data over time. It carries the client reference, the treatment context reference, the primary outcome measure, the measurement schedule, and the current outcome status.

Measurement Point: An entity representing a single administration of an outcome measure at a specific point in treatment. It includes the administration date, the treatment phase (baseline, mid-treatment, termination, follow-up), the session number, and the resulting outcome score.

## Key Value Objects

Outcome Score: An immutable score from a standardized outcome measure, including the measure name, the raw score, any subscale scores, the total score, and the normative classification.

Reliable Change Index: A calculated value indicating whether the change between two measurement points exceeds what would be expected from measurement error alone. It includes the pre-score, post-score, RCI value, and the determination (reliable improvement, no reliable change, reliable deterioration).

Clinical Significance Threshold: The cut-score on an outcome measure that separates the clinical population from the normative population. Crossing this threshold from the clinical side to the normative side represents clinically meaningful recovery.

Benchmark Comparison: The relationship between an individual's or program's outcomes and an established reference standard, expressed as relative performance (above, at, or below benchmark).

Treatment Response Category: A classification of the client's overall treatment response based on reliable change and clinical significance: recovered (reliable change plus normative functioning), improved (reliable change without reaching normative functioning), unchanged (no reliable change), or deteriorated (reliable negative change).

## Standardized Outcome Measures

The context supports multiple families of standardized outcome measures commonly used in psychology practice.

Symptom-focused measures track the severity of specific symptoms. Examples include the PHQ-9 for depression, the GAD-7 for anxiety, the PCL-5 for PTSD, and the AUDIT for alcohol use.

Broad-spectrum measures capture overall psychological distress. Examples include the OQ-45 (Outcome Questionnaire), the CORE-OM (Clinical Outcomes in Routine Evaluation), and the BASIS-24.

Functional measures assess real-world functioning. Examples include the WHODAS 2.0 and the Sheehan Disability Scale.

Session-by-session measures provide brief, frequent assessment. Examples include the ORS (Outcome Rating Scale) and the SRS (Session Rating Scale) used in feedback-informed treatment.

## Measurement-Based Care

The context supports measurement-based care (MBC), the systematic use of outcome data to inform clinical decision-making. In MBC, outcome measures are administered at regular intervals, results are compared against expected treatment response trajectories, and deviations trigger clinical alerts. Clients who are not progressing as expected may benefit from treatment plan modification, consultation, or referral.

The domain model supports expected treatment response (ETR) trajectories derived from normative outcome data. When a client's actual trajectory deviates significantly from the expected trajectory, the system raises a domain event that alerts the treating clinician.

## Domain Events

OutcomeMeasureAdministered: A standardized outcome measure has been completed by a client at a scheduled measurement point.

ReliableChangeDetected: A client's outcome scores demonstrate statistically reliable change between measurement points.

ClinicalSignificanceAchieved: A client's scores have crossed from the clinical range into the normative range.

TreatmentResponseAlert: A client's outcome trajectory has deviated significantly from the expected treatment response, warranting clinical review.

BenchmarkReportGenerated: An aggregate outcomes report has been produced comparing program-level outcomes against established benchmarks.

## Integration with Other Contexts

The Outcomes Measurement Context consumes session completion events from the Therapeutic Intervention Context to trigger measurement administration. It receives baseline assessment data from the Psychological Assessment Context. It shares statistical value objects with the Research Methods Context through a shared kernel. It receives behavioral data from the Behavioral Analysis Context for behavior-focused outcomes tracking.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media.
- Lambert, M. J. (2013). Bergin and Garfield's Handbook of Psychotherapy and Behavior Change (6th ed.). Wiley.
- Jacobson, N. S., & Truax, P. (1991). Clinical significance: A statistical approach. Journal of Consulting and Clinical Psychology, 59(1), 12-19.
