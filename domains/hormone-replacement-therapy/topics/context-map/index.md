# Context Map in the Hormone Replacement Therapy Domain

A context map is a visual and conceptual representation of the relationships and interactions between bounded contexts within a domain. In the hormone replacement therapy (HRT) domain, the context map reveals how clinical assessment, protocol management, lab monitoring, side effect tracking, treatment optimization, and outcomes tracking relate to each other and to external systems.

## Purpose

The context map serves as the primary strategic design artifact for understanding system integration. It makes explicit which contexts are upstream or downstream, which share kernels, which conform to others, and where anti-corruption layers are necessary. For HRT systems, the context map captures the flow from initial patient assessment through treatment delivery, monitoring, adjustment, and outcome measurement.

## Context Relationships

### Clinical Assessment to Hormone Protocol (Customer-Supplier)

The Clinical Assessment Context acts as the upstream supplier of patient evaluation data. The Hormone Protocol Context is the downstream customer that consumes assessment results to generate treatment protocols. The Clinical Assessment Context publishes AssessmentCompleted events containing symptom profiles, hormone panel interpretations, and candidacy determinations. The Hormone Protocol Context consumes these events and translates them into protocol selection criteria. The supplier commits to a stable published language for assessment outputs.

### Hormone Protocol to Lab Monitoring (Customer-Supplier)

The Hormone Protocol Context supplies monitoring requirements to the Lab Monitoring Context. When a protocol is prescribed, it includes the required laboratory tests, monitoring frequency, and therapeutic target ranges. The Lab Monitoring Context conforms to these specifications when generating monitoring plans. This is a customer-supplier relationship where the Protocol Context dictates what must be monitored.

### Lab Monitoring to Side Effect Management (Published Language)

The Lab Monitoring Context publishes lab results and threshold exceedance alerts using a published language that the Side Effect Management Context consumes. The published language defines standard representations for lab values, reference ranges, and alert severities. This ensures that side effect detection can respond to laboratory signals without coupling to the internal model of the Lab Monitoring Context.

### Lab Monitoring to Treatment Optimization (Open Host Service)

The Lab Monitoring Context exposes an open host service that the Treatment Optimization Context queries for trend data, historical results, and current status. This pattern allows Treatment Optimization to access rich monitoring data without requiring the Lab Monitoring Context to anticipate every optimization query.

### Side Effect Management to Treatment Optimization (Conformist)

The Treatment Optimization Context acts as a conformist to the Side Effect Management Context, adopting its adverse event classifications and risk levels directly. This design choice reflects the clinical reality that treatment adjustments must respect the exact risk assessments produced by pharmacovigilance processes.

### Treatment Optimization to Hormone Protocol (Customer-Supplier)

The Treatment Optimization Context produces OptimizationRecommended events that the Hormone Protocol Context receives. The Protocol Context retains authority to accept, modify, or reject recommendations, making Treatment Optimization the upstream supplier of advisory data and Hormone Protocol the customer with final decision authority.

### Outcomes Tracking to Treatment Optimization (Published Language)

The Outcomes Tracking Context publishes treatment efficacy data, quality of life scores, and longitudinal health outcomes using a published language. Treatment Optimization consumes this data to inform evidence-based protocol adjustments. The published language ensures that outcomes data is interpretable without coupling to the Outcomes Tracking internal model.

### Clinical Assessment to Side Effect Management (Shared Kernel)

These two contexts share a small kernel of patient risk factor data. Both contexts need to understand baseline risk factors such as family history of thromboembolism or breast cancer. The shared kernel is minimal and versioned, consisting of a RiskFactorProfile value object that both contexts reference.

## External System Integration

### Pharmacy Systems (Anti-Corruption Layer)

The Hormone Protocol Context integrates with external pharmacy systems for prescription fulfillment. An anti-corruption layer translates between the domain's protocol model and pharmacy-specific formats such as NCPDP SCRIPT or HL7 FHIR MedicationRequest resources.

### Laboratory Information Systems (Anti-Corruption Layer)

The Lab Monitoring Context integrates with external laboratory information systems (LIS) through an anti-corruption layer that translates HL7 v2 ORU messages or FHIR DiagnosticReport resources into domain-specific LabResult value objects.

### Regulatory Reporting Systems (Anti-Corruption Layer)

The Side Effect Management Context integrates with regulatory reporting systems such as FDA MedWatch through an anti-corruption layer that translates internal adverse event records into required reporting formats without exposing the internal domain model.

## Map Summary

The overall flow follows the clinical treatment lifecycle: Clinical Assessment feeds Hormone Protocol, which feeds Lab Monitoring. Lab Monitoring feeds both Side Effect Management and Treatment Optimization. Side Effect Management feeds Treatment Optimization. Treatment Optimization feeds back into Hormone Protocol. Outcomes Tracking receives from all contexts and feeds back into Treatment Optimization, creating a continuous improvement loop.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 14 on context maps.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 3 on context mapping.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 8 on context mapping patterns.
- Brandolini, Alberto. "Introducing EventStorming." Leanpub, 2021.
