# Integration Patterns in the Orthopaedic Domain

## Overview

Integration patterns define how bounded contexts communicate and share data with each
other and with external systems. In the orthopaedic domain, integration patterns govern
how clinical assessment data flows to surgical planning, how implant data reaches
registries, and how external hospital systems interact with orthopaedic contexts.

## Purpose

The orthopaedic domain must integrate with numerous systems: hospital information
systems, imaging archives, laboratory systems, national registries, and pharmacy
systems. Integration patterns provide structured approaches to these connections,
ensuring that each bounded context maintains its model integrity while exchanging
necessary information.

## Inter-Context Integration Patterns

### Published Language

Bounded contexts in the orthopaedic domain communicate through a published language:
a well-documented, versioned schema for domain events and shared data structures. The
published language defines the event payloads that flow between contexts.

Examples:
- The PatientAssessed event schema defines the structure of diagnostic findings.
- The ArthroplastyCompleted event schema defines the structure of implant details.
- The FractureClassified event schema defines the AO/OTA code format.

The published language is owned by the publishing context but designed with consumer
needs in mind.

### Open Host Service

Each bounded context exposes an open host service: a well-defined interface through
which other contexts can query or interact with it. The service uses the published
language and provides a stable contract for consumers.

Examples:
- Clinical Assessment exposes a service for querying patient assessment history.
- Joint Replacement exposes a service for querying implant status by serial number.
- Fracture Management exposes a service for querying fracture healing status.

### Customer-Supplier

Several relationships in the orthopaedic domain follow the customer-supplier pattern,
where the upstream supplier accommodates the downstream customer's data needs.

- Clinical Assessment (supplier) --> Surgical Planning (customer).
- Joint Replacement (supplier) --> Rehabilitation (customer).
- Fracture Management (supplier) --> Rehabilitation (customer).

The supplier publishes events and services that the customer depends on. Changes to
the supplier's interface require coordination with the customer.

### Partnership

Some orthopaedic contexts operate as partners with shared goals and mutual dependency.

- Surgical Planning <--> Joint Replacement: Implant selection affects both planning
  workflows and registry data. Both teams coordinate closely.
- Surgical Planning <--> Fracture Management: Operative fracture treatment requires
  coordination between planning and fixation execution.

### Conformist

The Joint Replacement Context adopts a conformist relationship with national joint
registries. The context conforms to the registry's data model and submission
requirements because the registry's standards are non-negotiable.

## External System Integration Patterns

### Anti-Corruption Layer for PACS

DICOM imaging data is translated through an ACL into the Clinical Assessment
Context's imaging model. The ACL handles DICOM metadata parsing, series identification,
and patient matching.

### Anti-Corruption Layer for EHR/HIS

HL7 v2 messages and FHIR resources are translated through an ACL into
context-specific patient representations. The ACL handles patient demographics,
clinical orders, and result reporting.

### Anti-Corruption Layer for Laboratory Systems

Lab results arriving via HL7 are translated through an ACL into pre-operative
assessment value objects within the Surgical Planning Context.

### Shared Kernel (Limited Use)

A shared kernel may be used for cross-cutting concepts that are truly shared:
- Patient identifier mapping between contexts.
- Anatomical reference data (bone names, laterality codes).
- Unit of measurement standards.

The shared kernel is kept as small as possible to minimize coupling.

## Integration Governance

- All integration interfaces are versioned and backward-compatible.
- Breaking changes require coordinated deployment across affected contexts.
- Integration contracts are validated with automated contract tests.
- External system changes are absorbed by the ACL, not the domain model.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 14.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 13.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapters 8-9.
- Hohpe, Gregor and Woolf, Bobby. Enterprise Integration Patterns. Addison-Wesley, 2003.
