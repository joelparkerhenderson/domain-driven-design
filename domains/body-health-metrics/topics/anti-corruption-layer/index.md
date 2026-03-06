# Anti-Corruption Layer for Body Health Metrics

## Overview

The anti-corruption layer (ACL) in the body health metrics domain provides a protective translation boundary between internal bounded contexts and external systems whose models, terminology, and data structures differ from the internal domain model. ACLs prevent external concepts from leaking into the core domain, preserving the integrity of the ubiquitous language and domain model design.

## Primary Anti-Corruption Layers

### Clinical Systems ACL

The most prominent anti-corruption layer sits between the Clinical Integration Context and external Electronic Health Record (EHR) systems. External clinical systems model health data using HL7 FHIR resources, LOINC observation codes, and SNOMED CT clinical terms. These external models are comprehensive but generic -- they serve all of clinical medicine, not specifically body health metrics.

Translation responsibilities:
- Convert internal MeasurementSession aggregates to FHIR Observation resources
- Map internal measurement types to LOINC codes (e.g., body weight maps to LOINC 29463-7)
- Translate internal body composition terminology to SNOMED CT concepts
- Convert internal health scores to FHIR RiskAssessment resources
- Transform FHIR Patient resources into internal PersonProfile value objects

### Wearable Device ACL

An anti-corruption layer mediates between the Biometric Collection Context and external wearable device APIs. Each device manufacturer (fitness trackers, smart scales, heart rate monitors) exposes data in proprietary formats with vendor-specific terminology and measurement conventions.

Translation responsibilities:
- Normalize vendor-specific measurement units to internal SI-based representations
- Map vendor activity classifications to internal fitness assessment categories
- Translate vendor-specific data quality indicators to internal calibration status values
- Convert vendor timestamps and timezone representations to internal temporal models

### Laboratory Systems ACL

When body composition data originates from clinical laboratory systems (DEXA scans, blood-based metabolic panels), an anti-corruption layer translates between laboratory information system formats and the internal Body Composition Context model.

Translation responsibilities:
- Convert laboratory result formats to internal composition profile value objects
- Map laboratory reference ranges to internal normative comparison structures
- Translate laboratory quality control indicators to internal measurement confidence values

## ACL Design Principles

**Isolation**: The anti-corruption layer is the only component that understands both the external model and the internal domain model. No external concepts appear in the core domain model. The internal domain never references FHIR resource types, LOINC codes, or vendor API structures directly.

**Bidirectional translation**: ACLs support both inbound and outbound data flow. Inbound translation converts external data into internal domain objects. Outbound translation converts internal domain events into external data formats.

**Validation at the boundary**: The ACL validates incoming data against internal domain invariants before creating internal domain objects. An external measurement that violates physiological plausibility constraints is rejected at the ACL boundary, not passed through to corrupt the internal model.

**Versioning**: External systems evolve independently. The ACL absorbs version changes in external APIs and data formats, shielding the internal domain from external evolution. When a device vendor releases a new API version, only the ACL adapter changes.

## Implementation Approach

Each ACL is implemented as a set of translator services and adapter interfaces:

- **Translator**: Stateless service that converts between external and internal representations. A FhirObservationTranslator converts FHIR Observation resources to internal Measurement value objects.

- **Adapter**: Interface that abstracts external system communication. A WearableDeviceAdapter provides a uniform interface for retrieving measurement data regardless of the underlying vendor API.

- **Facade**: Simplified interface that the internal domain uses to interact with external capabilities. The internal domain calls ClinicalReportService.submitReport() without awareness of the underlying FHIR resource construction.

## Error Handling

When translation fails due to incompatible data, the ACL records the failure, preserves the original external data for diagnostic purposes, and raises a TranslationFailedEvent rather than allowing corrupted data into the internal model. This approach maintains domain model integrity while providing visibility into integration issues.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 14 on anti-corruption layers.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 13 on integrating bounded contexts.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 9 on anti-corruption layer patterns.
- HL7 FHIR R4 Specification. https://www.hl7.org/fhir/
