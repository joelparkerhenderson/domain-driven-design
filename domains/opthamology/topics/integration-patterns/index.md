# Integration Patterns in Ophthalmology

## Overview

Integration patterns define how bounded contexts communicate and share data within
the ophthalmology domain and with external systems. Domain-Driven Design identifies
several integration patterns that characterize the nature of the relationship between
contexts, ranging from tightly coupled shared kernels to loosely coupled published
languages. In the ophthalmology domain, integration pattern selection is driven by
clinical workflow requirements, data ownership principles, and the degree of coupling
that is acceptable between contexts.

## Internal Integration Patterns

### Customer-Supplier

The customer-supplier pattern is the most common integration pattern in the
ophthalmology domain. In this pattern, the downstream (customer) context defines
what it needs, and the upstream (supplier) context commits to providing it.

Examples in ophthalmology:

- Clinical Examination (supplier) to Diagnostic Imaging (customer): the Diagnostic
  Imaging Context defines its requirements for examination data that accompanies
  imaging orders. The Clinical Examination Context commits to publishing
  ExaminationCompleted events with the required data.
- Diagnostic Imaging (supplier) to Surgical Management (customer): the Surgical
  Management Context defines its requirements for biometry and topography data.
  The Diagnostic Imaging Context commits to delivering interpreted results in the
  agreed format.
- Clinical Examination (supplier) to Glaucoma Management (customer): the Glaucoma
  Management Context requires IOP readings and optic nerve assessments in a specific
  format for longitudinal tracking.

### Conformist

In the conformist pattern, the downstream context accepts the upstream context's model
without negotiation. This pattern is used when the upstream context provides
standardized data that the downstream context adapts to.

Examples in ophthalmology:

- Diagnostic Imaging (upstream) to Glaucoma Management (conformist): the Glaucoma
  Management Context conforms to the OCT RNFL analysis format produced by the
  Diagnostic Imaging Context. The imaging data format is driven by device
  manufacturers and DICOM standards.
- Diagnostic Imaging (upstream) to Retinal Care (conformist): the Retinal Care
  Context conforms to the OCT macular thickness and angiography formats.

### Shared Kernel

The shared kernel pattern involves two contexts sharing a small, carefully managed
subset of the domain model. Changes to the shared kernel require agreement from both
context teams.

Examples in ophthalmology:

- Surgical Management and Refractive Services share a kernel of perioperative
  management concepts including surgical consent models, sterile technique protocols,
  and post-operative assessment standards. This shared kernel is kept minimal and
  changes require coordination between both teams.

### Published Language

The published language pattern provides a well-documented, shared data format for
communication between contexts. Unlike a shared kernel, the published language does
not share implementation; it shares data format specifications.

Examples in ophthalmology:

- Eye laterality (OD/OS/OU) is communicated using a published language that all
  contexts agree on, preventing ambiguity in left/right eye designation.
- Patient identity is exchanged using a published language that specifies identifier
  format, name representation, and date of birth encoding.
- Clinical findings are exchanged using published language based on clinical coding
  standards (ICD-10, SNOMED CT).

### Open Host Service

The open host service pattern provides a standardized protocol for external consumers
to access a context's capabilities. The hosting context publishes a service interface
that any authorized consumer can use.

Examples in ophthalmology:

- The Diagnostic Imaging Context exposes DICOM-compliant interfaces as an open host
  service, allowing PACS systems, third-party viewers, and research platforms to
  access imaging data using standard protocols.
- The Clinical Examination Context exposes HL7 FHIR-compliant interfaces for EHR
  integration, allowing external health information systems to query examination data.

## External Integration Patterns

### EHR Integration

Integration with electronic health record systems uses anti-corruption layers to
translate between the ophthalmology domain model and EHR data standards (HL7 FHIR,
HL7 v2, CDA). The ACL handles bidirectional data flow: pushing ophthalmology clinical
data to the EHR and pulling patient demographics and medical history from the EHR.

### PACS Integration

Integration with picture archiving and communication systems uses DICOM as the
published language. The Diagnostic Imaging Context acts as a DICOM modality worklist
consumer and image storage provider, using standard DICOM services (C-STORE, C-FIND,
C-MOVE) for image exchange.

### Billing Integration

Integration with billing systems translates clinical encounters and procedures into
billing-compatible formats. The ACL maps domain concepts to CPT procedure codes and
ICD-10 diagnosis codes, handles modifier assignment, and manages claim submission
data preparation.

### Laboratory Integration

Integration with laboratory information systems receives test results (HbA1c, blood
glucose, infectious disease screening) relevant to ophthalmology care. The ACL
translates laboratory result formats into domain value objects.

### Pharmacy Integration

Integration with pharmacy systems manages medication prescriptions and refills for
glaucoma medications and other ophthalmic pharmaceuticals. The ACL translates between
the domain's treatment plan model and pharmacy prescription formats.

## Integration Governance

Integration patterns are documented in the context map and reviewed quarterly. Changes
to integration patterns require agreement from all affected bounded context teams and
must be reflected in updated integration contracts and documentation.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 14.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapters 3, 13.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 9.
- Hohpe, Gregor, and Bobby Woolf. *Enterprise Integration Patterns*. Addison-Wesley, 2003.
