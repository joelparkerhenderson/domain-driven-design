# Healthcare Standards in Hospital Management

## Overview

Healthcare standards define the data formats, code systems, and communication protocols used to exchange clinical, administrative, and financial information between systems. In a DDD hospital system, these standards inform the design of value objects, published languages, and anti-corruption layers.

## Relevance to Hospital Management

Hospital systems do not operate in isolation. They exchange data with laboratories, pharmacies, insurance companies, public health agencies, and other hospitals. Standards enable interoperability:

- Lab systems send results using HL7 messages
- Insurance claims use EDI formats with ICD-10 and CPT codes
- Clinical data is shared via FHIR resources
- Public health reporting uses standardized code systems

Understanding these standards is essential for designing bounded context interfaces, anti-corruption layers, and published languages.

## Messaging Standards

### HL7 Version 2

The most widely deployed healthcare messaging standard. HL7 v2 uses pipe-delimited segments to transmit clinical data.

- **ADT (Admission, Discharge, Transfer)**: Patient movement messages — A01 (admit), A02 (transfer), A03 (discharge), A04 (register)
- **ORM (Order Message)**: Lab, radiology, and pharmacy orders
- **ORU (Observation Result)**: Lab and diagnostic results
- **SIU (Scheduling)**: Appointment scheduling notifications
- **DFT (Detailed Financial Transaction)**: Charge posting

**DDD mapping**: HL7 v2 messages often map to domain events. An ADT A01 (admit) corresponds to a PatientAdmitted domain event. An ORU message corresponds to LabResultReceived.

### HL7 FHIR (Fast Healthcare Interoperability Resources)

A modern RESTful standard using JSON/XML resources. FHIR defines standardized resource types that align well with DDD concepts.

| FHIR Resource | DDD Concept | Bounded Context |
|---------------|-------------|----------------|
| Patient | Patient entity | Patient Context |
| Encounter | Encounter entity | Clinical/EMR |
| Appointment | Appointment entity | Scheduling |
| DiagnosticReport | LabResult entity | Clinical/EMR |
| MedicationRequest | Prescription entity | Clinical/EMR |
| Claim | Claim entity | Billing |
| Observation | VitalSigns value object | Clinical/EMR |
| Condition | Diagnosis value object | Clinical/EMR |
| AllergyIntolerance | Allergy value object | Patient Context |

**DDD mapping**: FHIR resources serve as an excellent published language for inter-context and external system communication. Internal domain models translate to/from FHIR through anti-corruption layers.

## Code Systems

### ICD-10 (International Classification of Diseases)

Published by the World Health Organization. Used to classify diagnoses and health conditions.

- **Structure**: Alphanumeric codes, 3–7 characters (e.g., J18.9 — Pneumonia, unspecified)
- **Usage**: Clinical documentation, billing claims, public health reporting
- **DDD mapping**: DiagnosisCode value object with code system, code value, and description

### CPT (Current Procedural Terminology)

Published by the American Medical Association. Used to code medical procedures and services for billing.

- **Structure**: 5-digit numeric codes (e.g., 99213 — Office visit, established patient)
- **Categories**: Category I (procedures), Category II (performance measures), Category III (emerging technology)
- **DDD mapping**: BillingCode value object in the Billing context

### SNOMED CT (Systematized Nomenclature of Medicine — Clinical Terms)

Published by IHTSDO. The most comprehensive clinical terminology, covering findings, procedures, body structures, organisms, and substances.

- **Structure**: Numeric concept IDs with hierarchical relationships (e.g., 386661006 — Fever)
- **Usage**: Clinical documentation, decision support, semantic interoperability
- **DDD mapping**: ClinicalFinding value objects, used within Clinical/EMR context for precise clinical documentation

### LOINC (Logical Observation Identifiers Names and Codes)

Published by the Regenstrief Institute. Universal standard for identifying laboratory tests and clinical observations.

- **Structure**: Numeric codes with check digit (e.g., 2160-0 — Creatinine, serum)
- **Usage**: Lab orders and results, vital sign observations
- **DDD mapping**: TestType value object for lab orders; ObservationType for clinical observations

### RxNorm

Published by the National Library of Medicine. Standard nomenclature for medications.

- **Structure**: Concept unique identifiers (CUIs) for ingredients, dose forms, and branded/generic drugs
- **Usage**: Medication orders, drug interaction checking, formulary management
- **DDD mapping**: Medication value object in prescriptions and pharmacy orders

### DICOM (Digital Imaging and Communications in Medicine)

Standard for medical imaging data.

- **Usage**: Radiology (X-ray, CT, MRI), pathology, ophthalmology
- **DDD mapping**: ImagingStudy entity in the Clinical/EMR context; DICOM metadata as value objects

## Administrative Standards

### EDI (Electronic Data Interchange)

Used for administrative and financial transactions with insurance companies:

- **837P/837I**: Professional and institutional claims
- **835**: Remittance advice (payment details)
- **270/271**: Eligibility inquiry and response
- **276/277**: Claim status inquiry and response

**DDD mapping**: Anti-corruption layer translates between domain Claim objects and EDI transaction formats.

## Relationships to Other Topics

- [Anti-Corruption Layer](../anti-corruption-layer/) — ACLs translate between external standards and internal domain models
- [Integration Patterns](../integration-patterns/) — Standards define the published languages used in integration
- [Value Objects](../value-objects/) — Code systems are modeled as value objects (DiagnosisCode, BillingCode)
- [Clinical/EMR Context](../clinical-emr-context/) — Primary consumer and producer of clinical standards
- [Billing/Administrative Context](../billing-administrative-context/) — Consumer of coding standards and EDI formats
- [Ubiquitous Language](../ubiquitous-language/) — Standards terminology informs the ubiquitous language

## References

- HL7 International. "HL7 FHIR R4 Specification." https://hl7.org/fhir/
- HL7 International. "HL7 Version 2 Product Suite." https://www.hl7.org/implement/standards/product_brief.cfm?product_id=185
- World Health Organization. "ICD-10: International Statistical Classification of Diseases and Related Health Problems." 10th Revision. WHO, 2019.
- American Medical Association. "CPT — Current Procedural Terminology." AMA, published annually.
- IHTSDO. "SNOMED CT." https://www.snomed.org/
- Regenstrief Institute. "LOINC." https://loinc.org/
- National Library of Medicine. "RxNorm." https://www.nlm.nih.gov/research/umls/rxnorm/
- NEMA. "DICOM Standard." https://www.dicomstandard.org/
- ASC X12. "Health Care EDI Transaction Standards."
