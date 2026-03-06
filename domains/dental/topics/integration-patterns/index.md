# Integration Patterns - Dental Domain

## Overview

Integration patterns define how bounded contexts in the dental domain communicate and share information across their boundaries. These patterns, drawn from Eric Evans's context mapping taxonomy, describe the nature of the relationship between contexts and the mechanisms used to exchange data. The dental domain employs multiple integration patterns selected to match the specific relationship dynamics between each pair of interacting contexts.

## Pattern Catalog for the Dental Domain

### Published Language

Published Language defines a well-documented, shared data format that multiple contexts use for communication. In the dental domain, the primary published language is built on standardized dental terminology and coding systems.

Application in the dental domain:
- CDT procedure codes serve as a published language for procedure identification across all contexts. When the Clinical Examination Context identifies a needed procedure and the Treatment Planning Context creates a plan item, both reference the same CDT code with consistent meaning.
- Tooth numbering (universal system 1-32, A-T) provides a published language for referencing specific teeth across all clinical contexts.
- The ADA periodontal disease staging and grading system provides a published language for communicating periodontal status between the Clinical Examination, Periodontal Care, and Treatment Planning contexts.

Published language reduces translation overhead because all contexts agree on the shared terminology. However, each context may carry additional context-specific attributes beyond the shared language.

### Shared Kernel

Shared Kernel designates a small, explicitly agreed-upon subset of the domain model that two or more contexts share. Changes to the shared kernel require agreement from all sharing contexts.

Application in the dental domain:
- The Clinical Examination Context and Periodontal Care Context share a kernel for periodontal probing measurements. Both contexts use the same model for six-site-per-tooth probing depths measured in millimeters with bleeding on probing indicators. This shared kernel ensures measurement consistency between initial examination probing and longitudinal periodontal tracking.

The shared kernel is kept deliberately small to minimize coupling. Only the probing measurement model is shared, not the disease classification logic or treatment protocols, which remain within the Periodontal Care Context.

### Conformist

The Conformist pattern applies when a downstream context accepts the upstream context's model without modification. The downstream team has no influence over the upstream model and simply conforms to it.

Application in the dental domain:
- The Restorative Services Context conforms to the Treatment Planning Context's treatment item model. When a treatment plan is approved, the Restorative Services Context accepts the treatment item definitions (CDT code, tooth number, surfaces, and sequencing) as provided, without requesting modifications to the treatment plan model.

This pattern works well here because the Treatment Planning Context's output is a well-defined clinical specification that the Restorative Services Context can execute without reinterpretation.

### Customer-Supplier

The Customer-Supplier pattern describes a relationship where the downstream context (customer) can negotiate with the upstream context (supplier) about the data and format provided. The supplier accommodates reasonable customer requests.

Application in the dental domain:
- The Treatment Planning Context (customer) requests diagnostic data from the Clinical Examination Context (supplier). The Treatment Planning Context can request that examination findings include specific detail levels or categorizations that support treatment plan creation. The Clinical Examination Context accommodates these requests within its model capabilities.
- The Practice Management Context (customer) requests procedure completion data from clinical contexts (suppliers) in formats that support billing and claims processing.

### Open Host Service

Open Host Service provides a well-defined protocol or API that any context can use to access the service. The service context publishes its capabilities through a stable interface.

Application in the dental domain:
- The Practice Management Context operates as an open host service, providing scheduling, insurance verification, and billing capabilities to all clinical contexts. Any clinical context can check appointment availability, request insurance verification, or submit procedure completion data for billing through this stable interface.
- The open host service insulates clinical contexts from the complexity of scheduling algorithms, insurance clearinghouse integrations, and claims processing workflows.

### Anti-Corruption Layer

The Anti-Corruption Layer pattern creates a translation boundary between contexts or between a context and an external system. It protects the internal model from external model influence.

Application in the dental domain:
- The Orthodontic Management Context uses an ACL to translate treatment plan items into its own orthodontic case model, adapting single-procedure items into multi-phase orthodontic treatment structures.
- The Periodontal Care Context uses an ACL to translate treatment plan items into its disease management model, mapping procedures into the longitudinal tracking framework.
- The Practice Management Context uses an ACL to translate between internal claim models and ANSI X12 837D formats for insurance clearinghouse communication.
- The Clinical Examination Context uses an ACL to translate dental charting data into HL7 FHIR resources for EHR integration.

## Integration Pattern Selection Criteria

The dental domain selects integration patterns based on:

1. Coupling tolerance: Clinical contexts require lower coupling to enable independent clinical workflow evolution.
2. Team autonomy: Specialty contexts (orthodontic, periodontal) benefit from patterns that allow independent model evolution.
3. External system stability: Integration with insurance clearinghouses requires robust ACLs due to regulatory format requirements.
4. Shared terminology availability: Where ADA-standardized terminology exists, published language patterns reduce translation effort.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 14 on context mapping patterns.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 3 on context mapping and integration.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapters 8-9 on integration between bounded contexts.
