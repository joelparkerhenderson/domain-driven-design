# Integration Patterns - Kinesiology Domain

## Overview

Integration patterns define how bounded contexts relate to and exchange information with each other. Domain-Driven Design identifies several standard patterns for inter-context relationships, each with different implications for coupling, autonomy, and coordination. The kinesiology domain uses a combination of these patterns to balance the need for data flow between clinical contexts with the need to protect each context's model integrity.

Choosing the right integration pattern for each context relationship is a strategic design decision. The pattern determines how much influence one context has over another, how changes in one context propagate to others, and how much coordination is required between the teams responsible for each context.

## Customer-Supplier Pattern

In the customer-supplier pattern, the downstream context (customer) depends on the upstream context (supplier), and the upstream context accommodates the downstream context's needs. The upstream context commits to providing data in a format useful to the downstream context and to communicating changes in advance.

### Movement Assessment to Exercise Prescription

Movement Assessment is the supplier, and Exercise Prescription is the customer. The assessment context publishes AssessmentCompleted events with summary data structured to be useful for program design. The exercise prescription team can request that the assessment context include specific data elements, such as estimated one-repetition maximum values or flexibility measurements, in its published events. The assessment team commits to maintaining the agreed-upon data contract.

### Movement Assessment to Rehabilitation Planning

Movement Assessment supplies reassessment data that Rehabilitation Planning uses to track recovery progress. The rehabilitation team may request assessment data in specific formats that facilitate milestone evaluation, such as limb symmetry indices or functional test scores expressed as percentages of pre-injury baselines.

### Exercise Prescription to Injury Prevention

Exercise Prescription supplies training load data that Injury Prevention uses for workload monitoring. The injury prevention team requires session-level workload data in a format compatible with ACWR calculations, and the prescription team commits to publishing TrainingSessionCompleted events with the necessary load metrics.

## Published Language Pattern

In the published language pattern, two or more contexts agree on a shared data format for exchanging information. This format is documented, versioned, and maintained independently of any single context. It serves as a contract that both contexts can rely on.

### Movement Assessment to Injury Prevention

Assessment screening results are published using a standardized data contract that defines the format for movement quality scores, asymmetry indices, and functional test results. This published language allows Injury Prevention to reliably interpret screening data regardless of which specific assessment battery was used.

### Performance Analysis to Injury Prevention

Performance metrics are published using a data contract that defines the format for force measurements, power outputs, and fatigue indicators. This ensures consistent interpretation of quantitative performance data across contexts.

## Conformist Pattern

In the conformist pattern, the downstream context adopts the model of the upstream context without modification. The downstream context has no influence over the upstream context's model and simply conforms to whatever the upstream provides.

### Performance Analysis to Movement Assessment

Movement Assessment adopts the data formats and conventions of Performance Analysis without requesting changes. This is appropriate because instrumented measurement data (force plate readings, motion capture coordinates) has inherent precision requirements defined by measurement science. The assessment context conforms to these standards rather than requesting that performance analysis translate its data into a different format.

## Shared Kernel Pattern

In the shared kernel pattern, two contexts agree to share a small, carefully controlled subset of their models. Changes to the shared kernel require agreement from both teams, and the shared code is jointly owned and versioned.

### Exercise Prescription and Rehabilitation Planning

These two contexts share a small kernel of exercise-related concepts: exercise specifications, dosage parameters (sets, repetitions, load, rest), and progression rules. This shared kernel is appropriate because these concepts are genuinely identical in both contexts. An exercise specification for a squat is the same whether it appears in a general training program or a rehabilitation protocol.

The shared kernel is strictly limited. Concepts specific to general training (periodization phases, training peaks) or specific to rehabilitation (healing phase constraints, clinical precautions) remain within their respective contexts. Only the truly shared exercise specification concepts reside in the kernel.

## Open Host Service Pattern

In the open host service pattern, a context provides a well-defined service interface that multiple other contexts can consume. The service protocol is designed for general use rather than tailored to any single consumer.

### Research and Education as Open Host

The Research and Education Context exposes its knowledge base through an open host service. Any bounded context can query for clinical practice guidelines, evidence summaries, or recommended protocols through a standardized interface. The service protocol is designed to support diverse queries without being tailored to any specific consumer's needs.

Each consuming context applies its own anti-corruption layer to translate the generic research output into context-specific recommendations. The Movement Assessment Context translates evidence into assessment protocol recommendations. The Exercise Prescription Context translates evidence into programming guidelines. The Rehabilitation Planning Context translates evidence into recovery protocol recommendations.

## Anti-Corruption Layer Pattern

The anti-corruption layer pattern is used when a context must integrate with an external system whose model is significantly different from the context's internal model. The ACL translates between the two models, preventing the external model from corrupting the internal one. Anti-corruption layers in the kinesiology domain are documented in detail in the Anti-Corruption Layer topic.

## Separate Ways Pattern

In the separate ways pattern, two contexts choose not to integrate at all. They operate independently, and any needed coordination happens outside the system, typically through manual processes.

No context pairs in the kinesiology domain currently follow the separate ways pattern, as all contexts benefit from event-driven communication. However, this pattern could apply if a particular deployment chose not to implement certain bounded contexts.

## Pattern Selection Criteria

Choose customer-supplier when the downstream context has legitimate influence over what the upstream provides. Choose published language when multiple contexts need to agree on a stable interchange format. Choose conformist when the upstream context's model is authoritative and should not be modified. Choose shared kernel only when concepts are genuinely identical across contexts and joint ownership is feasible. Choose open host service when a context serves many diverse consumers.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 14 on integration patterns.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 3 on context integration.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 5 on integration strategies.
