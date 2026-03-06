# Integration Patterns in Medical Practice

## Overview

Integration patterns define how bounded contexts communicate, share data, and maintain consistency without violating each other's model integrity. In Domain-Driven Design, several well-established patterns govern inter-context relationships. In medical practice, these patterns are critical because clinical, pharmacy, insurance, and diagnostic systems must exchange data reliably while respecting each context's autonomy, privacy constraints, and regulatory requirements.

## Core Integration Patterns

### Customer-Supplier

In the customer-supplier pattern, the upstream context (supplier) provides data or services that the downstream context (customer) depends on. The upstream context accommodates the downstream context's needs within reason.

Medical examples:

- **Patient Records (supplier) -> Clinical Workflow (customer)**: Clinical Workflow depends on patient identity, allergies, and problem lists. Patient Records publishes events that Clinical Workflow subscribes to.
- **Clinical Workflow (supplier) -> Insurance & Claims (customer)**: Insurance depends on encounter data (diagnoses, procedures) to generate claims. Clinical Workflow publishes EncounterCompleted events.
- **Patient Records (supplier) -> Telemedicine (customer)**: Telemedicine depends on patient demographics and contact information for scheduling and session delivery.

### Partnership

In a partnership, two bounded contexts have a mutual dependency and jointly coordinate their development and integration. Neither context is purely upstream or downstream.

Medical examples:

- **Clinical Workflow <-> Pharmacy & Medication**: The clinical context sends medication orders; the pharmacy context sends back interaction alerts. Both contexts must agree on a shared medication vocabulary and coordinate changes.
- **Clinical Workflow <-> Telemedicine**: Virtual visits in Telemedicine map to encounters in Clinical Workflow. Both contexts share encounter lifecycle events and must coordinate changes to visit/encounter models.

### Shared Kernel

A shared kernel is a small, explicitly shared subset of the domain model that two or more bounded contexts agree to maintain together. Changes to the shared kernel require agreement from all participating teams.

Medical examples:

- **Patient identity kernel**: A minimal shared model containing MRN, patient name, and DOB used by all contexts for patient identification
- **Provider identity kernel**: A shared model containing provider NPI, name, and specialty used by Clinical Workflow, Insurance, and Pharmacy contexts

Shared kernels should be kept as small as possible to minimize coupling.

### Published Language

A published language is a well-documented, versioned data format used for communication between bounded contexts. It serves as a contract that both parties agree to.

Medical examples:

- **FHIR resources**: HL7 FHIR resource definitions serve as a published language for patient, encounter, medication, and observation data
- **EDI transaction sets**: X12 837/835/270/271 formats serve as the published language between medical practices and insurance payers
- **DICOM**: Serves as the published language for imaging data exchange

### Conformist

In the conformist pattern, the downstream context adopts the upstream context's model without translation, typically because the upstream context is external and non-negotiable.

Medical examples:

- **Insurance & Claims -> External Payers**: The practice must conform to each payer's claim submission requirements, denial codes, and adjudication formats
- **Diagnostics -> Reference Lab Systems**: External labs define their own result formats; the diagnostics context may conform to their HL7 v2 message structure

### Open Host Service

An open host service exposes a bounded context's capabilities through a well-defined protocol or API that multiple consumers can use.

Medical examples:

- **Patient Records Open Host**: Exposes FHIR-based Patient, Condition, and AllergyIntolerance endpoints that any authorized context can query
- **Diagnostics Open Host**: Exposes FHIR DiagnosticReport and Observation endpoints for result retrieval

### Anti-Corruption Layer

An anti-corruption layer translates between an external system's model and the internal domain model, preventing external concepts from contaminating the domain.

Medical examples:

- **HL7 v2 ACL**: Translates HL7 v2 messages from external labs into internal LabResult value objects
- **EDI ACL**: Translates X12 835 remittance advice into internal AdjudicationResult value objects
- **DICOM ACL**: Translates DICOM metadata into internal ImagingStudy entities

## Pattern Selection Guidance

| Scenario | Recommended Pattern |
|----------|-------------------|
| Internal context depends on another internal context | Customer-Supplier |
| Two internal contexts have mutual dependencies | Partnership |
| Multiple contexts need the same minimal data | Shared Kernel |
| Contexts agree on a formal data exchange format | Published Language |
| External system dictates the format and there is no negotiation | Conformist |
| A context exposes its data to multiple consumers | Open Host Service |
| External system model would corrupt internal domain | Anti-Corruption Layer |

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) -- Integration patterns connect bounded contexts
- [Context Map](../context-map/) -- The context map documents which patterns are used between contexts
- [Anti-Corruption Layer](../anti-corruption-layer/) -- A specific integration pattern for external system translation
- [Domain Events](../domain-events/) -- Events are the primary mechanism for customer-supplier integration
- [Event-Driven Architecture](../event-driven-architecture/) -- The architectural pattern underlying many integration approaches
- [Medical Standards](../medical-standards/) -- Standards like FHIR, HL7 v2, and DICOM serve as published languages

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 14. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 3. Addison-Wesley, 2013.
- Millett, Scott & Tune, Nick. "Patterns, Principles, and Practices of Domain-Driven Design." Chapter 8. Wrox, 2015.
- Hohpe, Gregor & Woolf, Bobby. "Enterprise Integration Patterns." Addison-Wesley, 2003.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 8. O'Reilly, 2021.
