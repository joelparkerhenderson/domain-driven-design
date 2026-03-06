# Value Objects in the Psychology Domain

## Overview

A value object is an immutable domain object that is defined entirely by its attributes rather than by a unique identity. Two value objects with identical attributes are considered equal and interchangeable. In the psychology domain, value objects represent the measurements, scores, clinical concepts, and standardized constructs that form the quantitative and qualitative backbone of psychological practice.

Value objects are immutable: once created, their state does not change. If a different value is needed, a new value object is created. This immutability guarantees consistency and simplifies reasoning about domain logic. Psychology is rich in value objects because clinical measurement, standardized scoring, and normative comparison all produce discrete, immutable data points.

## Test Score

The Test Score value object encapsulates a scored result from a psychometric instrument. It includes the raw score, the scaled score, the score type (standard score, T-score, percentile rank, z-score), and the associated confidence interval. Two Test Scores are equal if all their attributes match. Test Scores are immutable because a scored result does not change after scoring is complete. If a scoring error is discovered, a new Test Score is created with corrected values and the original is retained for audit purposes.

## Normative Comparison

The Normative Comparison value object represents how an individual's score relates to a reference population. It includes the normative group identifier, the percentile rank, the descriptive classification (e.g., Average, Below Average, Superior), and the confidence band. This value object enables clinicians to interpret raw scores in the context of population expectations.

## Diagnosis

The Diagnosis value object captures a clinical diagnostic impression. It includes the diagnostic code (DSM-5 or ICD-10), the diagnostic label, the severity specifier, and any applicable course specifiers. Diagnosis is modeled as a value object rather than an entity because the diagnosis itself has no identity independent of its attributes. A Diagnosis of Major Depressive Disorder, Single Episode, Moderate is the same value regardless of which client holds it.

## Treatment Goal

The Treatment Goal value object defines a specific, measurable objective within a treatment plan. It includes the goal statement, the target behavior or symptom, the measurement criteria, the expected timeline, and the current status (not started, in progress, achieved, revised). When a goal's status changes, a new Treatment Goal value object replaces the previous one within the Treatment Plan aggregate.

## Session Note Content

The Session Note Content value object captures the clinical documentation of a therapy session. It includes the session summary, interventions used, client response, risk assessment findings, and plan for next session. As a value object, session note content is immutable once finalized. Amendments are recorded as separate addendum value objects rather than modifications to the original.

## Reinforcement Schedule

The Reinforcement Schedule value object in the Behavioral Analysis Context specifies the contingency arrangement for delivering reinforcement. It includes the schedule type (fixed ratio, variable ratio, fixed interval, variable interval), the schedule parameters, and the target response. This value object governs how reinforcement is applied within a behavior intervention plan.

## Effect Size

The Effect Size value object in the Research Methods Context quantifies the magnitude of a research finding. It includes the effect size metric (Cohen's d, eta-squared, odds ratio), the calculated value, and the confidence interval. Effect sizes are immutable products of statistical calculation. Two Effect Size value objects with identical attributes represent the same finding regardless of which study produced them.

## Reliable Change Index

The Reliable Change Index value object in the Outcomes Measurement Context determines whether the change in a client's scores between two measurement points exceeds what would be expected from measurement error alone. It includes the pre-score, the post-score, the standard error of measurement, the calculated RCI value, and the determination (reliable improvement, no reliable change, reliable deterioration).

## Cognitive Index Score

The Cognitive Index Score value object in the Cognitive Assessment Context represents a composite score across tests within a cognitive domain. It includes the domain name, the composite score, the percentile rank, the confidence interval, and the classification level. This value object summarizes performance within a specific area of cognitive functioning.

## Consent Record

The Consent Record value object captures a client's or participant's informed consent. It includes the consent type (treatment, assessment, research), the date obtained, the information provided, the consenting party, and any limitations or conditions. Consent records are immutable snapshots of the consent status at a point in time.

## Design Principles for Value Objects

Value objects in the psychology domain should be self-validating. A Test Score value object should reject invalid score ranges. A Diagnosis value object should validate that the diagnostic code matches the diagnostic label. This validation at the value object level prevents invalid clinical data from propagating through the system.

Value objects should be composed from other value objects where appropriate. A Normative Comparison contains a Test Score. An Effect Size contains a Confidence Interval. This composition creates a rich, expressive domain model that mirrors the layered nature of psychological measurement.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley. Chapter 5 on value objects.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley. Chapter 6 on value object design.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media. Chapter 6 on modeling with value objects.
