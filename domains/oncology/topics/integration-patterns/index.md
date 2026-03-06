# Integration Patterns in Oncology

## Overview

Integration patterns define how bounded contexts interact with each other and with external systems. In Domain-Driven Design, integration patterns describe the nature of the relationship between contexts, the direction of influence, and the mechanisms for translating between different domain models. The oncology domain uses multiple integration patterns to manage the complex web of relationships between its six bounded contexts and the numerous external systems that support cancer care delivery.

## DDD Integration Patterns Applied to Oncology

### Customer-Supplier Pattern

The customer-supplier pattern is the most prevalent integration pattern in the oncology domain. In this pattern, the upstream (supplier) context produces data or events that the downstream (customer) context consumes. The supplier is aware of the customer's needs and may accommodate reasonable requests, but the supplier's model is not dictated by the customer.

In oncology, the Treatment Planning Context is a supplier to the three treatment modality contexts (Chemotherapy Management, Radiation Therapy, Surgical Oncology). The Treatment Planning Context publishes approved treatment plans, and each modality context consumes the portions relevant to its domain. The modality contexts can request that the Treatment Planning Context include specific information in its published events, but the Treatment Planning Context retains ownership of its model.

The Diagnosis and Staging Context is a supplier to the Treatment Planning Context. When a diagnosis is confirmed, the staging and molecular profiling data are published for the Treatment Planning Context to consume. The Treatment Planning Context can request that specific molecular markers be included in the diagnostic output, but the Diagnosis and Staging Context determines how its model represents diagnostic information.

### Published Language Pattern

The published language pattern establishes a shared, well-documented model that multiple contexts agree to use for communication. In oncology, several clinical standards serve as published languages.

TNM staging is a published language that the Diagnosis and Staging Context produces and all other contexts understand. When a stage of T2N1M0 Stage IIB is communicated, all contexts share a common understanding of what this means clinically. The TNM system is defined by an external authority (AJCC) and adopted by all contexts.

NCCN guidelines serve as a published language for treatment recommendations. The Treatment Planning Context references specific NCCN pathways, and the treatment modality contexts understand these references because the guidelines are externally defined and clinically standardized.

CTCAE grading serves as a published language for adverse event reporting. The Chemotherapy Management Context uses CTCAE terms and grades, and the Treatment Planning Context and Survivorship Care Context understand this grading system without translation.

### Conformist Pattern

In the conformist pattern, the downstream context adopts the upstream context's model without attempting to maintain its own independent model for the shared concepts. The Survivorship Care Context uses a partial conformist pattern when assembling treatment summaries. It conforms to the treatment representation used by each modality context, directly incorporating their treatment records into the survivorship care plan's treatment summary without imposing its own model.

This is a pragmatic choice. The Survivorship Care Context's primary value is in the surveillance and monitoring domain, not in remodeling treatment data. Conforming to the treatment contexts' models reduces translation complexity and ensures treatment summaries accurately reflect what was documented during active treatment.

### Anti-Corruption Layer Pattern

The anti-corruption layer pattern is used when integrating with external systems whose models could contaminate the oncology domain model. Each ACL translates between the external system's model and the oncology domain's model. Key ACL integrations in oncology include the laboratory system ACL, pharmacy system ACL, imaging system ACL, tumor registry ACL, and electronic health record ACL.

### Shared Kernel Pattern

The shared kernel pattern establishes a small, shared model that two or more contexts agree to maintain together. Changes to the shared kernel require agreement from all participating contexts.

In oncology, the patient identity (medical record number) is a shared kernel element. All six bounded contexts reference patients using the same identity, and changes to the patient identity scheme require coordination across all contexts. The cancer diagnosis identifier is another shared kernel element, ensuring that all contexts reference the same diagnosed cancer when coordinating treatment.

The shared kernel should be kept as small as possible. Only concepts that truly must be identical across contexts belong in the shared kernel. Overuse of the shared kernel reintroduces the coupling that bounded contexts are designed to eliminate.

### Open Host Service Pattern

The open host service pattern exposes a context's capabilities through a well-defined API that can be consumed by multiple other contexts. In oncology, the Diagnosis and Staging Context may expose an open host service that allows any context to query the current staging status for a patient. The Treatment Planning Context may expose an open host service for retrieving the current active treatment plan.

Open host services complement event-driven communication. Events handle asynchronous notifications of state changes, while open host services handle synchronous queries for current state.

## External System Integration

### Laboratory Information Systems

Integration with laboratory systems uses the ACL pattern with HL7 FHIR or HL7 v2 messaging. The ACL translates generic lab results into typed oncology value objects (biomarker results, tumor markers, complete blood counts for treatment eligibility).

### Pharmacy Systems

Integration with pharmacy systems uses the ACL pattern to translate between oncology regimen orders and pharmacy dispensing orders. The ACL maps oncology drug identifiers to NDC codes and translates dose calculations into dispensing quantities.

### Imaging Systems

Integration with imaging systems uses the ACL pattern with DICOM standards. The ACL extracts clinically relevant information from imaging studies and translates it into oncology domain concepts (tumor measurements, response assessments).

### Tumor Registries

Integration with tumor registries uses the ACL pattern to translate rich oncology domain data into coded registry formats (ICD-O codes, NAACCR data items). This is a lossy translation that the ACL manages explicitly.

## Pattern Selection Guidelines

Choosing the right integration pattern depends on the relationship between contexts. When one context clearly serves another's needs, use customer-supplier. When contexts share well-defined external standards, use published language. When the downstream context has no need to maintain an independent model, use conformist. When external systems could contaminate the domain model, use anti-corruption layer. When a small set of concepts must be identical across contexts, use shared kernel.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 14: Maintaining Model Integrity.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 3: Context Maps.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 7: Context Mapping Patterns.
- Hohpe, Gregor and Woolf, Bobby. Enterprise Integration Patterns. Addison-Wesley, 2003.
