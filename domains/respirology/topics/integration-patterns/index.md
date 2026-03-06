# Integration Patterns in the Respirology Domain

## Overview

Integration patterns define how bounded contexts interact with each other and with external systems. In Domain-Driven Design, integration patterns are documented in the context map and govern the flow of data, the ownership of shared concepts, and the degree of coupling between contexts. The respirology domain uses several integration patterns to connect its six bounded contexts with each other and with external hospital systems, laboratory platforms, medical devices, and health information exchanges.

## Pattern Catalog

### Shared Kernel

A shared kernel is a small, explicitly shared subset of the domain model that two or more bounded contexts agree to maintain together. Changes to the shared kernel require coordination between all contexts that use it.

**Application in Respirology**: The Airway Management Context and the Ventilatory Support Context share a kernel of airway device concepts. The shared kernel includes value objects such as AirwayDeviceType (endotracheal tube, tracheostomy tube, laryngeal mask), InterfaceSpecification (tube size, cuff type, connector type), and enumerations for device statuses. Both contexts depend on these shared types to coordinate the handoff from airway establishment to ventilator configuration.

The shared kernel is kept intentionally small. Only concepts that genuinely require identical representation in both contexts are included. Concepts specific to one context (such as intubation technique details or ventilator alarm thresholds) remain private to their respective contexts.

### Published Language

A published language is a well-documented, versioned data format used for communication between contexts. It serves as a contract that the publishing context commits to maintaining.

**Application in Respirology**: The Respiratory Diagnostics Context publishes diagnostic results using a published language aligned with HL7 FHIR DiagnosticReport and Observation resources. This standardized format allows the Chronic Disease Management Context to consume diagnostic results without depending on the internal model of the Diagnostics Context. The published language defines the structure of spirometry results, imaging reports, laboratory findings, and bronchoscopy reports in a format that is versioned and documented.

The Chronic Disease Management Context uses a published language for care plan summaries, enabling the Pulmonary Rehabilitation Context to consume care plan information without coupling to the internal care plan model.

### Customer-Supplier

In a customer-supplier relationship, the downstream (customer) context depends on the upstream (supplier) context for data. The supplier provides what the customer needs, and the two teams negotiate the interface.

**Application in Respirology**:
- The Clinical Assessment Context (supplier) provides assessment data to the Respiratory Diagnostics Context (customer), which needs assessment results to contextualize diagnostic orders.
- The Chronic Disease Management Context (supplier) provides care plan information to the Pulmonary Rehabilitation Context (customer), which needs disease severity and treatment history to design rehabilitation programs.
- The Ventilatory Support Context (supplier) provides ventilation status to the Pulmonary Rehabilitation Context (customer), which needs weaning progress to plan post-ventilation rehabilitation.

In each relationship, the supplier publishes domain events that the customer subscribes to. The customer may request additions to the event payload, and the supplier accommodates reasonable requests.

### Conformist

In a conformist relationship, the downstream context accepts the upstream context's model without negotiation. This pattern applies when the upstream system is outside the team's control and cannot be influenced.

**Application in Respirology**: The Clinical Assessment Context is a conformist to the hospital ADT (Admission, Discharge, Transfer) system. Patient admission and encounter data flows from the ADT system in its native format, and the Clinical Assessment Context must accept and translate this data without being able to request changes to the ADT model. An anti-corruption layer handles the translation.

### Open Host Service

An open host service provides a well-defined protocol for external systems to access a bounded context's functionality. The context exposes a clean API that external consumers can use without understanding its internals.

**Application in Respirology**: The Ventilatory Support Context provides an open host service for medical device integration. Ventilator manufacturers and monitoring systems can query ventilation status, receive alarm notifications, and retrieve historical parameter data through a standardized API. The open host service normalizes the internal model for external consumption.

The Respiratory Diagnostics Context provides an open host service for pulmonary function test equipment integration, accepting test results from spirometers and plethysmographs through a defined protocol.

### Anti-Corruption Layer

The anti-corruption layer (ACL) translates between external models and the internal model of a bounded context. It prevents external concepts from contaminating the domain model.

**Application in Respirology**: Anti-corruption layers are deployed at every boundary where the respirology domain interfaces with external systems:
- Between the hospital ADT system and the Clinical Assessment Context.
- Between the laboratory information system and the Respiratory Diagnostics Context.
- Between ventilator manufacturer APIs and the Ventilatory Support Context.
- Between the pharmacy system and the Airway Management Context.
- Between radiology PACS and the Respiratory Diagnostics Context.

### Separate Ways

The separate ways pattern acknowledges that some contexts have no integration relationship. They operate independently with no data flow between them.

**Application in Respirology**: The Airway Management Context and the Pulmonary Rehabilitation Context have no direct integration. While both serve respiratory patients, their activities are temporally and functionally distinct. Any coordination between them flows through intermediate contexts (e.g., Ventilatory Support or Chronic Disease Management).

## Integration Standards

### HL7 FHIR Resources

The respirology domain uses HL7 FHIR resource types as the basis for published languages:
- **DiagnosticReport**: For communicating diagnostic results.
- **Observation**: For communicating individual measurements (spirometry values, vital signs).
- **CarePlan**: For communicating care plan summaries.
- **Procedure**: For communicating completed procedures (intubation, bronchoscopy).
- **MedicationAdministration**: For communicating medication delivery events.

### Event Schema Registry

All domain events are registered in a schema registry that documents the event name, version, payload structure, publishing context, and known consumers. The registry ensures that event contracts are explicit and versioned.

## References

- Evans, E. (2003). *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley. Chapter 14 on integration patterns.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley. Chapter 3 on context mapping and integration.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media. Chapter 7 on integration patterns and context mapping.
- HL7 International. (2024). *HL7 FHIR R5 Specification*. https://hl7.org/fhir/
