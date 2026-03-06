# Integration Patterns: Autism Domain

Integration patterns define the strategies and mechanisms by which bounded contexts in the autism spectrum management domain collaborate, share information, and maintain model integrity across boundaries.

## Purpose

The six bounded contexts in the autism domain must exchange information to support coordinated care without creating tight coupling that would prevent independent evolution. Integration patterns formalize how this exchange occurs, specifying the relationship type, data flow direction, and model translation responsibilities between each pair of interacting contexts.

## Customer-Supplier Pattern

In the customer-supplier pattern, the downstream context (customer) requests specific data formats from the upstream context (supplier), and the supplier accommodates reasonable requests.

### Diagnostic Assessment to Behavioral Intervention

The Behavioral Intervention Context is the customer, requesting diagnostic data in formats useful for intervention planning. The Diagnostic Assessment Context supplies diagnostic reports with structured sections on behavioral observations, developmental levels, and identified intervention priorities. The supplier accommodates requests for specific data fields (e.g., communication modality observed during assessment) that inform ABA program design.

### Behavioral Intervention to Educational Planning

The Educational Planning Context is the customer, requesting behavioral data in educationally relevant formats. The Behavioral Intervention Context supplies behavior plan summaries, skill acquisition progress reports, and behavioral trend data formatted for IEP progress documentation. The anti-corruption layer translates ABA-specific terminology into educational behavior support language.

### Sensory Processing to Educational Planning

The Educational Planning Context is the customer, requesting sensory data formatted for classroom accommodation planning. The Sensory Processing Context supplies environmental modification recommendations and sensory activity schedules adapted for school settings.

### Communication Support to Educational Planning

The Educational Planning Context is the customer, requesting communication data for IEP goal development. The Communication Support Context supplies speech-language assessment summaries, AAC usage data, and communication goal progress in formats aligned with IEP documentation requirements.

## Published Language Pattern

In the published language pattern, the interacting contexts agree on a shared data format that is independent of either context's internal model. This is particularly useful when multiple contexts consume the same data.

### Diagnostic Reports as Published Language

The Diagnostic Assessment Context publishes diagnostic reports in a standardized format that serves as a published language consumed by all downstream contexts. The format includes structured sections for diagnostic codes, severity levels, assessment instrument scores, clinical observations, and recommendations. Each consuming context extracts the information relevant to its bounded context through its own anti-corruption layer.

### Behavioral Data for Outcomes Tracking

The Behavioral Intervention Context publishes session-level behavioral data in a standardized outcomes format. This published language includes skill acquisition progress metrics, behavior frequency and trend data, and service utilization statistics in formats that the Outcomes Tracking Context can aggregate without understanding ABA-specific internal models.

### Educational Progress as Published Language

The Educational Planning Context publishes IEP goal progress data in a standardized format that both the intervention contexts and the Outcomes Tracking Context consume. This format includes goal descriptions, progress ratings, supporting data summaries, and timeline information.

## Shared Kernel Pattern

In the shared kernel pattern, two contexts share a small, explicitly defined subset of their models. Changes to the shared kernel require agreement from both teams.

### Behavioral Intervention and Sensory Processing Shared Kernel

These two contexts share a common model for antecedent-behavior-consequence (ABC) data. Sensory triggers frequently serve as antecedents to challenging behaviors, and sensory interventions often function as antecedent strategies in behavior intervention plans. The shared kernel includes the ABC data model, the antecedent classification taxonomy (which includes sensory antecedent categories), and the behavior observation recording format. Both contexts must agree on changes to these shared elements.

## Partnership Pattern

In the partnership pattern, two contexts have a mutual dependency and must coordinate their development.

### Behavioral Intervention and Communication Support Partnership

Functional communication training (FCT) bridges these two contexts. When a functional behavior assessment identifies that a challenging behavior serves a communication function, the Behavioral Intervention Context designs the behavioral contingencies while the Communication Support Context determines the appropriate communicative replacement. Both contexts must collaborate on FCT goals, teaching procedures, and generalization criteria. Changes in one context's FCT model directly affect the other.

## Open Host Service Pattern

In the open host service pattern, a context provides a standardized service interface that multiple consumers can access.

### Sensory Processing Open Host Service

The Sensory Processing Context provides sensory profile data through a standardized service interface that both the Communication Support Context (for AAC environment optimization) and the Educational Planning Context (for classroom accommodation planning) consume. The service provides sensory profile summaries, environmental modification recommendations, and sensory activity schedules in a consistent format regardless of the consumer.

## Conformist Pattern

In the conformist pattern, the downstream context adopts the upstream context's model without requesting changes.

### Outcomes Tracking as Conformist

The Outcomes Tracking Context conforms to the data models published by all upstream contexts. Rather than requesting custom formats, it accepts the published language formats from each context and maps them internally to its own outcome measurement model. This reduces upstream burden and allows the Outcomes Tracking Context to adapt to any published format changes without negotiation.

## Integration Anti-Patterns to Avoid

- Shared database: Bounded contexts must not share database tables or schemas.
- Direct model access: One context must not directly instantiate or manipulate another context's domain objects.
- Synchronous call chains: Long chains of synchronous calls between contexts create fragility and should be replaced with asynchronous event-driven patterns.

## References

- Evans, E. (2003). *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley. Chapter 14 on context mapping patterns.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley. Chapter 3 on integration patterns.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media. Chapters 6-7 on integration and boundary management.
- Hohpe, G., & Woolf, B. (2003). *Enterprise Integration Patterns*. Addison-Wesley.
