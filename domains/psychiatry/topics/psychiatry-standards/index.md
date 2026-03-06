# Psychiatry Standards

## Overview

Domain-specific standards are external knowledge systems that inform the design of value objects, published languages, and business rules within the psychiatry domain. These standards provide the authoritative definitions, code sets, and measurement frameworks that the domain model must faithfully represent. In psychiatry, standards span diagnostic classification, medical coding, clinical measurement, laboratory reporting, and medication identification. Understanding these standards is essential for building a domain model that is clinically valid and interoperable with the broader healthcare ecosystem.

## Diagnostic Classification Standards

### DSM-5-TR (Diagnostic and Statistical Manual of Mental Disorders, Fifth Edition, Text Revision)

Published by the American Psychiatric Association (2022), the DSM-5-TR is the primary diagnostic classification system used in the United States for psychiatric disorders. It defines diagnostic categories, criteria sets, specifiers, and severity modifiers that form the authoritative vocabulary for the Diagnostic Assessment Context.

Domain model impact:
- Diagnosis Code value objects must support DSM-5-TR code structure including specifiers.
- Diagnostic criteria are modeled as business rules within the Differential Diagnosis Service.
- Severity specifiers (mild, moderate, severe) are value objects with diagnosis-specific definitions.
- The DSM-5-TR category hierarchy (neurodevelopmental, schizophrenia spectrum, bipolar, depressive, anxiety, OCD, trauma, dissociative, somatic, feeding, elimination, sleep, sexual, gender, disruptive, substance, neurocognitive, personality, paraphilic) informs aggregate organization.

### ICD-11 (International Classification of Diseases, Eleventh Revision)

Published by the World Health Organization (2019, effective 2022), the ICD-11 is the international standard for diagnostic classification. Chapter 06 covers mental, behavioural, and neurodevelopmental disorders. The ICD-11 must be supported alongside DSM-5-TR because international clinical settings, research, and mortality reporting use ICD classifications.

Domain model impact:
- Diagnosis Code value objects must support dual coding (DSM-5-TR and ICD-11).
- The anti-corruption layer must translate between DSM-5-TR and ICD-11 codes where mappings exist.
- Some ICD-11 categories do not have exact DSM-5-TR equivalents, requiring explicit handling.

### ICD-10-CM

The United States continues to use ICD-10-CM for billing and administrative purposes. The domain model must support ICD-10-CM codes for insurance claims and regulatory reporting, even though DSM-5-TR is the primary clinical classification.

## Medical Coding Standards

### CPT (Current Procedural Terminology)

Published by the American Medical Association, CPT codes identify specific medical procedures and services for billing purposes. Key psychiatric CPT codes include:

- 90791: Psychiatric diagnostic evaluation
- 90792: Psychiatric diagnostic evaluation with medical services
- 90832/90834/90837: Psychotherapy (30, 45, 60 minutes)
- 90833/90836/90838: Psychotherapy add-on to E/M (30, 45, 60 minutes)
- 90839/90840: Psychotherapy for crisis (first 60 minutes / each additional 30 minutes)
- 90846/90847: Family psychotherapy (without/with patient)
- 90853: Group psychotherapy
- 96130-96133: Psychological testing evaluation and administration

Domain model impact:
- Session Duration value objects must align with CPT time boundaries.
- The anti-corruption layer translates clinical service records into appropriate CPT codes for billing.

### HCPCS (Healthcare Common Procedure Coding System)

HCPCS Level II codes cover services not included in CPT, particularly rehabilitation services, psychiatric rehabilitation programs, and community-based mental health services. These codes are relevant to the Rehabilitation Services Context for billing supported employment, psychosocial rehabilitation, and community support services.

## Clinical Measurement Standards

### LOINC (Logical Observation Identifiers Names and Codes)

LOINC provides standardized codes for laboratory and clinical observations, including standardized assessment instruments. LOINC codes exist for major psychiatric rating scales:

- PHQ-9 (LOINC 44249-1)
- GAD-7 (LOINC 69737-5)
- Columbia Suicide Severity Rating Scale (LOINC panels)
- AUDIT-C (LOINC 75626-2)

Domain model impact:
- Rating Scale Score value objects reference LOINC codes for interoperability.
- Measurement Battery configurations use LOINC codes to identify instruments unambiguously.

### SNOMED CT (Systematized Nomenclature of Medicine - Clinical Terms)

SNOMED CT provides a comprehensive clinical terminology for documenting clinical findings, procedures, and diagnoses. It offers more granular clinical concepts than ICD codes and is used in structured clinical documentation and clinical decision support.

## Medication Standards

### RxNorm

Published by the National Library of Medicine, RxNorm provides normalized names and unique identifiers for clinical drugs. The Medication Management Context uses RxNorm concept unique identifiers (RxCUI) as the internal drug identification standard.

### NDC (National Drug Code)

The NDC identifies specific drug products (manufacturer, product, package). The anti-corruption layer translates between RxNorm (clinical) and NDC (dispensing) identifiers.

### FDA Drug Labeling

FDA-approved drug labeling provides the authoritative source for indications, contraindications, black box warnings, dosing ranges, and monitoring requirements. The Medication Management Context models these labeling elements as business rules that inform prescribing validation.

## Interoperability Standards

### HL7 FHIR (Fast Healthcare Interoperability Resources)

FHIR R4 provides the interoperability framework for exchanging clinical data with external EHR systems. Key FHIR resources relevant to the psychiatry domain include Condition (diagnoses), MedicationRequest (prescriptions), Encounter (evaluations/sessions), Observation (rating scale scores), and CarePlan (treatment/rehabilitation plans).

### NCPDP SCRIPT

The standard for electronic prescribing transactions between prescribers and pharmacies. Used by the Medication Management anti-corruption layer for prescription transmission.

## Standards Governance

Standards evolve over time (DSM-5 to DSM-5-TR, ICD-10 to ICD-11, FHIR R4 to R5). The domain model must accommodate version transitions through:
- Versioned value objects that identify which standard version applies.
- Anti-corruption layers that support multiple standard versions simultaneously during transition periods.
- Migration strategies that preserve historical data in its original standard version while enabling new data to use updated standards.

## References

- American Psychiatric Association. *DSM-5-TR*. APA Publishing, 2022.
- World Health Organization. *ICD-11*. WHO, 2019.
- American Medical Association. *CPT Professional Edition*. AMA, 2023.
- Regenstrief Institute. *LOINC*. https://loinc.org/.
- National Library of Medicine. *RxNorm*. https://www.nlm.nih.gov/research/umls/rxnorm/.
- HL7 International. *FHIR R4*. https://hl7.org/fhir/R4/.
- Evans, Eric. *Domain-Driven Design*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
