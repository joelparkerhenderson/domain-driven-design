# Anti-Corruption Layer - Asthma Domain

## Overview

An anti-corruption layer (ACL) is a translation boundary that protects a bounded context from the concepts, terminology, and models of external systems or other bounded contexts. In the asthma management domain, ACLs are critical because the system must integrate with diverse external systems -- electronic health records (EHRs), air quality services, pharmacy systems, and laboratory information systems -- each with its own model that may conflict with the asthma domain model.

Without anti-corruption layers, external system models would leak into the domain, contaminating the ubiquitous language and creating fragile coupling. The ACL translates between the external model and the internal domain model, ensuring that each bounded context maintains its integrity.

## ACL Design Principles

**Model Isolation.** The anti-corruption layer ensures that changes to external system APIs, data formats, or terminologies do not propagate into the domain model. The domain model evolves based on domain requirements, not external system constraints.

**Bidirectional Translation.** ACLs translate both inbound data (external to domain) and outbound data (domain to external). Inbound translation maps external representations to domain objects. Outbound translation maps domain objects to external representations.

**Semantic Mapping.** Translation goes beyond data format conversion. An ACL maps semantic meaning between models. For example, an external EHR system may represent asthma severity as a coded value (ICD-10 J45.x subcodes), while the domain model represents severity as a GINA classification value object with richer semantics.

## ACLs in the Asthma Domain

### EHR Integration ACL (Clinical Assessment Context)

The Clinical Assessment Context integrates with electronic health record systems that use HL7 FHIR or CDA standards. The ACL translates between FHIR resource representations and domain objects.

Translations:
- FHIR Observation (spirometry) --> SpirometrySession aggregate with validated FEV1, FVC, and PEF value objects.
- FHIR DiagnosticReport --> AssessmentResult entity with GINA classification.
- FHIR Condition (J45.x) --> AsthmaSeverity value object with full GINA semantics.
- Domain SpirometrySession --> FHIR Observation for EHR persistence.

The ACL rejects FHIR resources that do not meet domain validation rules (for example, spirometry observations missing calibration metadata or patient effort quality grades).

### Air Quality Service ACL (Trigger Monitoring Context)

The Trigger Monitoring Context integrates with external air quality data providers (EPA AirNow API, commercial providers like BreezoMeter or IQAir). Each provider uses different data formats, geographic resolution, pollutant categorizations, and update frequencies.

Translations:
- EPA AirNow AQI response --> AirQualityReading value object with standardized pollutant breakdown.
- Commercial provider JSON --> AirQualityReading with normalized geographic coordinates and timestamp.
- Pollen count service response --> PollenReading value object with standardized allergen taxonomy.

The ACL normalizes varying provider formats into a consistent domain model, abstracting away provider-specific quirks. If a provider changes its API, only the ACL adapter changes.

### Pharmacy System ACL (Medication Management Context)

The Medication Management Context exchanges prescription data with pharmacy systems using NCPDP SCRIPT or HL7 FHIR MedicationRequest standards.

Translations:
- Domain Prescription --> NCPDP SCRIPT NewRx message for electronic prescribing.
- NCPDP SCRIPT RxFill notification --> RefillConfirmation value object.
- FHIR MedicationDispense --> DispensingRecord entity.
- Domain MedicationRegimen --> FHIR MedicationStatement for patient medication summary.

The ACL handles differences in drug coding systems (RxNorm, NDC, SNOMED CT) and translates between the pharmacy system's medication model and the domain's medication model.

### Laboratory Information System ACL (Clinical Assessment Context)

The Clinical Assessment Context receives FeNO test results and blood eosinophil counts from laboratory information systems.

Translations:
- HL7 ORU message (lab result) --> FeNOReading value object with ppb value and interpretation.
- FHIR Observation (eosinophil count) --> EosinophilCount value object with reference range.

The ACL validates that laboratory results include required metadata (specimen type, reference range, units) and rejects malformed results.

### Insurance and Payer ACL (Medication Management Context)

The Medication Management Context integrates with insurance systems for prior authorization of biologic agents and specialty medications.

Translations:
- Domain BiologicPrescription --> Prior authorization request in payer-specific format.
- Payer authorization response --> AuthorizationResult value object (approved, denied, pending).
- Formulary data --> FormularyStatus value object indicating coverage tier and restrictions.

## ACL Implementation Patterns

**Adapter Pattern.** Each external system integration uses an adapter that implements a domain-defined port (interface). The adapter handles protocol-specific communication and data format translation. Multiple adapters can implement the same port for different external systems.

**Translator Objects.** Dedicated translator objects handle the semantic mapping between external and domain representations. Translators are stateless and testable in isolation.

**Facade Pattern.** For complex external systems with large APIs, a facade simplifies the interface to only the operations the domain requires. The Clinical Assessment Context does not need the full FHIR API surface; it needs only spirometry observation read/write operations.

**Event-Based ACL.** For asynchronous integrations, the ACL subscribes to external system events, translates them into domain events, and publishes them within the bounded context. Air quality alerts from external services are translated into TriggerAlert domain events.

## Testing Strategy

Anti-corruption layers require dedicated testing:

- **Contract Tests.** Verify that the ACL correctly translates known external system responses into expected domain objects.
- **Rejection Tests.** Verify that the ACL rejects malformed or incomplete external data.
- **Round-Trip Tests.** For bidirectional ACLs, verify that domain objects translated to external format and back produce equivalent domain objects.
- **Provider Change Tests.** Verify that ACL adapters can be swapped or updated without affecting domain logic.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 14 on anti-corruption layers.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 13 on integrating bounded contexts.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 9 on anti-corruption layer pattern.
- HL7 FHIR Specification. https://www.hl7.org/fhir/
