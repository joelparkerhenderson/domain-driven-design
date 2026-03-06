# Otolaryngology Standards - Otolaryngology Domain

## Overview

Domain-specific standards inform value object design and published language patterns within the otolaryngology domain. ENT practice relies on a comprehensive set of clinical, coding, terminology, and measurement standards that shape how the domain model represents clinical concepts. These standards ensure that the domain model aligns with established medical practice and enables interoperability with external healthcare systems.

## Clinical Classification Standards

### ICD-10-CM (International Classification of Diseases, 10th Revision, Clinical Modification)

ICD-10-CM provides the diagnosis coding system used throughout the otolaryngology domain. Key code ranges relevant to ENT include:

- H60-H95: Diseases of the ear and mastoid process (hearing loss, otitis media, cholesteatoma)
- J00-J39: Diseases of the upper respiratory tract (rhinitis, sinusitis, tonsillitis, laryngitis)
- J40-J47: Chronic lower respiratory diseases (relevant for airway management)
- C00-C14: Malignant neoplasms of lip, oral cavity, and pharynx
- C30-C39: Malignant neoplasms of respiratory and intrathoracic organs (nasal cavity, larynx)
- C73: Malignant neoplasm of thyroid gland
- G47.3: Sleep apnea
- R13: Dysphagia

In the domain model, ICD-10 codes are represented as value objects with code, description, and version, ensuring valid code usage and enabling mapping between domain diagnoses and standardized codes.

### CPT (Current Procedural Terminology)

CPT provides procedure coding for all ENT surgical and diagnostic procedures. Key code ranges include:

- 30000-30999: Nose procedures (septoplasty, rhinoplasty, turbinate reduction)
- 31000-31299: Sinus procedures (FESS, sinus irrigation, balloon sinuplasty)
- 31500-31599: Larynx procedures (laryngoscopy, injection laryngoplasty)
- 42000-42999: Pharynx/tonsil procedures (tonsillectomy, adenoidectomy, UPPP)
- 60000-60699: Thyroid/parathyroid procedures (thyroidectomy, parathyroidectomy)
- 69000-69979: Ear procedures (myringotomy, tympanoplasty, mastoidectomy, cochlear implant)
- 92550-92700: Audiological testing (pure tone audiometry, speech audiometry, OAE, ABR)
- 95004-95199: Allergy testing and immunotherapy

CPT codes are modeled as value objects within the Surgical Management and Clinical Assessment contexts, carrying code, description, relative value units, and applicable modifiers.

### SNOMED CT (Systematized Nomenclature of Medicine - Clinical Terms)

SNOMED CT provides the clinical terminology standard for representing ENT clinical concepts unambiguously. Unlike ICD-10 (designed for classification) or CPT (designed for billing), SNOMED CT is designed to represent clinical meaning. It serves as the published language for inter-context clinical concept communication.

Key SNOMED CT hierarchies for otolaryngology:
- Body structure hierarchy: Anatomical structures of the ear, nose, throat, and head/neck.
- Clinical finding hierarchy: Signs, symptoms, and diagnoses specific to ENT conditions.
- Procedure hierarchy: Diagnostic and therapeutic ENT procedures.

### LOINC (Logical Observation Identifiers Names and Codes)

LOINC provides standardized codes for laboratory and clinical observations. In the otolaryngology domain, LOINC codes identify specific test types within audiological evaluation, allergy testing, and sleep study results, enabling interoperability with laboratory and diagnostic systems.

## Clinical Measurement Standards

### Audiometric Standards

- **ANSI S3.6**: Standard for audiometric equipment calibration, ensuring valid and comparable hearing test results.
- **ANSI S3.1**: Standard for ambient noise levels in audiometric test environments.
- **ISO 8253**: International standard for audiometric test methods (pure tone, speech audiometry).

These standards inform the design of audiometric value objects, which include calibration reference and test environment metadata.

### Sleep Study Standards

- **AASM Manual for Scoring Sleep**: Defines rules for scoring polysomnography events (apneas, hypopneas, arousals, sleep stages). Version 2.6 is the current standard.
- **AASM Clinical Practice Guidelines**: Define diagnostic criteria and treatment thresholds for obstructive sleep apnea.

AHI calculation and severity classification value objects implement these standardized scoring rules.

### Voice Assessment Standards

- **CAPE-V (Consensus Auditory-Perceptual Evaluation of Voice)**: Standardized perceptual voice assessment protocol from ASHA.
- **Voice Handicap Index (VHI)**: Validated 30-item patient-reported outcome measure with established psychometric properties.

Voice assessment value objects implement validated scoring algorithms from these standards.

### Allergy Testing Standards

- **Joint Task Force Practice Parameters**: Define standardized protocols for skin testing technique, allergen extract preparation, result interpretation criteria, and immunotherapy dosing schedules.

Allergy testing value objects implement interpretation criteria from these practice parameters.

## Interoperability Standards

### HL7 FHIR

HL7 FHIR provides the data exchange standard for communicating with external EHR systems. FHIR resources serve as the published language at external integration boundaries.

### HL7 v2

HL7 version 2 messaging remains common for laboratory result communication (ORU messages) and order entry (ORM messages), particularly for allergy laboratory results.

### DICOM

DICOM (Digital Imaging and Communications in Medicine) provides the standard for medical imaging data exchange, relevant to CT, MRI, and ultrasound imaging in the Clinical Assessment Context.

## Standards Governance in the Domain Model

Standards are encapsulated within value objects and domain services, not exposed directly in the domain model's ubiquitous language. A clinician speaks of "hearing thresholds," not "ANSI S3.6 compliant audiometric results." The domain model translates between clinical language and standards-compliant representations at the appropriate boundaries.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- World Health Organization. *International Classification of Diseases, 10th Revision*. https://www.who.int/classifications/icd.
- American Medical Association. *CPT Professional Edition*. Published annually.
- SNOMED International. SNOMED CT. https://www.snomed.org.
- HL7 International. FHIR R4. https://www.hl7.org/fhir/.
