# Integration Patterns in the Sleep Quality Domain

## Overview

Integration patterns define how bounded contexts in the sleep quality domain interact, share data, and maintain their autonomy. Domain-Driven Design identifies several integration patterns that describe the nature of the relationship between contexts. The sleep quality domain employs multiple integration patterns, selected based on the specific relationship dynamics between each pair of interacting contexts. Choosing the right pattern for each relationship ensures that coupling is managed appropriately and that each context can evolve at its own pace.

## Customer-Supplier Pattern

The customer-supplier pattern describes an upstream-downstream relationship where the upstream context (supplier) provides data or services that the downstream context (customer) depends on. The supplier team takes the customer's needs into account when planning changes.

### Sleep Assessment (Supplier) to Sleep Disorders (Customer)

The Sleep Assessment Context supplies assessment results to the Sleep Disorders Context. The Assessment team consults with the Disorders team when considering changes to assessment result formats or scoring methods. The Disorders team can request specific data elements in assessment events to support diagnostic evaluation. This relationship works because both teams share a clinical sleep medicine orientation and have aligned incentives.

### Sleep Assessment (Supplier) to Outcomes Tracking (Customer)

The Sleep Assessment Context supplies baseline and periodic measurements to the Outcomes Tracking Context. The Assessment team ensures that assessment events carry the metrics needed for longitudinal tracking. The Outcomes team may request additional data points as new outcome metrics are defined.

### Sleep Disorders (Supplier) to Behavioral Interventions (Customer)

The Sleep Disorders Context supplies diagnostic information to the Behavioral Interventions Context. Diagnosis events include the disorder type, severity, and criteria fulfillment needed for treatment eligibility determination and protocol selection. The Interventions team depends on consistent diagnostic classifications from the Disorders team.

### Behavioral Interventions (Supplier) to Outcomes Tracking (Customer)

The Behavioral Interventions Context supplies treatment progress events to the Outcomes Tracking Context. Session completion, adherence metrics, and protocol status changes are published for outcomes correlation. The Outcomes team can request specific intervention data points for effectiveness analysis.

## Conformist Pattern

The conformist pattern applies when the downstream context accepts the upstream context's model without modification. The downstream team conforms to the upstream model because the cost of translation does not justify maintaining a separate model.

### Device Integration (Upstream) to Sleep Assessment (Conformist)

The Sleep Assessment Context conforms to the normalized data formats produced by the Device Integration Context for actigraphy and wearable-derived metrics. Because Device Integration has already translated device-specific data into domain-standard representations, the Assessment Context can accept these representations directly without additional transformation.

### Device Integration (Upstream) to Sleep Environment (Conformist)

The Sleep Environment Context conforms to the standardized sensor data formats produced by the Device Integration Context. Normalized environmental readings (light, noise, temperature, humidity) are accepted as-is by the Environment Context.

## Shared Kernel Pattern

The shared kernel pattern applies when two bounded contexts share a small, explicitly defined subset of the domain model. Both teams agree to coordinate changes to the shared model elements.

### Sleep Environment and Behavioral Interventions (Shared Kernel)

These two contexts share a kernel of sleep hygiene concepts. Sleep hygiene education is both an environmental optimization strategy and a behavioral intervention component. The shared kernel includes a common model for sleep hygiene factors (caffeine timing, alcohol effects, exercise timing, screen use, bedroom environment rules) that both contexts reference. Changes to the shared kernel require agreement from both teams.

## Anti-Corruption Layer Pattern

The anti-corruption layer pattern applies when a bounded context must protect its internal model from the influence of an external system with a different model.

### Device Integration to External Device APIs

The Device Integration Context maintains anti-corruption layers at its boundary with each external device manufacturer API. These ACLs translate proprietary data models, terminology, units, and data structures into the canonical domain model. This is the most prominent use of the ACL pattern in the sleep quality domain, as device APIs are numerous, heterogeneous, and change frequently.

### Sleep Disorders to External EHR Systems

The Sleep Disorders Context maintains an anti-corruption layer at its boundary with external electronic health record systems. The ACL translates between ICD-10-CM codes and the domain's ICSD-3-based classification model.

## Open Host Service Pattern

The open host service pattern provides a well-defined protocol for external consumers to access a bounded context's capabilities.

### Outcomes Tracking Open Host Service

The Outcomes Tracking Context exposes an open host service for reporting and analytics. External systems (clinical dashboards, research platforms, patient-facing applications) consume outcomes data through this published interface. The service defines a stable API contract with versioning support.

## Published Language Pattern

The published language pattern defines a shared language for inter-context communication, expressed in domain events and data contracts.

### Domain Event Schemas

All domain events in the sleep quality domain use a published language expressed in versioned event schemas. These schemas define the structure, data types, and semantics of event payloads. The published language ensures that event producers and consumers share a common understanding of event content, independent of their internal models.

## Separate Ways Pattern

Some potential integrations are not worth the coupling cost. The Separate Ways pattern applies when two contexts are better off not integrating.

### Sleep Environment and Sleep Disorders

While environmental factors can influence sleep disorder severity, the Sleep Environment Context and Sleep Disorders Context do not directly integrate. Environmental data reaches the Disorders Context indirectly through assessment results and outcomes tracking. Direct integration would create unnecessary coupling between two contexts with different change frequencies and team compositions.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley. Chapter 14 on context mapping patterns.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley. Chapter 3 on context mapping.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media. Chapter 8 on integration patterns.
