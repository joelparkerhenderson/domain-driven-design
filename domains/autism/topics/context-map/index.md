# Context Map: Autism Domain

The context map documents the relationships and integration patterns between the six bounded contexts in the autism spectrum management domain.

## Purpose

The autism domain involves multiple professional disciplines that must coordinate care for individuals on the autism spectrum. The context map makes explicit how each bounded context relates to the others, what integration patterns govern their interactions, and how information flows across boundaries. This enables teams to understand dependencies, plan integration work, and manage the complexity of cross-disciplinary coordination.

## Context Relationships

### Diagnostic Assessment Context (Upstream)

The Diagnostic Assessment Context is the primary upstream context. It produces diagnostic information that flows downstream to all other contexts:

- Diagnostic Assessment to Behavioral Intervention: Customer-Supplier relationship. The diagnostic report, including identified behavioral concerns and functional communication levels, informs the design of ABA intervention plans. The Behavioral Intervention Context is the customer that requests specific diagnostic data formats.

- Diagnostic Assessment to Sensory Processing: Customer-Supplier relationship. Sensory observations from the diagnostic evaluation feed into the Sensory Processing Context to guide initial sensory profile assessments.

- Diagnostic Assessment to Communication Support: Customer-Supplier relationship. Communication and language assessments from the diagnostic process inform AAC evaluations and speech-language therapy goal setting.

- Diagnostic Assessment to Educational Planning: Published Language relationship. Diagnostic reports are shared in a standardized format that educational teams use to develop IEPs and determine eligibility for special education services.

- Diagnostic Assessment to Outcomes Tracking: Conformist relationship. The Outcomes Tracking Context conforms to the diagnostic models to establish baseline measurements against which progress is tracked.

### Behavioral Intervention Context

- Behavioral Intervention to Sensory Processing: Shared Kernel relationship. Both contexts share a common model for antecedent-behavior-consequence (ABC) data, as sensory triggers are often antecedents to behaviors addressed in ABA therapy.

- Behavioral Intervention to Communication Support: Partnership relationship. Communication goals and behavioral goals are deeply interrelated, requiring close collaboration. Functional communication training (FCT) bridges both contexts.

- Behavioral Intervention to Educational Planning: Customer-Supplier relationship. The Educational Planning Context consumes behavior data and intervention strategies to inform classroom behavior support plans and IEP behavioral goals.

- Behavioral Intervention to Outcomes Tracking: Published Language relationship. Session-level behavioral data is published in standardized formats for outcome measurement and progress monitoring.

### Sensory Processing Context

- Sensory Processing to Communication Support: Open Host Service relationship. The Sensory Processing Context provides sensory profile data through a standardized service that the Communication Support Context uses to optimize AAC device settings and communication environments.

- Sensory Processing to Educational Planning: Customer-Supplier relationship. Environmental modification recommendations flow to the Educational Planning Context to inform classroom accommodation plans.

### Communication Support Context

- Communication Support to Educational Planning: Customer-Supplier relationship. Speech-language therapy goals and AAC usage data inform IEP communication objectives and classroom communication support strategies.

- Communication Support to Outcomes Tracking: Published Language relationship. Communication progress data is published for longitudinal outcome analysis.

### Educational Planning Context

- Educational Planning to Outcomes Tracking: Published Language relationship. IEP goal progress data is published in standardized formats for tracking educational outcomes alongside clinical outcomes.

### Outcomes Tracking Context (Downstream)

The Outcomes Tracking Context is the primary downstream consumer, receiving data from all other contexts through published language interfaces. It does not impose upstream requirements but must conform to the data formats published by each upstream context.

## Anti-Corruption Layers

Anti-corruption layers are positioned at key integration points where models differ significantly:

- Between Diagnostic Assessment and Educational Planning: Clinical diagnostic terminology (DSM-5 codes, severity levels) is translated into educational eligibility categories and service delivery language.

- Between Behavioral Intervention and Educational Planning: ABA-specific terminology (discrete trial data, reinforcement schedules) is translated into educationally relevant behavior support language.

- Between external healthcare systems and the autism domain: External EHR data, insurance claims, and referral information are translated at the domain boundary to protect internal model integrity.

## References

- Evans, E. (2003). *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley. Chapter 14 on context maps.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley. Chapter 3 on context mapping.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media. Chapter 6 on context mapping patterns.
