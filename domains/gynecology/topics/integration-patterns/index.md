# Integration Patterns in the Gynecology Domain

## Overview

Integration patterns define how bounded contexts communicate, share data, and coordinate behavior. Domain-Driven Design identifies several canonical integration patterns that apply to different types of relationships between contexts. In the gynecology domain, these patterns govern how clinical assessment, reproductive health, surgical services, prenatal care, screening programs, and patient education contexts interact with each other and with external systems.

## Purpose

The six bounded contexts in the gynecology domain cannot operate in isolation. Clinical workflows require data and decisions to flow between contexts. Integration patterns provide structured approaches to managing these flows, ensuring that each context maintains its model integrity while supporting the overall clinical workflow.

## Integration Patterns Applied

### Shared Kernel

A shared kernel is a small, explicitly shared subset of the domain model that two or more contexts agree to maintain together.

In the gynecology domain:

- Clinical Assessment Context and Prenatal Care Context share a kernel of patient demographic value objects (PatientName, DateOfBirth, MedicalRecordNumber) and basic clinical history value objects (AllergyList, MedicationList).
- Changes to the shared kernel require agreement from both contexts.
- The shared kernel is kept intentionally small to minimize coupling.

### Published Language

A published language is a well-documented, versioned data format used for communication between contexts.

In the gynecology domain:

- Clinical Assessment Context and Prenatal Care Context use a published language for pregnancy-related clinical findings, defining standard event payloads with explicit schemas.
- Screening Programs Context uses a published language for screening results based on standardized classification systems (Bethesda, BI-RADS).
- Published languages are versioned to allow contexts to evolve independently while maintaining backward compatibility.

### Open Host Service

An open host service exposes a bounded context's capabilities through a standardized, well-documented interface that multiple consumers can use.

In the gynecology domain:

- Screening Programs Context exposes an open host service for test ordering and result retrieval, allowing Clinical Assessment, Prenatal Care, and Reproductive Health contexts to order screening tests through a uniform interface.
- Patient Education Context exposes an open host service for educational content requests, allowing any clinical context to request appropriate educational materials for a patient.

### Customer-Supplier

In a customer-supplier relationship, the upstream context (supplier) provides data or services that the downstream context (customer) depends on. The upstream context accommodates the downstream context's needs.

In the gynecology domain:

- Clinical Assessment Context (supplier) provides diagnostic impressions to Reproductive Health Context (customer) and Surgical Services Context (customer).
- Screening Programs Context (supplier) provides abnormal result notifications to Clinical Assessment Context (customer).
- The supplier context commits to maintaining the stability and quality of published events.

### Conformist

In a conformist relationship, the downstream context adopts the upstream context's model without requesting modifications.

In the gynecology domain:

- Patient Education Context conforms to the event models published by all clinical contexts. It does not request changes to how Clinical Assessment, Surgical Services, or Screening Programs publish their events. Instead, it translates incoming events through its own anti-corruption layer.

### Anti-Corruption Layer

An anti-corruption layer translates between an external model and the internal bounded context model.

In the gynecology domain:

- Each bounded context uses an ACL when integrating with external systems (laboratory information systems, radiology platforms, EHR systems, pharmacy systems).
- Patient Education Context uses an ACL to translate clinical events from upstream contexts into educational trigger categories.

## External System Integration

### Laboratory Information Systems

- Pattern: Anti-corruption layer with published language.
- The Screening Programs Context translates LOINC-coded test orders and results into domain-specific screening value objects.

### Radiology and Ultrasound Systems

- Pattern: Anti-corruption layer.
- The Prenatal Care Context translates DICOM imaging data and HL7 reports into domain-specific ultrasound assessment value objects.

### Electronic Health Record Systems

- Pattern: Anti-corruption layer per bounded context.
- Each context maintains its own ACL to translate between the EHR's data model and its domain model.

### Pharmacy Systems

- Pattern: Anti-corruption layer with open host service.
- The Reproductive Health Context integrates with pharmacy systems for contraceptive prescriptions, translating NDC/RxNorm codes into domain medication value objects.

## Integration Governance

- All integration interfaces are versioned and documented.
- Breaking changes to published languages require coordinated migration plans.
- Shared kernel changes require joint approval from all participating context teams.
- Anti-corruption layers are tested with contract tests that verify translation correctness.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 14 on integration patterns.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 3 on context mapping and integration.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapters 8-9 on integration patterns.
- Hohpe, Gregor and Bobby Woolf. *Enterprise Integration Patterns*. Addison-Wesley, 2003.
