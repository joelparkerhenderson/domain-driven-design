# Anti-Corruption Layer in the Psychology Domain

## Overview

An anti-corruption layer (ACL) is a translation boundary that protects a bounded context from the concepts, models, and language of external systems or other bounded contexts. In the psychology domain, anti-corruption layers are essential because each bounded context uses specialized terminology that would corrupt the clarity of another context if imported directly.

The ACL pattern prevents the leaking of one context's model into another. Without it, the Therapeutic Intervention Context might absorb raw psychometric data structures from the Psychological Assessment Context, creating tight coupling and model pollution. The ACL translates external concepts into terms that are meaningful within the consuming context.

## ACL Between Assessment and Therapeutic Intervention

When assessment results flow to the Therapeutic Intervention Context, the anti-corruption layer translates detailed psychometric data into clinically actionable information. The Assessment Context produces rich data including raw scores, scaled scores, T-scores, percentile ranks, confidence intervals, validity indicators, and normative comparisons across multiple test instruments. The Therapeutic Intervention Context does not need this level of psychometric detail.

The ACL translates this into a Treatment-Relevant Assessment Summary that includes diagnostic impressions, functional strengths and limitations, treatment recommendations, and risk factors. The therapist's model operates on clinical concepts like treatment goals and intervention targets, not on psychometric constructs like factor loadings and internal consistency coefficients.

## ACL Between Behavioral Analysis and Therapeutic Intervention

The Behavioral Analysis Context uses highly specific language rooted in operant conditioning theory. Terms like discriminative stimulus, establishing operation, differential reinforcement, and response class have precise technical meanings. When behavior analysis findings inform therapeutic intervention, the ACL translates these into treatment-relevant concepts.

A functional behavior assessment result expressed in terms of antecedent-behavior-consequence contingencies is translated into a clinically meaningful behavior pattern description. Reinforcement schedules are translated into therapeutic strategy recommendations. This translation preserves the clinical intent while allowing each context to maintain its own conceptual integrity.

## ACL Between Research Methods and Clinical Contexts

Research data and clinical data serve fundamentally different purposes. Research data must be de-identified, coded according to research protocols, and analyzed at the aggregate level. Clinical data is identified, organized around individual clients, and interpreted at the individual level.

The ACL between Research Methods and clinical contexts handles the translation of clinical observations into research variables and the reverse translation of research findings into clinical guidelines. When a research study uses clinical assessment data, the ACL strips identifying information, maps clinical constructs to research variables, and applies inclusion and exclusion criteria. When research findings inform clinical practice, the ACL translates statistical effect sizes and group-level findings into individual-level clinical recommendations.

## ACL with External Systems

Psychology practice systems frequently integrate with external systems that have their own models and terminology. Electronic Health Record (EHR) systems use medical terminology and coding systems (ICD-10, CPT) that do not map directly to psychological domain concepts. Insurance systems use authorization and claims language that differs from clinical language.

Anti-corruption layers at these boundaries translate between the psychology domain model and external system models. A diagnosis in the psychology domain is a rich clinical concept with supporting evidence, differential considerations, and prognostic implications. The ACL translates this into an ICD-10 code for the EHR and a diagnosis code with medical necessity justification for the insurance system.

## ACL Between Cognitive Assessment and Psychological Assessment

Although these two assessment contexts are closely related, they use subtly different models. The Cognitive Assessment Context organizes results by cognitive domain (attention, memory, executive function, processing speed), while the Psychological Assessment Context may organize results by test instrument. The ACL ensures that when cognitive results are incorporated into a comprehensive psychological assessment report, the translation preserves the neuropsychological interpretation while fitting the broader assessment report structure.

## Implementation Principles

Anti-corruption layers in the psychology domain should be implemented as explicit translation services rather than ad hoc data mappings. Each ACL should define a clear contract specifying what information crosses the boundary and how it is transformed. The ACL should be owned by the consuming context, because the consuming context best understands what information it needs and how that information should be represented in its own model.

Changes to the upstream context's model should not require changes to the downstream context's internal model. The ACL absorbs the impact of upstream changes by adjusting the translation logic while keeping the downstream model stable.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley. Chapter 14 on anti-corruption layers.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley. Chapter 3 on context mapping and ACL.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media. Chapter 5 on protecting bounded context boundaries.
- Fowler, M. (2002). Patterns of Enterprise Application Architecture. Addison-Wesley. On gateway and adapter patterns.
