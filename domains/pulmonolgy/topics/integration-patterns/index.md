# Integration Patterns in the Pulmonolgy Domain

## Overview

Integration patterns define how bounded contexts share data, coordinate workflows, and maintain model integrity across boundaries. In Domain-Driven Design, several established integration patterns describe the spectrum of relationships between contexts, from tightly coupled shared kernels to loosely coupled published language interfaces. The pulmonolgy domain employs multiple integration patterns based on the specific relationship requirements between its six bounded contexts.

Selecting the right integration pattern for each context relationship is a strategic design decision. Tighter coupling patterns offer simplicity and consistency but reduce autonomy. Looser coupling patterns maximize independence but increase the complexity of translation and synchronization. The pulmonolgy domain balances these trade-offs based on clinical workflow requirements and organizational structure.

## Customer-Supplier Pattern

The customer-supplier pattern establishes an upstream-downstream relationship where the downstream context (customer) depends on data produced by the upstream context (supplier). The customer can negotiate the format and content of the data it receives, and the supplier commits to meeting those needs.

### Pulmonary Assessment to Respiratory Diagnostics

The Pulmonary Assessment Context (supplier) provides assessment findings to the Respiratory Diagnostics Context (customer). The diagnostics team specifies what assessment data it needs to determine diagnostic indications, and the assessment team commits to providing that data in the agreed format. Changes to the assessment output format are negotiated with the diagnostics team before implementation.

### Pulmonary Assessment to Chronic Disease Management

The Pulmonary Assessment Context (supplier) provides baseline assessment data to the Chronic Disease Management Context (customer). The chronic disease management team specifies the assessment data elements needed for disease classification and baseline establishment.

### Chronic Disease Management to Pulmonary Rehabilitation

The Chronic Disease Management Context (supplier) provides disease severity data, treatment goals, and functional status information to the Pulmonary Rehabilitation Context (customer). The rehabilitation team specifies what clinical data is needed to design appropriate exercise programs.

## Partnership Pattern

The partnership pattern describes two contexts that have a mutual dependency and coordinate their development efforts. Neither context is strictly upstream or downstream; both evolve together with bilateral coordination.

### Respiratory Diagnostics and Chronic Disease Management

These two contexts maintain a partnership relationship because they have bidirectional data dependencies. Diagnostic results inform disease classification and treatment planning, while chronic disease management data helps prioritize diagnostic procedures. Both teams coordinate model changes and share a published language for diagnostic result communication.

This partnership requires regular synchronization meetings between the diagnostics and chronic disease management teams to ensure that changes in diagnostic classification systems are reflected in disease management models, and vice versa.

## Conformist Pattern

The conformist pattern applies when a downstream context adopts the upstream context's model without negotiation. The downstream team conforms to whatever the upstream team provides, translating internally as needed.

### Chronic Disease Management to Critical Care

When a chronic disease patient enters the ICU, the Critical Care Context receives patient history from the Chronic Disease Management Context and conforms to its data model for historical information. The critical care team does not negotiate changes to how chronic disease management represents patient history. Instead, it accepts the upstream model and maps relevant elements into its own real-time clinical model.

This conformist approach is appropriate because the critical care team needs historical context quickly and cannot afford the coordination overhead of negotiating upstream changes during acute care situations.

## Shared Kernel Pattern

The shared kernel pattern defines a small, carefully managed subset of the model that is shared between two contexts. Changes to the shared kernel require agreement from both teams, and the shared portion is typically kept as small as possible to minimize coupling.

### Sleep Medicine and Pulmonary Assessment Shared Kernel

The Sleep Medicine Context and Pulmonary Assessment Context share a small model subset for respiratory symptom evaluation. This includes the Epworth Sleepiness Scale, daytime symptom severity scales, and respiratory symptom questionnaires that both contexts use in their clinical evaluations.

The shared kernel is governed by a joint review process. Any proposed changes to shared symptom evaluation instruments must be reviewed and approved by both the sleep medicine and pulmonary assessment teams before implementation.

## Anti-Corruption Layer Pattern

The anti-corruption layer pattern creates a translation boundary that prevents external or upstream models from contaminating the downstream context's internal model. See the dedicated anti-corruption layer topic for detailed coverage.

Key applications in the pulmonolgy domain:
- Sleep Medicine Context to external sleep lab systems.
- Critical Care Context to Pulmonary Rehabilitation Context.
- Pulmonary Assessment Context to external radiology PACS.
- Chronic Disease Management Context to external pharmacy systems.

## Published Language Pattern

The published language pattern defines a well-documented, shared data format that contexts use for communication. The language is published as a formal specification that any context can implement.

### Diagnostic Result Published Language

The Respiratory Diagnostics Context and Chronic Disease Management Context communicate diagnostic results using a published language that defines the structure, terminology, and semantics of diagnostic result messages. This published language specifies:

- Standard diagnostic classification codes and their meanings.
- Result severity indicators and their clinical significance.
- Specimen identification and chain-of-custody documentation.
- Temporal markers for result validity and review status.

### Clinical Measurement Published Language

Multiple contexts share a published language for clinical measurements including spirometry results, oxygen saturation readings, and arterial blood gas values. This published language standardizes units, reference range representations, and quality indicators across context boundaries.

## Open Host Service Pattern

The open host service pattern provides a well-defined protocol that any context can use to access a service's capabilities. The service defines its interface publicly, and consumers use it without bilateral negotiation.

### Assessment Data Service

The Pulmonary Assessment Context exposes an open host service for retrieving patient assessment data. Any authorized context can query this service using the published interface. The service returns assessment summaries in the published language format, shielding consumers from internal assessment model details.

## Pattern Selection Criteria

The choice of integration pattern for each relationship considers several factors:

- **Team autonomy**: How independently do the teams need to evolve their models?
- **Data dependency direction**: Is the dependency unidirectional or bidirectional?
- **Change frequency**: How often do the shared data structures change?
- **Clinical criticality**: How time-sensitive is the data exchange?
- **Organizational alignment**: Do the contexts belong to the same or different organizational units?

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 14 on integration patterns between bounded contexts.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 3 on context mapping patterns and integration strategies.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapters 8-9 on integration patterns and their application in context mapping.
