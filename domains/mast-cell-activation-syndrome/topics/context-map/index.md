# Context Map for Mast Cell Activation Syndrome

## Overview

A context map provides a visual and conceptual representation of the relationships and interactions between bounded contexts in a domain. For the MCAS domain, the context map reveals how diagnostic assessment, trigger management, medication protocols, symptom tracking, specialist coordination, and outcomes measurement relate to one another and to external systems.

The context map captures both the structural relationships between contexts and the nature of their integration. It identifies which contexts are upstream providers of information, which are downstream consumers, and what integration patterns govern their interactions.

## Context Relationships

### Diagnostic Assessment to Trigger Management

Relationship type: Customer-Supplier. The Diagnostic Assessment Context is upstream, publishing confirmed diagnoses and mediator profiles. The Trigger Management Context is downstream, consuming diagnostic information to inform trigger identification strategies. A confirmed diagnosis triggers the creation of an initial trigger assessment plan tailored to the patient's specific mast cell activation pattern.

### Diagnostic Assessment to Medication Protocol

Relationship type: Customer-Supplier. The Diagnostic Assessment Context supplies diagnostic conclusions that the Medication Protocol Context uses to determine initial treatment recommendations. Diagnostic mediator levels inform which medication classes are prioritized in the treatment regimen.

### Symptom Tracking to Trigger Management

Relationship type: Published Language. The Symptom Tracking Context publishes symptom events using a shared event schema. The Trigger Management Context subscribes to these events to correlate symptom flares with potential triggers. The published language defines standard symptom categories, severity scales, and temporal markers.

### Symptom Tracking to Medication Protocol

Relationship type: Published Language. Symptom events published by the Symptom Tracking Context are consumed by the Medication Protocol Context to assess medication response and inform dosage adjustments. Treatment response is evaluated by comparing symptom patterns before and after medication changes.

### Symptom Tracking to Outcomes Measurement

Relationship type: Customer-Supplier. The Symptom Tracking Context is the primary upstream data source for the Outcomes Measurement Context. Symptom burden calculations, quality of life scoring, and trend analysis all depend on the continuous flow of symptom data.

### Medication Protocol to Outcomes Measurement

Relationship type: Customer-Supplier. The Medication Protocol Context publishes medication change events that the Outcomes Measurement Context consumes to correlate treatment modifications with outcome changes. This relationship enables effectiveness analysis for specific medications and regimen changes.

### Trigger Management to Outcomes Measurement

Relationship type: Conformist. The Outcomes Measurement Context conforms to the Trigger Management Context's model when assessing trigger avoidance effectiveness. It accepts the trigger categorization scheme without modification to measure how well avoidance strategies reduce flare frequency.

### Specialist Coordination to All Clinical Contexts

Relationship type: Open Host Service. The Specialist Coordination Context provides a standardized interface through which any clinical context can publish or retrieve care plan information. It serves as a coordination hub without imposing its model on other contexts. Each clinical context communicates with the coordination layer through a well-defined protocol.

## External System Integration

### Electronic Health Records (EHR)

The MCAS domain integrates with external EHR systems through anti-corruption layers. The Diagnostic Assessment Context translates EHR laboratory results into its own mediator level model. The Specialist Coordination Context maps provider information from the EHR into its care team model.

### Laboratory Information Systems (LIS)

Laboratory results for tryptase, histamine metabolites, and prostaglandin D2 flow from external LIS through an anti-corruption layer into the Diagnostic Assessment Context. The translation layer maps laboratory-specific result formats into the domain's standardized mediator measurement model.

### Pharmacy Systems

The Medication Protocol Context integrates with external pharmacy systems for prescription processing and compounding requests. An anti-corruption layer translates between the domain's medication model and the pharmacy system's formulary and dispensing models.

## Integration Patterns Summary

The MCAS context map employs four primary integration patterns. Customer-Supplier relationships govern most inter-context data flows. Published Language provides standardized event schemas for symptom data sharing. Open Host Service enables the Specialist Coordination Context to serve multiple consumers. Anti-Corruption Layers protect internal contexts from external system complexity.

## Evolution Considerations

The context map is expected to evolve as MCAS diagnostic criteria are refined and new treatment modalities emerge. The loose coupling achieved through domain events and anti-corruption layers ensures that changes in one context do not cascade through the entire system.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 14 on context maps.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 3 on context mapping.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 8 on context map patterns.
