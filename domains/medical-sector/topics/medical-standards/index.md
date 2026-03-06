# Medical Standards in Medical Practice

## Overview

Medical standards are industry-wide specifications for data formats, terminology, communication protocols, and coding systems that enable interoperability between healthcare systems. In Domain-Driven Design, these standards inform value object design, published languages, anti-corruption layer translations, and integration patterns. Understanding medical standards is essential for building a medical practice system that can exchange data with external laboratories, insurance payers, pharmacies, imaging systems, and health information exchanges.

## Key Standards

### HL7 FHIR (Fast Healthcare Interoperability Resources)

- **Purpose**: Modern RESTful standard for exchanging healthcare information electronically
- **Version**: R4 (current widely adopted release), R5 (emerging)
- **Key Resources**: Patient, Encounter, Condition, Observation, MedicationRequest, DiagnosticReport, Claim, ImagingStudy
- **DDD Relevance**: FHIR resources serve as a published language for inter-context communication and open host service APIs. FHIR profiles can be mapped to internal domain entities and value objects.
- **Organization**: HL7 International (https://hl7.org/fhir/)

### HL7 v2 (Version 2.x Messaging)

- **Purpose**: Legacy but still widely used messaging standard for real-time clinical data exchange
- **Key Messages**: ADT (admit/discharge/transfer), ORM (orders), ORU (results), SIU (scheduling), DFT (financial)
- **DDD Relevance**: HL7 v2 messages are translated through anti-corruption layers into internal domain objects. Lab interfaces, ADT feeds, and scheduling interfaces commonly use v2.
- **Organization**: HL7 International (https://www.hl7.org/implement/standards/)

### DICOM (Digital Imaging and Communications in Medicine)

- **Purpose**: International standard for transmitting, storing, retrieving, and sharing medical images
- **Key Concepts**: Study, Series, Instance, Modality, SOP Class, Transfer Syntax, WADO (Web Access to DICOM Objects)
- **DDD Relevance**: DICOM metadata maps to ImagingStudy entities and Modality value objects. DICOM protocols are encapsulated behind an anti-corruption layer in the Diagnostics & Imaging context.
- **Organization**: NEMA (https://www.dicomstandard.org/)

### ICD-10 (International Classification of Diseases, 10th Revision)

- **Purpose**: Global standard for coding diagnoses and health conditions
- **Variants**: ICD-10-CM (clinical modification for US diagnosis coding), ICD-10-PCS (procedure coding for inpatient)
- **Structure**: Alphanumeric codes (e.g., E11.9 = Type 2 diabetes without complications, J06.9 = Acute upper respiratory infection)
- **DDD Relevance**: ICD-10 codes are modeled as value objects (ICD10Code) used in problem lists, encounter diagnoses, and insurance claim service lines
- **Organization**: WHO (https://www.who.int/classifications/icd/), CMS for US modifications

### CPT (Current Procedural Terminology)

- **Purpose**: Standard for describing medical, surgical, and diagnostic services for billing
- **Structure**: Five-digit numeric codes organized into Category I (procedures), Category II (performance measures), Category III (emerging technology)
- **Examples**: 99213 (office visit, established patient, moderate complexity), 85025 (CBC with differential), 71046 (chest X-ray, 2 views)
- **DDD Relevance**: CPT codes are modeled as value objects used in encounter procedure documentation and insurance claim service lines
- **Organization**: AMA (https://www.ama-assn.org/practice-management/cpt)

### SNOMED CT (Systematized Nomenclature of Medicine Clinical Terms)

- **Purpose**: Comprehensive clinical terminology for electronic health records, covering clinical findings, procedures, body structures, substances, and organisms
- **Structure**: Hierarchical concept identifiers with descriptions and relationships
- **DDD Relevance**: SNOMED CT terms inform the ubiquitous language and provide precise clinical terminology for value objects representing findings, diagnoses, and procedures
- **Organization**: SNOMED International (https://www.snomed.org/)

### LOINC (Logical Observation Identifiers Names and Codes)

- **Purpose**: Universal standard for identifying medical laboratory observations, clinical measurements, and survey instruments
- **Structure**: Codes with six axes: component, property, timing, system, scale, method
- **Examples**: 2160-0 (serum creatinine), 718-7 (hemoglobin), 8480-6 (systolic blood pressure)
- **DDD Relevance**: LOINC codes are modeled as value objects (LOINCCode) used to identify lab test types and clinical observations in the Diagnostics context
- **Organization**: Regenstrief Institute (https://loinc.org/)

### NDC (National Drug Code)

- **Purpose**: Unique identifier for human drugs in the United States
- **Structure**: Three-segment number identifying labeler, product, and package (e.g., 0078-0435-15)
- **DDD Relevance**: NDC codes are modeled as value objects in the Pharmacy context for identifying specific drug products for dispensing and inventory
- **Organization**: FDA (https://www.fda.gov/drugs/drug-approvals-and-databases/national-drug-code-directory)

### RxNorm

- **Purpose**: Standardized nomenclature for clinical drugs providing normalized names for medications
- **Key Concepts**: Ingredient, Semantic Clinical Drug (SCD), Semantic Branded Drug (SBD), dose form, strength
- **DDD Relevance**: RxNorm codes serve as the canonical medication identifier in the Pharmacy context, enabling drug interaction checking and medication reconciliation across systems
- **Organization**: NLM (https://www.nlm.nih.gov/research/umls/rxnorm/)

### C-CDA (Consolidated Clinical Document Architecture)

- **Purpose**: HL7 standard for structured clinical document exchange (continuity of care documents, discharge summaries, referral notes)
- **Key Document Types**: CCD (Continuity of Care Document), Discharge Summary, Referral Note, Progress Note
- **DDD Relevance**: C-CDA documents are translated through anti-corruption layers during care transitions and health information exchange
- **Organization**: HL7 International

## Standards and DDD Patterns

| Standard | DDD Pattern | Usage |
|----------|------------|-------|
| HL7 FHIR | Published Language, Open Host Service | Inter-context APIs, external data exchange |
| HL7 v2 | Anti-Corruption Layer | Legacy lab, ADT, and scheduling interfaces |
| DICOM | Anti-Corruption Layer | Imaging system integration |
| ICD-10 | Value Object | Diagnosis coding in problem lists and claims |
| CPT | Value Object | Procedure coding in encounters and claims |
| SNOMED CT | Ubiquitous Language, Value Object | Clinical terminology in EHR |
| LOINC | Value Object | Lab test and observation identification |
| NDC | Value Object | Drug product identification |
| RxNorm | Value Object | Canonical medication identification |
| C-CDA | Anti-Corruption Layer | Clinical document exchange |

## Relationships to Other Topics

- [Value Objects](../value-objects/) -- Standards define the coding systems used in medical value objects
- [Anti-Corruption Layer](../anti-corruption-layer/) -- ACLs translate between external standard formats and internal models
- [Integration Patterns](../integration-patterns/) -- Standards serve as published languages for integration
- [Ubiquitous Language](../ubiquitous-language/) -- Clinical terminology standards inform domain vocabulary
- [Bounded Contexts](../bounded-contexts/) -- Different contexts use different subsets of standards
- [Regulatory Compliance](../regulatory-compliance/) -- HIPAA mandates specific standards for electronic transactions

## References

- HL7 International. "HL7 FHIR R4 Specification." https://hl7.org/fhir/
- HL7 International. "HL7 v2 Product Suite." https://www.hl7.org/implement/standards/
- NEMA. "DICOM Standard." https://www.dicomstandard.org/
- WHO. "International Classification of Diseases (ICD)." https://www.who.int/classifications/icd/
- AMA. "Current Procedural Terminology (CPT)." https://www.ama-assn.org/practice-management/cpt
- SNOMED International. "SNOMED CT." https://www.snomed.org/
- Regenstrief Institute. "LOINC." https://loinc.org/
- NLM. "RxNorm." https://www.nlm.nih.gov/research/umls/rxnorm/
- FDA. "National Drug Code Directory." https://www.fda.gov/drugs/drug-approvals-and-databases/national-drug-code-directory
- Benson, Tim & Grieve, Grahame. "Principles of Health Interoperability: FHIR, HL7 and SNOMED CT." 4th ed. Springer, 2021.
