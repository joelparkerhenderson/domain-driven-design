# Cognitive Assessment Context

## Overview

The Cognitive Assessment Context is the bounded context responsible for the specialized evaluation of cognitive functioning. It encompasses cognitive screening, comprehensive neuropsychological evaluation, executive function testing, memory assessment, attention testing, and the construction of cognitive profiles. This context applies the science of neuropsychology and cognitive psychology to characterize brain-behavior relationships and inform clinical decision-making.

Cognitive assessment differs from general psychological assessment in its focus on specific cognitive domains and its interpretation within the framework of brain-behavior relationships. While the Psychological Assessment Context handles broad evaluations including personality and psychopathology, this context provides the deep, domain-specific cognitive analysis that informs diagnostic questions about neurological conditions, cognitive decline, learning disabilities, and traumatic brain injury.

## Core Aggregate: Cognitive Evaluation

The Cognitive Evaluation aggregate is the primary unit of consistency. It is rooted in the Cognitive Evaluation entity and contains Domain Assessment entities (one per cognitive domain evaluated), Cognitive Score value objects, and the Cognitive Profile value object.

The Cognitive Evaluation aggregate enforces invariants including: a cognitive profile cannot be generated until all selected domains have been assessed; normative comparisons must use age-appropriate norms (and education-corrected norms where available); impairment classifications must follow established cut-score criteria; and the evaluation must document any factors that may have affected test performance.

## Key Entities

Cognitive Evaluation: The root entity representing the complete cognitive assessment. It carries the client reference, the evaluating neuropsychologist, the referral question, the domains assessed, the evaluation status, and the completion date.

Domain Assessment: An entity representing the evaluation of a specific cognitive domain. Each domain assessment includes the tests administered, the scores obtained, the normative comparisons, and the domain-specific interpretation. Cognitive domains include attention, processing speed, language, visual-spatial processing, learning and memory, and executive function.

## Key Value Objects

Cognitive Score: An immutable score within a specific cognitive domain, including the test name, raw score, standard score, percentile rank, and classification.

Cognitive Index: A composite score aggregating performance across tests within a cognitive domain, with associated confidence interval and normative comparison.

Cognitive Profile: An integrated summary of the individual's cognitive strengths and weaknesses across all assessed domains, including pattern analysis and diagnostic implications.

Normative Comparison: The relationship between the individual's score and age-appropriate (and optionally education- or demographically-corrected) reference data.

Performance Validity Indicator: A measure assessing whether the individual put forth adequate effort during testing, essential for valid interpretation of cognitive test results.

## Cognitive Domains

The context models cognitive functioning across established neuropsychological domains.

Attention and Concentration: Sustained attention, selective attention, divided attention, and processing speed. Assessed through measures like the Continuous Performance Test, Trail Making Test Part A, and digit span tasks.

Executive Function: Planning, cognitive flexibility, inhibition, working memory, and problem-solving. Assessed through measures like the Wisconsin Card Sorting Test, Trail Making Test Part B, Stroop Test, and Tower tests.

Memory: Verbal and visual learning, immediate and delayed recall, recognition memory, and prospective memory. Assessed through measures like the California Verbal Learning Test, Rey Complex Figure recall, and logical memory subtests.

Language: Naming, fluency, comprehension, and repetition. Assessed through measures like the Boston Naming Test, verbal fluency tasks, and the Token Test.

Visual-Spatial Processing: Visual construction, visual perception, and spatial reasoning. Assessed through measures like the Rey Complex Figure copy, Block Design, and Judgment of Line Orientation.

## Screening vs. Comprehensive Evaluation

The context distinguishes between cognitive screening and comprehensive neuropsychological evaluation. Screening uses brief instruments like the Montreal Cognitive Assessment (MoCA) or the Mini-Mental State Examination (MMSE) to detect possible impairment. When screening results fall below established cut-scores, a CognitiveScreeningCompleted event is raised that may trigger a referral for comprehensive evaluation.

Comprehensive evaluation involves a full battery of tests across multiple cognitive domains, producing a detailed cognitive profile. The selection of specific tests is guided by the referral question, the client's demographic characteristics, and clinical hypotheses about the nature of cognitive dysfunction.

## Domain Events

CognitiveScreeningCompleted: A cognitive screening has been administered and scored, with results indicating whether further evaluation is warranted.

CognitiveEvaluationStarted: A comprehensive cognitive evaluation has been initiated with domain selection and test planning completed.

DomainAssessmentCompleted: Testing within a specific cognitive domain has been completed and scored.

CognitiveProfileGenerated: All selected domains have been assessed and an integrated cognitive profile has been produced.

CognitiveDeclineDetected: Comparison with prior evaluation results indicates significant decline in one or more cognitive domains.

## Integration with Other Contexts

The Cognitive Assessment Context partners with the Psychological Assessment Context, receiving referrals and returning cognitive profiles for integration into comprehensive assessment reports. It provides cognitive profile information to the Therapeutic Intervention Context to inform treatment adaptation. It supplies cognitive measures to the Research Methods Context when studies include cognitive outcomes.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media.
- Lezak, M. D., Howieson, D. B., Bigler, E. D., & Tranel, D. (2012). Neuropsychological Assessment (5th ed.). Oxford University Press.
- Strauss, E., Sherman, E. M. S., & Spreen, O. (2006). A Compendium of Neuropsychological Tests (3rd ed.). Oxford University Press.
