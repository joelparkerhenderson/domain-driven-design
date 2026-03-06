# Integration Patterns - Otolaryngology Domain

## Overview

Integration patterns define how bounded contexts communicate and share information. In the otolaryngology domain, integration patterns govern both inter-context communication (between the six ENT bounded contexts) and external system integration (with EHR, laboratory, billing, and imaging systems). The choice of integration pattern at each boundary reflects the relationship between the teams and systems involved.

## Inter-Context Integration Patterns

### Customer-Supplier

The customer-supplier pattern describes an upstream-downstream relationship where the downstream context (customer) depends on data produced by the upstream context (supplier). The supplier accommodates the customer's needs within reasonable constraints.

**Clinical Assessment (Supplier) to All Subspecialty Contexts (Customers)**

Clinical Assessment produces diagnostic findings consumed by all downstream contexts. As the supplier, Clinical Assessment publishes standardized clinical data that downstream contexts translate into their own models. The supplier commits to publishing encounter completion events with consistent structure and content.

Customer-specific accommodations:
- Surgical Management requires imaging study results in a format compatible with surgical planning.
- Voice Disorders requires laryngoscopic findings with stroboscopic detail.
- Sleep Disorders requires airway measurements (Mallampati, tonsil size, neck circumference).
- Allergy Management requires nasal endoscopy findings with mucosal detail.
- Hearing Services requires audiometric screening results with calibration metadata.

### Partnership

The partnership pattern describes two contexts that evolve together with mutual coordination, neither being upstream or downstream.

**Surgical Management and Hearing Services (Partnership)**

These contexts collaborate closely for cochlear implant and ear surgery cases. Pre-operative audiological data informs surgical planning, and surgical outcomes drive post-operative audiological management. Both teams coordinate their models to ensure consistent patient care across the surgical and rehabilitation phases.

**Surgical Management and Voice Disorders (Partnership)**

Similarly, these contexts partner for phonosurgery cases. Pre-operative voice assessment informs surgical planning, and surgical outcomes drive post-operative voice rehabilitation. Both teams maintain shared understanding of voice outcome measures.

**Sleep Disorders and Allergy Management (Partnership)**

These contexts partner for patients with concurrent sleep apnea and allergic rhinitis, where nasal obstruction from allergic disease contributes to sleep-disordered breathing. Both teams coordinate treatment to address the interrelated conditions.

### Shared Kernel

The shared kernel pattern defines a small, shared model that two contexts use in common, maintained jointly by both teams.

**Voice Disorders and Hearing Services (Shared Kernel)**

These contexts share a small kernel of communication assessment concepts. For patients with combined voice and hearing disorders, both contexts need a shared understanding of communication ability, patient communication goals, and quality of life measures. The shared kernel includes communication assessment value objects and patient communication profile concepts.

### Conformist

The conformist pattern describes a downstream context that conforms entirely to an upstream model without any translation, typically when the upstream model is a standard that cannot be negotiated.

**All Contexts to Billing/Coding Standards (Conformist)**

All bounded contexts conform to CPT procedure codes, ICD-10 diagnosis codes, and HCPCS codes. These coding standards are externally defined and non-negotiable. Each context maps its domain events to appropriate codes without attempting to influence the coding system design.

### Published Language

The published language pattern defines a shared, documented data exchange format used for inter-context communication.

**HL7 FHIR as Published Language**

HL7 FHIR resources serve as the published language for data exchange with external systems. FHIR provides standardized resource types (Patient, Encounter, Observation, Procedure, DiagnosticReport) that serve as the lingua franca for healthcare data exchange.

**SNOMED CT as Published Language**

SNOMED CT clinical terminology serves as a published language for clinical concept representation across contexts. When contexts need to communicate clinical findings, SNOMED CT codes provide unambiguous concept identification.

### Open Host Service

The open host service pattern provides a well-defined protocol through which external consumers can access a context's capabilities.

**Clinical Assessment as Open Host Service**

The Clinical Assessment Context exposes its diagnostic findings through a well-defined event publication protocol. Any new bounded context added to the system can subscribe to Clinical Assessment events using the documented event schema, without requiring changes to the Clinical Assessment Context.

## External System Integration Patterns

### EHR Integration

Pattern: Anti-corruption layer with published language (HL7 FHIR).
Direction: Bidirectional. Domain events are translated to FHIR resources for outbound communication; FHIR resources from external systems are translated to domain objects for inbound consumption.
Key translations: FHIR Encounter to ENT Encounter, FHIR Observation to domain-specific findings, FHIR Procedure to Surgical Case.

### Laboratory Information System

Pattern: Anti-corruption layer with messaging (HL7 v2 ORU messages).
Direction: Inbound. Laboratory results are received and translated into domain-specific allergy test result value objects within the Allergy Management Context.
Key translations: ORU result messages to Serum IgE Level value objects with allergen-specific interpretation.

### PACS (Imaging Archive)

Pattern: Anti-corruption layer with standard protocol (DICOM).
Direction: Bidirectional. Imaging study references are maintained in the domain model; actual images remain in the external PACS.
Key translations: DICOM metadata to Imaging Study Reference value objects.

### Billing Systems

Pattern: Conformist with anti-corruption layer.
Direction: Outbound. Domain events are translated into billable encounters with CPT and ICD-10 codes.
Key translations: Domain procedure and diagnosis concepts to standardized billing codes.

## Integration Anti-Patterns to Avoid

- **Direct database sharing**: Bounded contexts must not share databases, as this creates hidden coupling.
- **Synchronous cross-context calls**: Prefer asynchronous event-driven communication to avoid cascading failures.
- **Model leakage**: External system data structures (FHIR, HL7, DICOM) must not appear in the domain model.
- **Big ball of mud integration**: Avoid point-to-point integrations that create an unmaintainable web of dependencies.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 14 on integration patterns.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 13 on bounded context integration.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapters 8-9 on integration patterns.
- Hohpe, Gregor, and Bobby Woolf. *Enterprise Integration Patterns*. Addison-Wesley, 2003.
