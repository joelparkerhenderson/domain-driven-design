# Dental Standards - Dental Domain

## Overview

Dental standards provide the coding systems, classification frameworks, and clinical guidelines that inform domain model design in the dental domain. These standards shape value object definitions, published language contracts, and the business rules that govern clinical and administrative processes. Aligning the domain model with established dental standards ensures that the system speaks the same language as the broader dental industry and supports interoperability with external systems.

## Current Dental Terminology (CDT)

The Current Dental Terminology code set, maintained by the American Dental Association, is the standardized system for reporting dental procedures on insurance claims and in clinical records. CDT codes are organized by category:

- D0100-D0999: Diagnostic services (examinations, radiographs, tests).
- D1000-D1999: Preventive services (prophylaxis, fluoride, sealants).
- D2000-D2999: Restorative services (fillings, crowns, inlays).
- D3000-D3999: Endodontic services (root canals, apicoectomies).
- D4000-D4999: Periodontic services (scaling, root planing, grafts).
- D5000-D5999: Prosthodontic services (dentures, partials).
- D6000-D6999: Implant services (surgical placement, prosthetics).
- D7000-D7999: Oral surgery services (extractions, biopsies).
- D8000-D8999: Orthodontic services (comprehensive, limited treatment).
- D9000-D9999: Adjunctive general services (anesthesia, consultations).

CDT codes are updated annually with additions, revisions, and deletions. The domain model must support CDT code versioning to correctly handle procedures coded under different annual editions. The CDT Procedure Code value object validates format and provides category lookup based on code ranges.

## Tooth Numbering Systems

### Universal Numbering System

The Universal Numbering System, adopted by the ADA, assigns numbers 1-32 to permanent teeth and letters A-T to primary (deciduous) teeth. Numbering starts at the upper right third molar (1) and proceeds across the upper arch to the upper left third molar (16), then continues to the lower left third molar (17) and across the lower arch to the lower right third molar (32).

The domain model uses the Universal Numbering System as the primary tooth identification scheme for consistency with ADA standards and insurance claim requirements.

### ISO/FDI Notation

The Federation Dentaire Internationale (FDI) two-digit notation system is used internationally. The first digit indicates the quadrant (1-4 for permanent, 5-8 for primary) and the second digit indicates the tooth position (1-8). For example, tooth 11 is the upper right central incisor.

The domain model supports FDI notation through value object translation for international interoperability, while maintaining Universal numbering as the internal standard.

## Surface Notation

Tooth surfaces are designated using standard abbreviations:
- M: Mesial (toward the midline).
- O: Occlusal (chewing surface of posterior teeth).
- I: Incisal (biting edge of anterior teeth).
- D: Distal (away from the midline).
- B: Buccal (cheek side) or F: Facial (equivalent for anterior teeth).
- L: Lingual (tongue side) or P: Palatal (equivalent for upper teeth).

Multi-surface designations combine abbreviations: MOD indicates mesial, occlusal, and distal surfaces. The Tooth Surface value object enforces valid surface combinations and the correct use of occlusal versus incisal based on tooth type.

## Periodontal Classification Standards

The 2017 World Workshop on the Classification of Periodontal and Peri-Implant Diseases and Conditions replaced the 1999 classification system. The current system classifies periodontitis by:

- Stage (I-IV): Based on severity (attachment loss, bone loss, tooth loss).
- Grade (A-C): Based on progression rate and risk factors (smoking, diabetes).
- Extent: Localized (less than 30% of teeth) or generalized.

The Periodontal Disease Classification value object encodes this multi-dimensional classification system and provides comparison logic for tracking disease status changes over time.

## Radiographic Guidelines

The ADA, in collaboration with the FDA, publishes guidelines for dental radiographic examinations that recommend imaging intervals based on patient age, risk status, and clinical indications. The guidelines specify:

- New patient: Full-mouth or panoramic series with bitewings.
- Recall patient (low risk): Bitewings every 24-36 months.
- Recall patient (high risk): Bitewings every 6-18 months.
- Children and adolescents: Modified intervals based on dentition stage.

These guidelines inform the radiographic ordering protocols in the Clinical Examination Context and the insurance coverage frequency limitations in the Practice Management Context.

## ANSI X12 837D Standard

The ANSI X12 837D transaction set is the standard format for electronic dental claim submission. It defines the data segments and elements required for claim processing, including patient demographics, provider information, procedure codes, tooth and surface designations, and supporting documentation references. The Practice Management Context's anti-corruption layer translates between the internal domain model and the X12 837D format.

## HL7 FHIR Dental Profiles

Health Level Seven (HL7) Fast Healthcare Interoperability Resources (FHIR) includes dental-specific profiles that enable interoperability between dental systems and broader healthcare information exchanges. The dental profiles extend standard FHIR resources (Patient, Condition, Procedure, Observation) with dental-specific extensions for tooth numbers, surface codes, and dental charting data.

## References

- American Dental Association. CDT: Current Dental Terminology. ADA, updated annually.
- American Dental Association/Food and Drug Administration. Dental Radiographic Examinations: Recommendations for Patient Selection and Limiting Radiation Exposure. ADA/FDA, 2012.
- Federation Dentaire Internationale. Two-Digit Notation System. FDI World Dental Federation.
- Tonetti, M.S., H. Greenwell, K.S. Kornman. Staging and Grading of Periodontitis. Journal of Periodontology, 2018.
- ANSI ASC X12. Implementation Guide for 837D Health Care Claim: Dental. X12 Standards.
- HL7 International. FHIR Dental Data Exchange Project. HL7, ongoing.
- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
