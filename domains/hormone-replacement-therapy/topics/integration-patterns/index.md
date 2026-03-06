# Integration Patterns in the Hormone Replacement Therapy Domain

Integration patterns define how bounded contexts interact, share data, and coordinate activities within the hormone replacement therapy (HRT) domain. Domain-Driven Design identifies several integration patterns that are applicable across the six HRT bounded contexts and between the domain and external systems.

## Purpose

The HRT domain requires coordinated interaction between clinical assessment, protocol management, laboratory monitoring, side effect tracking, treatment optimization, and outcomes measurement. Each interaction point needs a clearly defined integration pattern that establishes responsibilities, data ownership, and coupling characteristics. Choosing the right integration pattern for each relationship is a strategic design decision that affects system maintainability, autonomy, and evolvability.

## Customer-Supplier Pattern

### Clinical Assessment to Hormone Protocol

The Clinical Assessment Context (supplier) provides patient evaluation data to the Hormone Protocol Context (customer). The supplier commits to a stable interface for assessment outputs, including the AssessmentCompleted event schema and the structure of CandidacyStatus and RiskFactorProfile value objects. The customer relies on this interface for protocol selection. Changes to the assessment output format require coordination between the two teams, with the supplier accommodating reasonable customer needs while maintaining its own domain model integrity.

### Hormone Protocol to Lab Monitoring

The Hormone Protocol Context (supplier) provides monitoring requirements to the Lab Monitoring Context (customer). When a ProtocolPrescribed event is published, it includes the required test types, monitoring frequencies, and therapeutic target ranges. The Lab Monitoring Context conforms to these specifications when generating monitoring plans. The supplier defines what must be monitored; the customer determines how monitoring is implemented and managed.

### Treatment Optimization to Hormone Protocol

The Treatment Optimization Context (supplier) provides protocol change recommendations to the Hormone Protocol Context (customer). The OptimizationRecommended event carries proposed changes with supporting evidence. The Hormone Protocol Context retains full authority to accept, modify, or reject recommendations. This customer-supplier relationship is advisory, not prescriptive, reflecting the clinical reality that prescriber judgment supersedes algorithmic recommendations.

## Published Language Pattern

### Lab Monitoring Published Language

The Lab Monitoring Context publishes lab results and threshold alerts using a published language that defines standard representations for laboratory values, units of measurement, reference ranges, therapeutic targets, and alert classifications. The published language schema is versioned and documented, enabling consuming contexts (Side Effect Management, Treatment Optimization) to parse and interpret results without coupling to the Lab Monitoring internal model. This pattern is chosen because multiple consumers with different needs access the same data.

### Outcomes Tracking Published Language

The Outcomes Tracking Context publishes treatment efficacy data using a published language that defines standard representations for symptom resolution rates, quality of life scores, efficacy ratings, and longitudinal measurement summaries. The Treatment Optimization Context consumes this published language to incorporate outcomes evidence into optimization calculations.

## Open Host Service Pattern

### Lab Monitoring Open Host Service

The Lab Monitoring Context exposes an open host service that the Treatment Optimization Context queries for rich monitoring data. While published events cover real-time notifications, the open host service supports analytical queries such as retrieving trend data for a specific test type over a date range, comparing current levels against historical baselines, and accessing cross-test correlation data. The open host service defines a public protocol that any authorized consumer can use without bilateral negotiation.

## Shared Kernel Pattern

### Clinical Assessment and Side Effect Management Shared Kernel

These two contexts share a small, carefully managed kernel of patient risk factor data. The RiskFactorProfile value object is jointly owned, versioned, and maintained by both contexts. Changes to the shared kernel require agreement from both teams. The kernel is kept minimal to limit coupling: it includes risk factor definitions and severity classifications but not the assessment logic or mitigation logic that each context applies independently.

## Conformist Pattern

### Side Effect Management to Treatment Optimization

The Treatment Optimization Context acts as a conformist to the Side Effect Management Context, adopting its adverse event classifications and risk levels directly without translation. This design choice reflects the clinical principle that treatment adjustments must respect the exact risk assessments produced by pharmacovigilance processes. The Treatment Optimization Context conforms to whatever event classifications and risk tier definitions the Side Effect Management Context produces.

## Anti-Corruption Layer Pattern

### External Laboratory Information System

The Lab Monitoring Context maintains an anti-corruption layer against external laboratory information systems that communicate using HL7 v2 or FHIR formats. The ACL translates between external standard representations and internal domain value objects, preventing healthcare interoperability standards from dictating the domain model structure.

### External Pharmacy Systems

The Hormone Protocol Context maintains an anti-corruption layer against pharmacy management systems. The ACL translates between the domain's TreatmentProtocol aggregate and pharmacy-specific formats, mapping formulation identifiers to NDC codes and translating dosing schedules into prescription formats.

### External EHR Systems

Multiple contexts maintain anti-corruption layers against electronic health record systems. Each ACL is context-specific, translating only the data relevant to that context's needs and preventing the broad, generalized EHR data model from contaminating specialized domain models.

### Regulatory Reporting Systems

The Side Effect Management Context maintains an anti-corruption layer against regulatory reporting systems such as FDA MedWatch and international pharmacovigilance databases. The ACL translates internal adverse event records into required regulatory formats.

## Separate Ways Pattern

The Clinical Assessment Context and the Outcomes Tracking Context operate independently for their core functions. While Outcomes Tracking receives data from all contexts, it does not share a direct integration pattern with Clinical Assessment for ongoing operations. Each context handles patient data independently, with Clinical Assessment focused on intake evaluation and Outcomes Tracking focused on longitudinal measurement. They go their separate ways except when the TreatmentMilestoneReached event triggers a reassessment through the Clinical Assessment Context.

## Integration Governance

Integration patterns are documented in the context map and reviewed when bounded context responsibilities change. Pattern selection follows the principle of choosing the least coupling that still meets clinical workflow requirements. Shared kernels are minimized. Anti-corruption layers are mandated for all external system integrations. Published languages are versioned with backward compatibility commitments.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 14 on maintaining model integrity.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 3 on context mapping.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapters 8-9 on integration patterns.
- Hohpe, Gregor, and Woolf, Bobby. Enterprise Integration Patterns. Addison-Wesley, 2003.
