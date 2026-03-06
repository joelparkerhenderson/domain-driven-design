# Orthopaedic Standards

## Overview

Orthopaedic standards are the clinical classification systems, measurement protocols,
outcome instruments, and data standards that govern how orthopaedic conditions are
categorized, measured, and reported. These standards directly influence the design of
domain models, value objects, and published languages within the orthopaedic bounded
contexts.

## Purpose

Standards ensure consistency, comparability, and interoperability in orthopaedic care.
In DDD terms, standards shape the ubiquitous language and constrain value object design.
An AO/OTA fracture code is not an arbitrary string; it must conform to the classification
system's valid combinations. A patient-reported outcome score is not a raw number; it
must follow the validated scoring methodology. The domain model enforces these standards
at the type level.

## Classification Standards

### AO/OTA Fracture Classification

The AO Foundation and Orthopaedic Trauma Association maintain the standard fracture
classification system. It uses an alphanumeric code: bone number (1-9), segment
(1-3), fracture type (A, B, C), group (1-3), and subgroup (1-3).

Impact on domain model: The AOOTACode value object validates that the bone, segment,
type, group, and subgroup form a valid combination. Invalid codes are rejected at
construction time.

### Kellgren-Lawrence Classification

The Kellgren-Lawrence grading system classifies osteoarthritis severity on radiographs
from Grade 0 (no arthritis) to Grade 4 (severe arthritis). Used in the Clinical
Assessment Context to inform treatment decisions.

Impact on domain model: The KellgrenLawrenceGrade value object constrains values to
the valid range (0-4) and provides domain meaning to the numeric grade.

### Garden Classification

The Garden classification categorizes femoral neck fractures into four types based on
displacement. Used in the Fracture Management Context to guide treatment decisions.

### Neer Classification

The Neer classification categorizes proximal humerus fractures based on the number
of displaced parts. Used in Fracture Management for upper extremity trauma.

## Outcome Measurement Standards

### Oxford Hip Score and Oxford Knee Score

Validated 12-item patient-reported outcome measures scored from 0 (worst) to 48 (best).
Used in the Joint Replacement Context for post-operative outcome monitoring.

Impact on domain model: Score value objects enforce the valid range and calculation
methodology. Individual item responses are captured to support data quality validation.

### WOMAC (Western Ontario and McMaster Universities Osteoarthritis Index)

A validated questionnaire with subscales for pain (5 items), stiffness (2 items), and
physical function (17 items). Used across Joint Replacement and Rehabilitation contexts.

### Visual Analog Scale (VAS) for Pain

A continuous scale from 0 (no pain) to 100 (worst pain). Used across all contexts
for pain assessment.

### EQ-5D

A generic health-related quality of life measure with five dimensions. Used alongside
joint-specific scores for comprehensive outcome assessment.

## Data Standards

### DICOM (Digital Imaging and Communications in Medicine)

The international standard for medical imaging data. All orthopaedic imaging flows
through DICOM-compliant systems. The anti-corruption layer between PACS and the
Clinical Assessment Context translates DICOM data structures.

### HL7 FHIR (Fast Healthcare Interoperability Resources)

The modern healthcare interoperability standard. Used for patient demographics,
clinical observations, and care plans exchanged between orthopaedic contexts and
hospital systems.

### SNOMED CT and ICD-10/11

Clinical terminology and classification systems for diagnoses and procedures.
Used in Clinical Assessment for diagnostic coding and in Surgical Planning for
procedure classification.

## Registry Standards

### National Joint Registry (NJR) - United Kingdom

Defines submission requirements for hip, knee, shoulder, elbow, and ankle
arthroplasty data. The Joint Replacement Context conforms to NJR data fields.

### American Joint Replacement Registry (AJRR) - United States

The AAOS-managed registry with specific data submission formats for arthroplasty
procedures performed in the United States.

## Clinical Practice Guidelines

### AAOS Clinical Practice Guidelines

Evidence-based guidelines published by the American Academy of Orthopaedic Surgeons
covering conditions such as hip and knee osteoarthritis, rotator cuff tears, and
distal radius fractures. These guidelines influence decision-making logic in domain
services.

### NICE Guidelines

National Institute for Health and Care Excellence guidelines relevant to orthopaedic
care in the United Kingdom, including joint replacement referral criteria and
fracture management protocols.

## Standards in Domain Model Design

- Value objects enforce classification validity at construction time.
- Published language schemas reference specific standard versions.
- Domain services implement clinical guideline logic for decision support.
- Anti-corruption layers translate between external data standards and domain models.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- AO Foundation. AO/OTA Fracture Classification. https://www.aofoundation.org
- Dawson, J. et al. "The Oxford Hip Score." Journal of Bone and Joint Surgery, 1996.
