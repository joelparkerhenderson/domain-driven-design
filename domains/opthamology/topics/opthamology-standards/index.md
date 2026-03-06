# Ophthalmology Standards

## Overview

The ophthalmology domain operates within a rich ecosystem of clinical, technical, and
data standards that inform domain model design, value object construction, published
languages, and integration interfaces. These standards ensure interoperability between
ophthalmology systems and compliance with industry-wide expectations for clinical data
quality and exchange. In Domain-Driven Design, standards influence the design of value
objects, anti-corruption layers, and published languages.

## Clinical Coding Standards

### ICD-10-CM (International Classification of Diseases, 10th Revision, Clinical Modification)

ICD-10-CM provides the diagnosis coding system used throughout the ophthalmology domain.
Ophthalmology-specific codes reside primarily in Chapter VII (H00-H59: Diseases of the
Eye and Adnexa) and Chapter VII supplement codes. The domain model uses ICD-10 codes
in value objects that represent diagnoses, in domain events that communicate clinical
findings, and in ACLs that translate clinical data for billing systems.

Key ophthalmology code ranges:
- H25-H28: Disorders of the lens (cataract)
- H30-H36: Disorders of the choroid and retina
- H40-H42: Glaucoma
- H49-H52: Disorders of ocular muscles, binocular movement, accommodation, refraction
- E08-E13 with ophthalmic manifestations: Diabetic retinopathy

### CPT (Current Procedural Terminology)

CPT codes define the procedures and services performed in ophthalmology practice.
The domain model references CPT codes in the Surgical Management Context for procedure
classification and in the Clinical Examination Context for encounter-level service
coding. The Retinal Care Context uses CPT codes for intravitreal injection services.

Key ophthalmology CPT ranges:
- 65091-68899: Eye and ocular adnexa surgical procedures
- 67028: Intravitreal injection
- 66982-66984: Cataract surgery codes
- 92002-92014: Ophthalmological examination services
- 92015: Refraction
- 92081-92083: Visual field examination codes
- 92132-92134: OCT codes

### SNOMED CT (Systematized Nomenclature of Medicine - Clinical Terms)

SNOMED CT provides a comprehensive clinical terminology for documenting clinical
findings, diagnoses, and procedures in a structured, machine-readable format. The
ophthalmology domain uses SNOMED CT concepts in value objects for clinical findings
and in published languages for inter-context communication when more granularity
than ICD-10 is needed.

## Imaging Standards

### DICOM (Digital Imaging and Communications in Medicine)

DICOM is the foundational standard for the Diagnostic Imaging Context. It defines:
- Image file formats for ophthalmic imaging modalities (OCT, fundus photography,
  angiography, visual fields).
- Communication protocols for image exchange (C-STORE, C-FIND, C-MOVE, C-GET).
- Modality worklist management for imaging order fulfillment.
- Structured reporting templates for ophthalmic imaging interpretation.
- Ophthalmic-specific information object definitions (IODs) including ophthalmic
  photography, ophthalmic tomography, and visual field data.

The Diagnostic Imaging Context uses DICOM as its open host service protocol for PACS
integration and as the published language for imaging data exchange.

### DICOM Supplement 173: Ophthalmic Refractive Reports

This supplement defines standardized storage of refractive measurements, enabling
interoperability between autorefractors, phoropters, and clinical systems. The
Clinical Examination Context uses this standard in its ACL for refraction device
integration.

## Interoperability Standards

### HL7 FHIR (Fast Healthcare Interoperability Resources)

HL7 FHIR provides the RESTful interoperability standard for EHR integration across
all bounded contexts. Relevant FHIR resources include:
- Patient: patient demographic data
- Encounter: clinical visit documentation
- Observation: clinical measurements (visual acuity, IOP)
- DiagnosticReport: imaging interpretation reports
- Procedure: surgical and therapeutic procedures
- MedicationRequest: pharmacy prescriptions
- Condition: diagnosis documentation

The ophthalmology domain's ACLs translate between FHIR resources and domain aggregates.

### HL7 v2

HL7 v2 messaging remains in use for legacy system integration, particularly for
laboratory result feeds (ORU messages) and order management (ORM messages). The
domain's ACLs handle both FHIR and v2 formats.

## Clinical Guidelines

### AAO Preferred Practice Patterns (PPP)

The American Academy of Ophthalmology's Preferred Practice Patterns provide
evidence-based clinical guidelines that inform business rules within bounded contexts:
- Primary Open-Angle Glaucoma PPP informs Glaucoma Management Context rules.
- Diabetic Retinopathy PPP informs Retinal Care Context rules.
- Cataract in the Adult Eye PPP informs Surgical Management Context rules.
- Age-Related Macular Degeneration PPP informs Retinal Care Context rules.
- Comprehensive Adult Medical Eye Evaluation PPP informs Clinical Examination rules.

### ETDRS (Early Treatment Diabetic Retinopathy Study)

ETDRS provides the classification system and visual acuity testing protocol used in
the Retinal Care Context for diabetic retinopathy severity grading and the Clinical
Examination Context for standardized visual acuity measurement.

### AREDS (Age-Related Eye Disease Study)

AREDS provides the classification system for AMD severity used in the Retinal Care
Context to stage macular degeneration and determine treatment recommendations.

## Impact on Domain Model

Standards influence the domain model in several ways:
- Value objects incorporate standard code sets (ICD-10, CPT, SNOMED CT).
- Anti-corruption layers translate between domain models and standard formats.
- Published languages adopt standard terminologies for inter-context communication.
- Business rules reference clinical guidelines as their authoritative source.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- DICOM Standards Committee. *DICOM Standard*. https://www.dicomstandard.org/
- HL7 International. *HL7 FHIR Specification*. https://www.hl7.org/fhir/
- American Medical Association. *CPT Professional Edition*. Updated annually.
