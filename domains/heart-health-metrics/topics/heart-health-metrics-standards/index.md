# Heart Health Metrics Standards

Heart health metrics standards define the clinical, technical, and interoperability standards that govern cardiac data representation, exchange, measurement, and terminology within the heart health metrics domain. These standards inform value object design, published language definitions, and integration protocols across all bounded contexts.

## Overview

The heart health metrics domain operates within a rich ecosystem of clinical and technical standards. These standards are not optional compliance checkboxes; they are fundamental inputs to the domain model itself. Blood pressure measurement categories follow ACC/AHA guidelines. ECG signal formats follow IEEE and DICOM standards. Clinical observations use LOINC codes. Diagnoses use SNOMED CT codes. Health data exchange uses HL7 FHIR resources. Device communication uses IEEE 11073 protocols.

Standards serve as a form of published language in the DDD sense: they provide shared, well-documented data formats and vocabularies that enable interoperability between bounded contexts and with external systems. Incorporating standards into the domain model from the outset prevents costly retrofitting and ensures clinical validity.

## Key Concepts

**HL7 FHIR (Fast Healthcare Interoperability Resources).** The primary standard for exchanging cardiac health data with external clinical systems. FHIR defines resource types for observations (heart rate, blood pressure), diagnostic reports (ECG interpretations), risk assessments, and clinical decision support hooks. The Clinical Integration Context maps domain objects to FHIR resources.

**LOINC (Logical Observation Identifiers Names and Codes).** The coding system used to identify cardiac measurements. Heart rate uses LOINC code 8867-4. Systolic blood pressure uses 8480-6. Diastolic blood pressure uses 8462-4. QT interval uses 8634-8. LOINC codes are embedded in value objects and used in FHIR resource construction.

**SNOMED CT.** The clinical terminology system used to code cardiac diagnoses, findings, and procedures. Atrial fibrillation is SNOMED CT 49436004. Hypertension is 38341003. ECG procedure is 29303009. SNOMED CT codes enable unambiguous clinical communication across systems.

**IEEE 11073.** The family of standards for personal health device communication. IEEE 11073-10404 defines the pulse oximeter device specialization. IEEE 11073-10407 defines the blood pressure monitor specialization. IEEE 11073-10406 defines the heart rate monitor specialization. These standards inform the Data Collection Context's device abstraction layer.

**ACC/AHA Clinical Guidelines.** The American College of Cardiology and American Heart Association publish evidence-based guidelines that define clinical thresholds, risk scoring algorithms, and treatment recommendations. Blood pressure staging (normal, elevated, Stage 1, Stage 2, hypertensive crisis) follows the 2017 ACC/AHA guideline. ASCVD risk calculation follows the 2013 ACC/AHA guideline.

**European Society of Cardiology HRV Standards.** The 1996 Task Force publication defines standard methods for measuring and interpreting heart rate variability, including time-domain measures (SDNN, RMSSD, pNN50), frequency-domain measures (VLF, LF, HF), and recording requirements (5-minute short-term, 24-hour long-term).

**ICD-10 (International Classification of Diseases).** The World Health Organization's diagnostic coding system used for billing and epidemiological reporting. Cardiac diagnoses carry ICD-10 codes (e.g., I48 for atrial fibrillation, I10 for essential hypertension) that the domain must map for administrative and reporting purposes.

## Domain Examples

When the Cardiac Analysis Context computes HRV metrics, it follows the European Society of Cardiology Task Force standards: SDNN is calculated from all normal R-R intervals over the recording period, RMSSD from successive interval differences, and frequency-domain measures from power spectral analysis using Fast Fourier Transform or autoregressive methods. The resulting HRV Metrics value object includes all standard parameters with their defined units.

When the Clinical Integration Context constructs a FHIR Observation for a blood pressure reading, it uses LOINC code 85354-9 for the blood pressure panel, with component codes 8480-6 (systolic) and 8462-4 (diastolic), units in mmHg, and a category code of "vital-signs." This standardized representation ensures that any FHIR-compliant EHR can correctly interpret the observation.

## Relationships to Other Topics

- [Ubiquitous Language](../ubiquitous-language/index.md) - Standards define authoritative term definitions.
- [Value Objects](../value-objects/index.md) - Standards define units, ranges, and coding for value objects.
- [Clinical Integration Context](../clinical-integration-context/index.md) - Standards govern external data exchange.
- [Data Collection Context](../data-collection-context/index.md) - Device standards inform the device abstraction layer.
- [Cardiac Analysis Context](../cardiac-analysis-context/index.md) - HRV and ECG analysis standards.
- [Regulatory Compliance](../regulatory-compliance/index.md) - Standards support regulatory compliance requirements.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly, 2021.
- HL7 FHIR Specification, R4. https://www.hl7.org/fhir/
- LOINC Committee. "Logical Observation Identifiers Names and Codes." Regenstrief Institute. https://loinc.org/
- Task Force of the European Society of Cardiology. "Heart Rate Variability: Standards of Measurement." *Circulation*, 93(5), 1043-1065, 1996.
- Whelton, P.K. et al. "2017 ACC/AHA Guideline for High Blood Pressure." *Hypertension*, 71(6), e13-e115, 2018.
- IEEE 11073 Personal Health Device Communication Standards. IEEE Standards Association.
