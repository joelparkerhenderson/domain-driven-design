# Context Map for Ophthalmology

## Overview

A context map is a visual and conceptual representation of the relationships and
interactions between bounded contexts within a domain. In the ophthalmology domain,
the context map documents how the six bounded contexts communicate, which contexts
are upstream or downstream, and what integration patterns govern their interactions.
The context map is a living artifact maintained collaboratively by domain experts and
development teams.

## Context Relationships

### Clinical Examination to Diagnostic Imaging

Relationship: Customer-Supplier.
The Clinical Examination Context is the customer, requesting imaging studies based on
examination findings. The Diagnostic Imaging Context is the supplier, fulfilling imaging
orders and returning interpreted results. The Clinical Examination Context defines what
it needs from imaging, and the Diagnostic Imaging Context commits to delivering it.

### Clinical Examination to Surgical Management

Relationship: Customer-Supplier.
The Surgical Management Context consumes pre-operative examination data from the Clinical
Examination Context. Examination findings such as visual acuity, lens opacity grading,
and biometry measurements are essential inputs to operative planning. The Clinical
Examination Context provides these through published domain events.

### Clinical Examination to Glaucoma Management

Relationship: Customer-Supplier.
The Glaucoma Management Context consumes tonometry readings and optic nerve assessments
from the Clinical Examination Context. These measurements feed into the longitudinal
Glaucoma Patient Profile for progression analysis.

### Clinical Examination to Retinal Care

Relationship: Customer-Supplier.
The Retinal Care Context consumes fundoscopic examination findings from the Clinical
Examination Context. Retinal findings documented during examination trigger retinal
assessments and treatment planning.

### Diagnostic Imaging to Glaucoma Management

Relationship: Conformist.
The Glaucoma Management Context conforms to the imaging data model produced by the
Diagnostic Imaging Context for OCT nerve fiber layer analysis and visual field test
results. The Glaucoma Management Context adapts to the imaging data format rather
than requesting changes.

### Diagnostic Imaging to Retinal Care

Relationship: Conformist.
The Retinal Care Context conforms to imaging outputs for OCT macular scans, fundus
photographs, and fluorescein angiography results. These imaging studies are consumed
as-is for retinal condition assessment.

### Diagnostic Imaging to Surgical Management

Relationship: Customer-Supplier.
The Surgical Management Context requests specific imaging studies (e.g., IOL biometry,
corneal topography) to inform operative planning. The Diagnostic Imaging Context supplies
these results in a format agreed upon between the two teams.

### Surgical Management to Refractive Services

Relationship: Shared Kernel.
Refractive Services and Surgical Management share a small set of concepts related to
perioperative management, including surgical consent, sterile technique protocols, and
post-operative assessment standards. This shared kernel is carefully managed to avoid
excessive coupling.

### Glaucoma Management to Surgical Management

Relationship: Customer-Supplier.
When medical therapy is insufficient, the Glaucoma Management Context refers patients
to the Surgical Management Context for glaucoma surgical interventions. The Surgical
Management Context provides surgical outcome data back to Glaucoma Management for
longitudinal tracking.

### Retinal Care to Surgical Management

Relationship: Customer-Supplier.
Retinal conditions requiring surgical intervention (vitrectomy, retinal detachment
repair) are referred from Retinal Care to Surgical Management. The Surgical Management
Context provides operative outcomes back to Retinal Care.

## External System Integrations

### EHR Systems

Integration Pattern: Anti-Corruption Layer.
All bounded contexts interact with external electronic health record systems through
an anti-corruption layer that translates between the ophthalmology domain model and
the EHR data model (commonly HL7 FHIR or HL7 v2).

### PACS / Imaging Archives

Integration Pattern: Open Host Service.
The Diagnostic Imaging Context exposes an open host service using DICOM standards
for integration with picture archiving and communication systems (PACS).

### Billing Systems

Integration Pattern: Anti-Corruption Layer.
Surgical Management and Clinical Examination contexts translate clinical encounters
into billing-compatible formats (CPT codes, ICD-10 diagnoses) through an ACL.

## Map Governance

The context map is reviewed quarterly by representatives from each bounded context
team, along with clinical stakeholders. Changes to integration patterns or context
boundaries require consensus and are documented in the context map changelog.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 14.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 3.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 9.
- Brandolini, Alberto. *EventStorming*. Leanpub, 2021.
