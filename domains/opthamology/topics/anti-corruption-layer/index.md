# Anti-Corruption Layer in Ophthalmology

## Overview

An anti-corruption layer (ACL) is a translation boundary that protects a bounded context
from the concepts, terminology, and data models of external systems. In the ophthalmology
domain, ACLs are essential because ophthalmology systems must integrate with numerous
external systems including electronic health records (EHR), picture archiving and
communication systems (PACS), billing platforms, laboratory information systems, and
pharmacy management systems. Without ACLs, external system models would leak into the
ophthalmology domain model, creating tight coupling and compromising domain integrity.

## ACL Responsibilities

An anti-corruption layer in the ophthalmology domain performs several functions:

- Translates external data formats into the ophthalmology domain model. For example,
  converting HL7 FHIR Observation resources into the domain's IntraocularPressure
  value object.
- Maps external terminology to the ubiquitous language. External systems may use
  different coding schemes (SNOMED CT, LOINC) that must be mapped to the domain's
  own terminology.
- Filters irrelevant data. An EHR system may return a patient's complete medical
  history, but the ophthalmology domain model only needs eye-related information.
- Handles protocol differences. External systems may use REST, SOAP, HL7 v2 messages,
  or DICOM protocols that differ from the domain's internal communication patterns.

## ACL Placement in Ophthalmology

### EHR Integration ACL

All bounded contexts interact with external EHR systems through a shared ACL that
translates between the ophthalmology domain model and HL7 FHIR or HL7 v2 data formats.
This ACL converts FHIR Patient resources into the domain's patient identity
representation, FHIR Encounter resources into the domain's examination model, and
FHIR DiagnosticReport resources into the domain's imaging study results.

### PACS Integration ACL

The Diagnostic Imaging Context uses an ACL to translate between DICOM-formatted imaging
data and the domain's Imaging Study aggregate. This ACL handles DICOM metadata
extraction, study-series-instance hierarchy mapping, and image reference management.
The ACL ensures that DICOM-specific concepts (SOP Class, Transfer Syntax, Modality
Worklist) do not leak into the domain model.

### Billing System ACL

The Clinical Examination Context and Surgical Management Context use an ACL to translate
clinical encounters and surgical procedures into billing-compatible formats. The ACL
maps domain events (ExaminationCompleted, SurgicalCaseCompleted) into CPT procedure
codes and ICD-10 diagnosis codes for submission to billing systems.

### Pharmacy System ACL

The Glaucoma Management Context and Retinal Care Context use an ACL to interact with
pharmacy management systems for medication management. The ACL translates between the
domain's Treatment Plan aggregate and pharmacy system prescription formats, handling
drug formulary lookups, dosage calculations, and refill scheduling.

### Laboratory System ACL

The Clinical Examination Context uses an ACL to receive laboratory results (e.g.,
HbA1c for diabetic retinopathy screening, blood glucose levels) from laboratory
information systems. The ACL translates laboratory data formats into the domain's
value objects.

## Design Principles

ACLs in the ophthalmology domain follow these principles:

- The ACL is owned by the consuming bounded context. The context that needs external
  data is responsible for building and maintaining the ACL.
- The ACL is stateless. It performs translation but does not maintain its own state
  or business logic.
- The ACL isolates failure. If an external system is unavailable, the ACL provides
  graceful degradation without corrupting the domain model.
- The ACL is testable. Translation logic is unit-testable with mock external data.
- The ACL is versioned. As external system APIs evolve, the ACL adapts without
  requiring changes to the domain model.

## ACL Implementation Considerations

- Adapter pattern: each external system integration is encapsulated in an adapter
  that implements a domain-defined interface.
- Facade pattern: complex external system interactions are simplified behind a
  domain-friendly facade.
- Event translation: external system events are translated into domain events at
  the ACL boundary.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 14.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 3.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 9.
- HL7 International. *HL7 FHIR Specification*. https://www.hl7.org/fhir/
