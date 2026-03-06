# Bounded Contexts in the Psychology Domain

## Overview

A bounded context is a logical boundary within which a specific domain model and its ubiquitous language are consistent and unambiguous. In the psychology domain, six bounded contexts reflect the natural professional and functional divisions of psychology practice: Psychological Assessment, Therapeutic Intervention, Behavioral Analysis, Cognitive Assessment, Research Methods, and Outcomes Measurement.

Each bounded context encapsulates its own aggregates, entities, value objects, domain events, repositories, and domain services. Terms that appear in multiple contexts carry context-specific meanings. For example, "baseline" in the Behavioral Analysis Context refers to the rate of a target behavior before intervention, while in the Outcomes Measurement Context it refers to initial scores on standardized measures before treatment begins.

## Psychological Assessment Context

This context encompasses the lifecycle of formal psychological evaluation. It begins with a referral, proceeds through test selection, administration, scoring, and interpretation, and culminates in a clinical report. The primary aggregate is the Assessment, which coordinates test sessions, raw scores, scaled scores, normative comparisons, and interpretive narratives. Key value objects include Test Score, Normative Percentile, Confidence Interval, and Reliability Coefficient. The language of this context draws heavily from psychometric theory and standardized test manuals.

## Therapeutic Intervention Context

This context models the delivery of psychological treatment. It manages treatment plans, therapy sessions, therapeutic techniques, homework assignments, and progress tracking. The primary aggregate is the Treatment Plan, which coordinates the sequencing of evidence-based interventions such as CBT, ACT, and EMDR. Key entities include Session, Therapist, and Client within this context. Value objects capture Diagnosis, Treatment Goal, and Session Note content. The ubiquitous language reflects clinical practice guidelines and therapy manuals.

## Behavioral Analysis Context

This context applies the principles of Applied Behavior Analysis to assess and modify behavior. It manages functional behavior assessments, behavior intervention plans, data collection protocols, and reinforcement schedules. The primary aggregate is the Behavior Plan, which coordinates target behaviors, replacement behaviors, antecedent strategies, and consequence-based interventions. Key value objects include Antecedent-Behavior-Consequence Record, Reinforcement Schedule, and Behavior Rate. The language derives from behavioral science and ABA professional standards.

## Cognitive Assessment Context

This context handles the specialized evaluation of cognitive functioning. It manages cognitive screening, executive function testing, memory assessment, attention testing, and the construction of cognitive profiles. The primary aggregate is the Cognitive Evaluation, which integrates scores from multiple cognitive domains into a coherent profile. Value objects include Domain Score, Cognitive Index, and Normative Comparison. This context uses language specific to neuropsychology and cognitive science.

## Research Methods Context

This context governs the design, execution, and analysis of psychological research. It manages experimental designs, participant recruitment, IRB protocols, data collection, statistical analyses, and publication workflows. The primary aggregate is the Research Study, which coordinates all aspects of a research project from proposal through publication. Key entities include Participant, Dataset, and Manuscript. Value objects capture Effect Size, Statistical Test Result, and P-Value. The language follows research methodology and APA publication standards.

## Outcomes Measurement Context

This context tracks the effectiveness of psychological services. It manages standardized outcome measures, patient-reported outcomes, clinician-rated scales, and benchmarking data. The primary aggregate is the Outcome Record, which tracks an individual's scores across multiple measures over time. Value objects include Outcome Score, Reliable Change Index, and Clinical Significance Threshold. The language reflects outcomes research and quality improvement methodology.

## Context Boundary Enforcement

Each context enforces its boundary by controlling how external concepts enter and leave. When the Therapeutic Intervention Context needs assessment results, it does not directly access the Psychological Assessment Context's model. Instead, it receives a translated summary through an anti-corruption layer or consumes a domain event that carries only the information relevant to treatment planning.

## Why Six Contexts

The decision to separate these six contexts reflects genuine differences in professional practice. Assessment psychologists, clinical therapists, behavior analysts, neuropsychologists, research psychologists, and outcomes specialists each bring distinct training, vocabulary, and operational concerns. Attempting to unify these into a single model would create an ambiguous, overly complex structure that fails to serve any stakeholder well.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley. Chapter 14 on bounded contexts.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley. Chapter 2 on bounded contexts in practice.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media. Chapter 4 on defining bounded context boundaries.
- American Psychological Association. (2017). Ethical Principles of Psychologists and Code of Conduct.
