# Aggregates in the Psychology Domain

## Overview

An aggregate is a cluster of related domain objects that is treated as a single unit for the purpose of data changes and consistency enforcement. Each aggregate has a root entity that serves as the sole entry point for external access. In the psychology domain, aggregates ensure that complex clinical processes like psychological assessments, treatment plans, and behavior intervention plans maintain internal consistency as they evolve through their lifecycles.

Aggregates define transactional boundaries. All modifications to objects within an aggregate pass through the aggregate root, which enforces business invariants. This pattern is particularly important in psychology, where clinical data integrity has direct implications for patient safety and regulatory compliance.

## Assessment Aggregate

The Assessment aggregate in the Psychological Assessment Context is rooted in the Assessment entity. It contains Test Session entities, Test Score value objects, Normative Comparison value objects, and Interpretive Narrative value objects. The Assessment root enforces invariants such as: all tests in a battery must be administered before scoring is finalized; validity indicators must be evaluated before scores are interpreted; and a diagnostic formulation must reference at least one scored test.

The Assessment aggregate manages the lifecycle from referral through completion. An assessment cannot be marked as complete until all required tests have been administered and scored, all validity checks have passed, and an interpretive report has been generated. These invariants protect the clinical integrity of the assessment process.

## Treatment Plan Aggregate

The Treatment Plan aggregate in the Therapeutic Intervention Context is rooted in the Treatment Plan entity. It contains Treatment Goal entities, Intervention entities, Session entities, and Progress Note value objects. The Treatment Plan root enforces invariants such as: every treatment goal must be linked to at least one intervention; session notes must reference the goals addressed; and treatment plan reviews must occur at specified intervals.

The Treatment Plan aggregate ensures that therapeutic activities remain organized around explicit goals and evidence-based interventions. When a new goal is added or an intervention is modified, the aggregate root validates that the treatment plan remains internally consistent and clinically sound.

## Behavior Plan Aggregate

The Behavior Plan aggregate in the Behavioral Analysis Context is rooted in the Behavior Plan entity. It contains Target Behavior entities, Replacement Behavior entities, Antecedent Strategy value objects, Consequence Strategy value objects, and Data Collection Protocol entities. The aggregate root enforces invariants such as: every target behavior must have at least one replacement behavior; reinforcement strategies must specify schedules; and data collection must begin before intervention implementation.

This aggregate maintains the relationship between assessment findings and intervention strategies. A behavior plan cannot be activated without a completed functional behavior assessment, and modifications to target behaviors trigger a re-evaluation of the intervention strategy.

## Cognitive Evaluation Aggregate

The Cognitive Evaluation aggregate in the Cognitive Assessment Context is rooted in the Cognitive Evaluation entity. It contains Domain Assessment entities (one per cognitive domain tested), Cognitive Score value objects, and Cognitive Profile value objects. The aggregate root enforces invariants such as: a cognitive profile cannot be generated until all selected domains have been assessed; normative comparisons must use age-appropriate norms; and impairment classifications must follow established cut-score criteria.

## Research Study Aggregate

The Research Study aggregate in the Research Methods Context is rooted in the Research Study entity. It contains Study Design value objects, Participant Enrollment entities, Data Collection entities, and Analysis Plan entities. The aggregate root enforces invariants such as: data collection cannot begin until IRB approval is obtained; participant enrollment must verify informed consent; and analysis procedures must match the pre-registered analysis plan.

## Outcome Record Aggregate

The Outcome Record aggregate in the Outcomes Measurement Context is rooted in the Outcome Record entity. It contains Measurement Point entities, Outcome Score value objects, and Change Index value objects. The aggregate root enforces invariants such as: reliable change calculations require at least two measurement points; clinical significance determinations must reference appropriate normative data; and benchmarking comparisons must use matching population norms.

## Aggregate Design Principles

Psychology domain aggregates should be designed to be as small as practical while maintaining consistency. A single therapy session does not need to load the entire treatment history. The aggregate boundary should encompass only what is needed to enforce the invariant being protected.

References between aggregates use identity references rather than direct object references. A treatment plan references an assessment by its identifier, not by containing the full assessment object. This keeps aggregates loosely coupled while maintaining the ability to navigate between related clinical concepts.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley. Chapter 6 on aggregates.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley. Chapter 10 on aggregate design.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media. Chapter 7 on modeling aggregates.
