# Clinical Integration Context

## Overview

The Clinical Integration Context bridges the body health metrics system with external clinical healthcare systems. It manages Electronic Health Record (EHR) integration, physician reporting, and FHIR-based data exchange. This context operates as the primary anti-corruption layer between the internal domain model and the external world of clinical informatics, translating between the body health metrics ubiquitous language and clinical data standards such as HL7 FHIR, LOINC, and SNOMED CT.

## Scope and Responsibilities

This context manages the following integration capabilities:

- **EHR Integration**: Bidirectional data exchange with Electronic Health Record systems, enabling body health metrics to flow into patient charts and clinical data to flow into the body health system
- **Physician Reporting**: Generation of formatted health metric summaries suitable for clinical review, including standardized observations with clinical coding
- **FHIR Data Exchange**: Implementation of HL7 FHIR resource construction, validation, and transmission for interoperable health data sharing
- **Clinical Coding**: Mapping internal measurement types to LOINC observation codes and SNOMED CT clinical terms
- **Data Provenance**: Tracking the source, transformation history, and clinical validity of data exchanged across system boundaries

## FHIR Resource Mapping

The context translates internal domain concepts to FHIR R4 resources:

- **MeasurementSession** maps to FHIR **Bundle** containing related **Observation** resources
- **Weight** maps to FHIR **Observation** with LOINC code 29463-7 (Body weight)
- **Height** maps to FHIR **Observation** with LOINC code 8302-2 (Body height)
- **BMI** maps to FHIR **Observation** with LOINC code 39156-5 (Body mass index)
- **BloodPressure** maps to FHIR **Observation** with LOINC code 85354-9 (Blood pressure panel)
- **BodyFatPercentage** maps to FHIR **Observation** with LOINC code 41982-0 (Percentage of body fat)
- **CompositionProfile** maps to FHIR **DiagnosticReport** with component Observations
- **HealthScore** maps to FHIR **RiskAssessment** with outcome predictions
- **Person** maps to FHIR **Patient** resource

## Aggregate Design

### ClinicalReport (Aggregate Root)

The ClinicalReport aggregate packages health metrics data for transmission to external clinical systems.

**Components**:
- ReportMetadata (value object): report date, author, intended recipient, clinical context
- ClinicalObservations: collection of coded observation entries ready for clinical consumption
- ClinicalCoding (value object): LOINC and SNOMED CT code assignments for each observation
- SubmissionStatus (value object): draft, validated, submitted, acknowledged, rejected
- FhirBundle: the constructed FHIR resource bundle ready for transmission

**Invariants**:
- Every observation must carry a valid LOINC code
- Report must reference a verified person identity with matching FHIR Patient resource
- All measurements must be converted to UCUM (Unified Code for Units of Measure) standard units
- A submitted report cannot be modified; corrections require a new report
- Clinical coding must conform to the published FHIR profiles for the target system

### ExternalMeasurementImport (Aggregate Root)

Manages the ingestion of clinical measurements received from external systems.

**Components**:
- SourceSystem (value object): identifier and version of the originating clinical system
- ReceivedFhirResources: raw FHIR resources as received
- TranslatedMeasurements: internal domain representations derived from the external data
- ImportStatus (value object): received, validated, translated, integrated, rejected

**Invariants**:
- External measurements must pass the same physiological plausibility validation as internally captured measurements
- Source system and original resource identifiers must be preserved for provenance tracking
- Translation failures must not result in corrupted internal domain data

## Domain Events Published

- **ClinicalReportSubmitted**: Raised when a report is successfully transmitted to an external system
- **ClinicalReportAcknowledged**: Raised when the receiving system confirms receipt
- **ClinicalReportRejected**: Raised when the receiving system rejects the report, carrying rejection reasons
- **ExternalMeasurementImported**: Raised when externally sourced measurements are successfully translated and integrated
- **TranslationFailed**: Raised when FHIR-to-internal translation fails, preserving the original data for diagnostic review

## Domain Services

- **FhirTranslationService**: Translates between internal domain representations and FHIR R4 resources in both directions
- **ClinicalCodingService**: Maps internal measurement types to LOINC codes and SNOMED CT terms using maintained mapping tables
- **FhirValidationService**: Validates constructed FHIR resources against profiles and conformance requirements before transmission
- **ProvenanceTrackingService**: Records the complete transformation chain from source data through translation to final internal representation

## Integration Points

- **Upstream**: Consumes HealthScoreCalculated events from Health Scoring, BiometricMeasurementRecorded from Biometric Collection, and CompositionProfileUpdated from Body Composition
- **Downstream**: Publishes ExternalMeasurementImported events to Biometric Collection Context via Open Host Service
- **External**: Communicates with EHR systems, clinical data warehouses, and health information exchanges via FHIR APIs

## Ubiquitous Language (Context-Specific)

- **Clinical Observation**: A measurement or assessment result formatted with clinical coding for consumption by healthcare systems
- **FHIR Resource**: A standardized data structure conforming to the HL7 FHIR specification for health data interoperability
- **LOINC Code**: A universal identifier that uniquely specifies a type of clinical observation or measurement
- **UCUM Unit**: A standardized unit of measure from the Unified Code for Units of Measure system used in clinical data exchange
- **Conformance Profile**: A set of constraints on FHIR resources specifying required fields, allowed values, and structural rules for a specific use case
- **Data Provenance**: The recorded history of a data element's origin, transformations, and chain of custody across system boundaries

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- HL7 FHIR R4 Specification. https://www.hl7.org/fhir/
- LOINC (Logical Observation Identifiers Names and Codes). https://loinc.org/
- Benson, Tim and Grieve, Grahame. *Principles of Health Interoperability: FHIR, HL7 and SNOMED CT*. 4th ed. Springer, 2021.
