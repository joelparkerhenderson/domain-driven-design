# Context Map in the Respirology Domain

## Overview

A context map is a visual and documented representation of the relationships and interactions between bounded contexts within a domain. In the respirology domain, the context map describes how the six bounded contexts -- Clinical Assessment, Respiratory Diagnostics, Airway Management, Chronic Disease Management, Ventilatory Support, and Pulmonary Rehabilitation -- communicate, share data, and depend on one another. The context map is a strategic design tool that makes integration patterns explicit and guides architectural decisions.

## Upstream and Downstream Relationships

### Clinical Assessment Context (Upstream) to Respiratory Diagnostics Context (Downstream)

Relationship type: Customer-Supplier.

The Clinical Assessment Context produces respiratory assessments that inform which diagnostic tests should be ordered. The Respiratory Diagnostics Context acts as the customer, requesting assessment data to prioritize and contextualize diagnostic workups. The Clinical Assessment Context publishes AssessmentCompleted events that the Diagnostics Context subscribes to.

### Respiratory Diagnostics Context (Upstream) to Chronic Disease Management Context (Downstream)

Relationship type: Published Language.

The Respiratory Diagnostics Context publishes diagnostic results using a standardized format aligned with HL7 FHIR DiagnosticReport resources. The Chronic Disease Management Context consumes these results to update care plans, adjust GOLD classifications, and trigger exacerbation protocols. The published language ensures that diagnostic data is exchanged without requiring tight coupling between the two contexts.

### Clinical Assessment Context (Upstream) to Chronic Disease Management Context (Downstream)

Relationship type: Customer-Supplier.

Clinical assessments feed into chronic disease management by providing updated symptom scores, risk stratification results, and examination findings. The Chronic Disease Management Context depends on regular assessment updates to maintain current care plans.

### Airway Management Context to Ventilatory Support Context

Relationship type: Shared Kernel.

These two contexts share a small kernel of concepts related to airway devices, tube types, and ventilation interfaces. When an airway is established through intubation or tracheostomy, the Ventilatory Support Context must know the device characteristics to configure ventilation appropriately. The shared kernel defines common types such as AirwayDeviceType and InterfaceSpecification.

### Ventilatory Support Context (Upstream) to Pulmonary Rehabilitation Context (Downstream)

Relationship type: Customer-Supplier.

The Ventilatory Support Context supplies ventilation status and weaning progress information that the Pulmonary Rehabilitation Context uses to design appropriate exercise programs. Patients transitioning from ventilatory support to rehabilitation require coordinated handoff.

### Chronic Disease Management Context (Upstream) to Pulmonary Rehabilitation Context (Downstream)

Relationship type: Customer-Supplier.

The Chronic Disease Management Context provides care plan details, disease severity classifications, and exacerbation history that the Pulmonary Rehabilitation Context uses to tailor rehabilitation programs to individual patient needs.

## External System Integrations

### Hospital ADT System (External) to Clinical Assessment Context

Relationship type: Conformist.

The Clinical Assessment Context must accept patient admission, discharge, and transfer data in the format provided by the hospital ADT system. An anti-corruption layer translates ADT messages into the Clinical Assessment Context's internal model.

### Laboratory Information System (External) to Respiratory Diagnostics Context

Relationship type: Anti-Corruption Layer.

Laboratory results for sputum culture and sensitivity, arterial blood gases, and other respiratory-related lab tests flow from the external laboratory system through an anti-corruption layer that maps laboratory data models to the Respiratory Diagnostics Context's internal model.

### Medical Device Integration (External) to Ventilatory Support Context

Relationship type: Open Host Service.

Ventilator devices expose data through manufacturer-specific APIs. The Ventilatory Support Context provides an open host service that normalizes device data from multiple manufacturers into a unified internal representation. This service also handles alarm forwarding and parameter updates.

## Event Flow Patterns

The context map reveals several critical event flows:

1. **Assessment-to-Diagnosis Flow**: AssessmentCompleted -> DiagnosticOrderCreated -> DiagnosticResultAvailable
2. **Diagnosis-to-Care-Plan Flow**: DiagnosticResultAvailable -> CarePlanUpdated -> ActionPlanRevised
3. **Airway-to-Ventilation Flow**: AirwayEstablished -> VentilatorConfigured -> VentilationStarted
4. **Ventilation-to-Rehabilitation Flow**: WeaningCompleted -> RehabilitationReferralCreated -> ProgramDesigned
5. **Exacerbation Flow**: ExacerbationDetected -> CarePlanEscalated -> AssessmentRequested

## Context Map Maintenance

The context map is a living document that must be updated as the respirology domain evolves. When new bounded contexts are identified, existing relationships change, or new external integrations are added, the context map should be revised. Regular review sessions with domain experts and development teams ensure the map remains accurate.

## References

- Evans, E. (2003). *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley. Chapter 14 on context maps.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley. Chapter 3 on context mapping.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media. Chapter 7 on context mapping patterns.
