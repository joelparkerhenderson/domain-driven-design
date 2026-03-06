# Psychological Assessment Context

## Overview

The Psychological Assessment Context is the bounded context responsible for the full lifecycle of formal psychological evaluation. It encompasses referral management, test selection, test administration, scoring, interpretation, and report generation. This context captures the specialized knowledge of psychometric theory, standardized testing protocols, and clinical interpretation that distinguishes professional psychological assessment from informal evaluation.

Psychological assessment is one of the defining competencies of the psychology profession. The domain model must capture the rigor, standardization, and clinical judgment that characterize this practice area. Assessment results inform clinical decision-making across other bounded contexts, making this context a critical upstream producer of clinical data.

## Core Aggregate: Assessment

The Assessment aggregate is the primary unit of consistency in this context. It is rooted in the Assessment entity and contains Test Session entities, Test Score value objects, Validity Indicator value objects, Normative Comparison value objects, and the Interpretive Report value object.

The Assessment aggregate enforces several critical invariants. All tests in a selected battery must be administered before the assessment can proceed to scoring. Validity indicators must be evaluated before scores are interpreted. Standardized administration procedures must be documented. The interpretive report must reference scored tests and provide a diagnostic formulation grounded in the data.

## Key Entities

Assessment: The root entity representing the complete evaluation. It carries a unique identifier, the referral information, the client reference, the administering psychologist, the selected test battery, the assessment status, and the completion date.

Test Session: An entity representing a single administration session within the assessment. A comprehensive assessment may require multiple sessions across different days. Each session records the date, duration, tests administered, behavioral observations, and any deviations from standard administration.

Assessment Instrument: An entity representing a specific psychometric test with its properties, norms, and scoring rules. Instruments are managed independently and referenced by assessments.

## Key Value Objects

Test Score: An immutable score including raw score, scaled score, percentile rank, confidence interval, and classification.

Normative Comparison: The relationship between an individual score and a reference population, including norm group, percentile, and descriptive classification.

Validity Indicator: A measure of whether the assessment results are interpretable, including effort indicators, response style measures, and inconsistency indices.

Referral Question: The specific clinical question the assessment is designed to answer, such as differential diagnosis, cognitive capacity evaluation, or treatment planning.

Psychometric Property: Reliability coefficients, validity evidence, and standard error of measurement for an assessment instrument.

## Domain Events

AssessmentReferralReceived: A new referral for psychological evaluation has been accepted.

TestBatterySelected: The assessing psychologist has chosen the specific tests to administer based on the referral question and client characteristics.

TestBatteryAdministered: All tests in the battery have been administered across one or more sessions.

ScoringCompleted: All tests have been scored and validity indicators have been evaluated.

AssessmentCompleted: The full assessment cycle is finished, including interpretation and report generation.

ValidityThreatDetected: Validity indicators suggest results may not accurately reflect the client's true functioning.

## Assessment Types

The context supports multiple assessment types, each with its own test selection logic and interpretive framework. Personality assessment uses instruments like the MMPI-3 and PAI. Intelligence testing uses instruments like the WAIS-IV and WISC-V. Neuropsychological assessment uses flexible or fixed batteries covering multiple cognitive domains. Diagnostic assessment focuses on differential diagnosis using structured interviews and symptom measures.

## Scoring and Interpretation

Scoring in this context is handled by the Scoring Service, a domain service that applies instrument-specific algorithms. Interpretation is a clinical judgment process that integrates scores across instruments, considers validity indicators, accounts for contextual factors, and produces a coherent diagnostic formulation. The domain model supports this process but does not automate clinical judgment.

## Integration with Other Contexts

Assessment results flow to the Therapeutic Intervention Context through an anti-corruption layer that produces treatment-relevant summaries. Assessment results flow to the Outcomes Measurement Context as baseline measures. Assessment instruments are shared with the Research Methods Context when studies use standardized measures.

## Regulatory Considerations

Psychological assessment is governed by APA ethical guidelines, state licensure laws, and test publisher usage agreements. Only qualified professionals may administer, score, and interpret certain tests. Test security must be maintained -- test content is proprietary and must not be disclosed. The domain model must enforce these constraints without embedding specific regulatory rules that vary by jurisdiction.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media.
- Groth-Marnat, G., & Wright, A. J. (2016). Handbook of Psychological Assessment (6th ed.). Wiley.
- American Psychological Association. (2020). APA Guidelines for Psychological Assessment and Evaluation.
