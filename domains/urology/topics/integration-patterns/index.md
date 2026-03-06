# Integration Patterns in the Urology Domain

## Overview

Integration patterns define how bounded contexts communicate and share information. In the urology domain, six bounded contexts and multiple external systems must exchange clinical data while preserving model integrity within each context. Domain-Driven Design provides a vocabulary of integration patterns that describe the nature of these relationships and guide their implementation.

## Published Language

Published language establishes a shared, well-documented data format for communication between contexts. In the urology domain, HL7 FHIR (Fast Healthcare Interoperability Resources) serves as the published language for clinical data exchange. FHIR resources such as Patient, Condition, Procedure, DiagnosticReport, and Observation provide a standardized vocabulary for expressing urological clinical data. Each bounded context translates its internal model to and from FHIR resources at its boundary.

### Urology-Specific FHIR Profiles

The urology domain defines FHIR profiles that constrain generic resources for urological use. A Prostate Biopsy DiagnosticReport profile requires Gleason score components. A Urodynamic Study DiagnosticReport profile requires specific observation types (Qmax, PdetQmax, bladder compliance). These profiles constitute the urology domain's published language, enabling interoperability while preserving clinical specificity.

## Open Host Service

An open host service exposes a bounded context's capabilities through a well-defined protocol that multiple consumers can use. The Clinical Assessment Context implements an open host service that provides diagnostic profiles to any treatment context. The service exposes operations such as GetDiagnosticProfile, GetLatestImaging, and GetSymptomScoreHistory. Multiple downstream contexts (Oncologic, Stone Disease, Incontinence, Male Reproductive Health) consume this service without requiring bilateral agreements.

## Shared Kernel

A shared kernel is a small, shared model that two or more bounded contexts agree to maintain jointly. In the urology domain, a shared kernel exists between the Oncologic Urology Context and the Surgical Management Context for cancer staging terminology. Both contexts must agree on TNM stage definitions, ISUP Grade Group classifications, and margin status categories. Changes to the shared kernel require agreement from both contexts' teams, and the kernel is versioned to manage evolution.

### Kernel Boundaries

The shared kernel is deliberately minimal. It includes staging classification value objects and procedure indication codes, but it does not include treatment algorithms, risk calculators, or outcome models, which remain private to their respective contexts. The kernel represents only the vocabulary that both contexts must agree on for their interface to function.

## Customer-Supplier

In a customer-supplier relationship, the downstream context (customer) depends on the upstream context (supplier), and the supplier accommodates the customer's needs. The Clinical Assessment Context is a supplier to all treatment contexts. The treatment contexts communicate their diagnostic data requirements, and the Clinical Assessment Context ensures its published diagnostic profiles include the necessary information.

### Power Dynamics

The customer-supplier pattern acknowledges power dynamics. If the Clinical Assessment Context changes its diagnostic profile format, all downstream treatment contexts are affected. The supplier team must consider the impact on customers before making changes, and customers should have a voice in the supplier's roadmap. This is managed through interface versioning and deprecation policies.

## Conformist

In a conformist relationship, the downstream context adopts the upstream context's model without modification. The urology domain maintains conformist relationships with several external standards bodies. All bounded contexts conform to the AJCC TNM staging system for cancer classification without modification. The Stone Disease Context conforms to the WHO crystallographic classification for stone composition. The Clinical Assessment Context conforms to ICS (International Continence Society) standardization of urodynamic terminology.

## Anti-Corruption Layer

Anti-corruption layers protect bounded contexts from external model contamination. Each urology bounded context implements ACLs for its external integrations. The Oncologic Context's ACL translates pathology system reports into structured oncologic domain objects. The Clinical Assessment Context's ACL translates radiology reports into urological imaging findings. The Stone Disease Context's ACL translates laboratory results into metabolic risk profiles. ACLs are detailed in the dedicated anti-corruption layer topic.

## Partnership

In a partnership, two bounded contexts coordinate closely because they have mutual dependencies. The Clinical Assessment Context and Surgical Management Context maintain a partnership. Clinical Assessment provides pre-operative diagnostic data, and Surgical Management feeds operative findings back. Both teams must coordinate development schedules and interface changes, as modifications on either side affect the other.

## Separate Ways

Some bounded contexts do not need to integrate at all. In the urology domain, the Stone Disease Context and the Male Reproductive Health Context operate on separate ways for most clinical scenarios. They serve different patient populations with different clinical problems and do not exchange domain events or share data. When rare overlap occurs (a stone patient who also has infertility), coordination happens at the patient level through the Clinical Assessment Context rather than through direct integration.

## Integration Testing

Integration between urology bounded contexts requires testing at the boundaries. Contract tests verify that the published interfaces match what consumers expect. Consumer-driven contract testing allows downstream contexts to specify their expectations, and upstream contexts verify they fulfill those contracts. Integration tests verify end-to-end event flows across context boundaries.

## Versioning Strategy

As bounded contexts evolve, their interfaces change. The urology domain uses semantic versioning for published interfaces. Breaking changes require a major version increment and a deprecation period during which both old and new versions are supported. This allows consuming contexts to migrate on their own schedule rather than requiring coordinated deployments.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 14.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 3.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapters 8-9.
- HL7 International. FHIR Specification. https://www.hl7.org/fhir/
- Hohpe, Gregor and Woolf, Bobby. Enterprise Integration Patterns. Addison-Wesley, 2003.
