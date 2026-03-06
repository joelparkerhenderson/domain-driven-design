# Context Map in the Pulmonolgy Domain

## Overview

A context map is a visual and conceptual representation of the relationships and interactions between bounded contexts within a domain. In the pulmonolgy domain, the context map documents how six bounded contexts communicate, share data, and depend on one another across the full spectrum of respiratory care.

The context map serves as a strategic navigation tool for the entire domain. It reveals upstream and downstream dependencies, identifies integration patterns in use, and exposes potential friction points where translation or anti-corruption layers are needed. For pulmonolgy, the context map traces the patient journey from initial assessment through diagnostics, chronic management, acute care, sleep evaluation, and rehabilitation.

## Context Relationships

### Pulmonary Assessment Context (Upstream) to Respiratory Diagnostics Context (Downstream)

Relationship type: Customer-Supplier. The Pulmonary Assessment Context produces assessment findings and diagnostic orders that the Respiratory Diagnostics Context consumes. The diagnostics team (customer) negotiates the data format and content of assessment summaries with the assessment team (supplier). Assessment results flow downstream to inform which diagnostic procedures are ordered.

### Pulmonary Assessment Context (Upstream) to Chronic Disease Management Context (Downstream)

Relationship type: Customer-Supplier. Initial assessment findings, including PFT results and symptom evaluations, flow from the assessment context into the chronic disease management context. The chronic disease management team depends on assessment data to establish baseline measurements and initiate treatment plans.

### Respiratory Diagnostics Context to Chronic Disease Management Context

Relationship type: Partnership. These two contexts maintain a close collaborative relationship. Diagnostic results directly inform disease classification and treatment plan adjustments, while chronic disease management data helps prioritize which diagnostic procedures are clinically indicated. Both teams coordinate model changes and share a published language for diagnostic result communication.

### Chronic Disease Management Context (Upstream) to Critical Care Context (Downstream)

Relationship type: Conformist. When a chronic disease patient experiences acute respiratory failure, the Critical Care Context receives patient history from the Chronic Disease Management Context. The critical care team conforms to the chronic disease management model for historical data, adapting it to its own real-time clinical model without attempting to negotiate changes to the upstream format.

### Chronic Disease Management Context (Upstream) to Pulmonary Rehabilitation Context (Downstream)

Relationship type: Customer-Supplier. The rehabilitation context consumes disease severity data, functional capacity measurements, and treatment goals from the chronic disease management context. The rehabilitation team specifies what data it needs from the chronic disease management team to design appropriate exercise programs and track outcomes.

### Sleep Medicine Context to Pulmonary Assessment Context

Relationship type: Shared Kernel. The Sleep Medicine Context and Pulmonary Assessment Context share a small, carefully managed subset of the model related to respiratory symptom evaluation. Both contexts use the same symptom severity scales and daytime sleepiness assessments, and changes to this shared kernel require agreement from both teams.

### Critical Care Context to Pulmonary Rehabilitation Context

Relationship type: Anti-Corruption Layer. Post-ICU patients transitioning to rehabilitation require data from the critical care context, but the rehabilitation context translates this data through an anti-corruption layer. ICU-specific concepts like ventilator modes and sedation scores are translated into rehabilitation-relevant concepts like functional status and exercise tolerance.

### Sleep Medicine Context to External Sleep Lab Systems

Relationship type: Anti-Corruption Layer. External sleep lab data systems use vendor-specific formats and proprietary data models. The Sleep Medicine Context maintains an anti-corruption layer that translates external polysomnography data, CPAP compliance downloads, and scoring results into the domain's internal model.

## Upstream and Downstream Summary

Upstream contexts (producers of data):
- Pulmonary Assessment Context: Produces assessment findings consumed by Respiratory Diagnostics and Chronic Disease Management.
- Chronic Disease Management Context: Produces disease history consumed by Critical Care and Pulmonary Rehabilitation.
- Respiratory Diagnostics Context: Produces diagnostic results consumed by Chronic Disease Management.

Downstream contexts (consumers of data):
- Respiratory Diagnostics Context: Consumes assessment orders and findings.
- Critical Care Context: Consumes chronic disease history during acute episodes.
- Pulmonary Rehabilitation Context: Consumes disease management data and post-ICU data.

Bidirectional contexts:
- Sleep Medicine Context: Shares data with Pulmonary Assessment through a shared kernel and communicates with external systems through anti-corruption layers.

## Integration Friction Points

The context map reveals several areas where integration requires careful attention:

- The transition from Chronic Disease Management to Critical Care involves a shift from longitudinal to real-time clinical models, requiring careful data translation and temporal alignment.
- External sleep lab system integration introduces vendor-specific variability that the anti-corruption layer must absorb.
- The partnership between Respiratory Diagnostics and Chronic Disease Management requires ongoing coordination to keep shared interfaces aligned as both models evolve.

## Maintaining the Context Map

The context map is a living document that should be updated when new bounded contexts are introduced, when relationships between contexts change, or when integration patterns are modified. Regular reviews with domain experts from each bounded context ensure the map remains accurate and useful.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 14 on context maps and their role in maintaining model integrity.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 3 on context mapping patterns and relationship types.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 8 on context mapping and integration strategies.
