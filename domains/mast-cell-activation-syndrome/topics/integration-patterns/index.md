# Integration Patterns for Mast Cell Activation Syndrome

## Overview

Integration patterns define how bounded contexts interact and share information. Domain-Driven Design identifies several integration patterns that describe the nature of relationships between contexts. In the MCAS domain, these patterns govern how diagnostic data flows to treatment contexts, how symptom information reaches analytical contexts, and how external healthcare systems connect to the domain.

Choosing the right integration pattern for each context relationship determines the degree of coupling, the autonomy of each context, and the complexity of maintaining the integration over time.

## Customer-Supplier Pattern

The customer-supplier pattern is the most common integration pattern in the MCAS domain. In this pattern, the upstream supplier context provides data or services that the downstream customer context depends on. The supplier accommodates the customer's needs through negotiated interfaces.

### Diagnostic Assessment to Trigger Management

The Diagnostic Assessment Context supplies diagnostic conclusions to the Trigger Management Context. The diagnostic context (supplier) publishes DiagnosisConfirmed events containing the information that the trigger management context (customer) needs to create initial trigger profiles. The two teams negotiate the event schema to ensure it contains sufficient diagnostic detail for trigger assessment.

### Diagnostic Assessment to Medication Protocol

The Diagnostic Assessment Context supplies diagnostic results to the Medication Protocol Context. Confirmed mediator profiles inform initial medication selection. The medication protocol team specifies what diagnostic information they need, and the diagnostic assessment team ensures the DiagnosisConfirmed event payload includes those details.

### Symptom Tracking to Outcomes Measurement

The Symptom Tracking Context is the primary data supplier for the Outcomes Measurement Context. Symptom events flow continuously from tracking to measurement. The outcomes measurement team defines the symptom data format requirements, and the symptom tracking team publishes events that meet those specifications.

### Medication Protocol to Outcomes Measurement

The Medication Protocol Context supplies medication change events to the Outcomes Measurement Context. Medication start, titration, and discontinuation events enable effectiveness analysis. The interface is negotiated to include sufficient medication and timing detail for correlation analysis.

## Published Language Pattern

The published language pattern defines a shared schema or protocol that multiple contexts use for communication. The language is not owned by any single context but is maintained as a shared asset.

### Symptom Event Language

The MCAS domain defines a published language for symptom events. This shared schema specifies the structure of symptom entries including organ system classification, severity scoring scale, timestamp format, and assessor type. Both the Symptom Tracking Context (publisher) and multiple consumer contexts (Trigger Management, Medication Protocol, Outcomes Measurement) conform to this shared language.

The published language evolves through a governance process that involves representatives from all contexts that use it. Changes are versioned and backward compatibility is maintained to prevent breaking existing integrations.

### Mediator Measurement Language

A published language for mediator measurements standardizes how laboratory results are represented across the domain. This language defines the structure for tryptase levels, histamine metabolite levels, prostaglandin D2 levels, and other mediator measurements. It is used by the Diagnostic Assessment Context when publishing results and by any context that consumes mediator data.

## Open Host Service Pattern

The open host service pattern provides a standardized interface that any context can use to access services. The host context defines a protocol that multiple consumers can use without individual negotiation.

### Specialist Coordination as Open Host

The Specialist Coordination Context provides an open host service for care plan access and referral management. Any clinical context can query the current shared care plan, submit referral requests, and publish care plan updates through a standardized interface. The coordination context does not customize its interface for individual consumers; instead, it provides a general-purpose protocol that all contexts use.

This pattern is appropriate for the Specialist Coordination Context because it serves as a hub for multiple contexts with similar integration needs. Maintaining individual customer-supplier relationships with each clinical context would create unnecessary complexity.

## Conformist Pattern

The conformist pattern applies when a downstream context adopts the upstream context's model without modification. The downstream team accepts the upstream model as-is rather than translating it.

### Outcomes Measurement Conforming to Trigger Management

The Outcomes Measurement Context conforms to the Trigger Management Context's trigger categorization model when assessing trigger avoidance effectiveness. Rather than creating its own trigger classification, the outcomes context accepts the trigger management model directly. This simplifies the integration but creates a dependency on the trigger management model's stability.

## Shared Kernel Pattern

The shared kernel pattern involves two or more contexts sharing a small, carefully managed subset of the domain model. Changes to the shared kernel require agreement from all participating teams.

### Patient Identity Kernel

The MCAS domain uses a shared kernel for patient identity. All contexts share a common patient identifier format and a minimal patient reference model. This kernel is small by design, containing only the patient identifier, a display name, and the diagnostic status. Changes to this kernel require coordination across all context teams.

## Anti-Corruption Layer Pattern

Anti-corruption layers protect bounded contexts from external system models. In the MCAS domain, ACLs are used at the boundaries with external healthcare systems.

### Laboratory System ACL

The Diagnostic Assessment Context uses an ACL to translate laboratory information system data into its internal model. The ACL maps LOINC codes, converts units, and transforms result formats.

### EHR System ACL

Multiple contexts use ACLs to integrate with electronic health record systems. Each ACL translates between HL7 FHIR resources and the consuming context's internal model.

### Pharmacy System ACL

The Medication Protocol Context uses an ACL to integrate with pharmacy systems for prescription processing and compounding requests. The ACL translates between the domain's medication model and pharmacy formulary systems.

## Choosing Integration Patterns

The choice of integration pattern depends on the nature of the relationship between contexts. Customer-supplier is used when there is a clear upstream-downstream relationship with the ability to negotiate interfaces. Published language is used when multiple contexts need a shared communication format. Open host service is used when one context serves multiple consumers with similar needs. Conformist is used when the downstream context has no leverage or need to diverge from the upstream model. Anti-corruption layer is used when integrating with external systems whose models must not contaminate the domain.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 14 on integration patterns.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 13 on context integration.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 9 on communication patterns.
