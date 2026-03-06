# Anti-Corruption Layer in the Orthopaedic Domain

## Overview

An anti-corruption layer (ACL) is a translation boundary that protects a bounded
context's domain model from being contaminated by the models of external systems or
other bounded contexts. In the orthopaedic domain, ACLs are critical because
orthopaedic systems must integrate with hospital information systems, imaging
archives, and national registries that use fundamentally different data models.

## Purpose

Without anti-corruption layers, the orthopaedic domain model would be forced to
adopt the data structures, naming conventions, and assumptions of external systems.
This would erode the ubiquitous language, introduce accidental complexity, and make
the domain model fragile in the face of external system changes.

## Key Anti-Corruption Layers

### PACS Integration ACL

The Picture Archiving and Communication System stores imaging data using DICOM
(Digital Imaging and Communications in Medicine) standards. The PACS ACL:

- Translates DICOM metadata into the Clinical Assessment Context's Imaging Study
  value objects.
- Maps DICOM series identifiers to domain-specific imaging references.
- Converts DICOM date formats and patient identifiers into domain representations.
- Shields the assessment model from DICOM's complex and verbose data structure.

### EHR/HIS Integration ACL

Electronic Health Record and Hospital Information Systems use HL7 v2, HL7 FHIR,
or proprietary formats. The EHR/HIS ACL:

- Translates patient demographics from HL7 resources into context-specific patient
  representations.
- Maps clinical orders from EHR format into domain commands.
- Converts medication and allergy data into pre-operative assessment value objects.
- Ensures that changes to the EHR's data model do not ripple into orthopaedic contexts.

### National Joint Registry ACL

National joint registries (NJR in the UK, AJRR in the US) have specific submission
formats. The registry ACL:

- Translates the Joint Replacement Context's Arthroplasty Case into the registry's
  submission schema.
- Maps internal implant identifiers to registry catalog numbers.
- Converts outcome scores into registry-specified formats and coding systems.
- Handles version changes in registry submission requirements.

### Laboratory System ACL

Pre-operative blood work and pathology results arrive from laboratory information
systems. The laboratory ACL:

- Translates lab result messages into pre-operative assessment value objects.
- Maps lab-specific coding systems (LOINC) to domain-relevant test categories.
- Normalizes units of measurement across different laboratory systems.

## ACL Design Principles

- Place the ACL at the boundary of the bounded context, not inside the domain model.
- Use adapter and translator patterns to convert external data structures.
- Define the translation in terms of the domain's ubiquitous language.
- Test ACLs with contract tests that verify translation correctness.
- Version the ACL independently from both the domain model and the external system.

## Implementation Approach

1. Define the domain model's interface (what the context needs).
2. Analyze the external system's data model (what the system provides).
3. Build a translator that maps between the two models.
4. Wrap the translator in an adapter that the domain consumes.
5. Add error handling for data that cannot be cleanly translated.

## Benefits

- The orthopaedic domain model remains clean and clinically meaningful.
- External system upgrades require changes only in the ACL, not in the domain.
- Multiple external systems can be supported by separate ACLs behind a common interface.
- ACLs can be tested independently with known input/output pairs.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 14.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 3.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 9.
