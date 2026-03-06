# Anti-Corruption Layer

The anti-corruption layer (ACL) in the heart health metrics domain provides translation boundaries that protect cardiac domain models from the concepts, naming conventions, and data structures of external systems such as device vendor APIs, legacy hospital systems, and third-party clinical platforms.

## Overview

Eric Evans introduced the anti-corruption layer as a defensive pattern that prevents a bounded context's domain model from being corrupted by external models. In the heart health metrics domain, ACLs are essential because the system integrates with diverse external systems: proprietary device SDKs from wearable manufacturers, legacy hospital information systems with decades-old data models, laboratory information systems with vendor-specific result formats, and national health data exchanges with government-mandated schemas.

Without anti-corruption layers, device vendor terminology would leak into clinical analysis models, legacy system constraints would distort risk assessment logic, and external API changes would ripple uncontrollably through the cardiac monitoring system.

## Key Concepts

**Device Vendor ACL.** The Data Collection Context uses an anti-corruption layer to translate between vendor-specific device protocols and the domain's normalized measurement model. Each device vendor has its own signal format, sampling rate convention, error coding scheme, and metadata structure. The ACL translates these into the domain's Heart Rate Reading, Blood Pressure Reading, and ECG Signal value objects.

**EHR Integration ACL.** The Clinical Integration Context uses an anti-corruption layer to translate between the heart health metrics domain model and external EHR data representations. Even when both sides use FHIR, the ACL handles differences in coding systems (LOINC versus proprietary codes), unit conventions, and observation bundling strategies.

**Legacy System ACL.** When the cardiac monitoring system must coexist with legacy hospital systems that use HL7 v2 messages or custom database schemas, the ACL translates between the legacy representation and the domain model. This prevents legacy naming conventions (cryptic field codes, concatenated data fields) from corrupting the domain's ubiquitous language.

**Translation Mechanisms.** ACLs employ adapters, facades, and translators. An adapter converts external API calls to domain commands. A facade simplifies a complex external interface into domain-friendly operations. A translator maps external data structures to domain value objects and entities.

**Bidirectional Translation.** ACLs in the heart health metrics domain often need to translate in both directions: inbound translation converts external data into domain objects, while outbound translation converts domain objects into formats expected by external systems.

## Domain Examples

A wearable device manufacturer provides heart rate data as a proprietary JSON payload with fields like "hr_bpm," "confidence_pct," and "motion_artifact_flag." The device vendor ACL translates this into a Heart Rate Reading value object with standardized attributes: beats per minute, measurement confidence level, and signal quality indicator. The domain model never references the vendor's field names.

A hospital's legacy cardiology system stores blood pressure readings as pipe-delimited strings ("120|80|mmHg|sitting|left_arm"). The legacy system ACL parses this format and produces a Blood Pressure Reading value object with systolic pressure, diastolic pressure, unit, patient position, and measurement site as properly typed domain attributes.

## Relationships to Other Topics

- [Strategic Design](../strategic-design/index.md) - ACLs are a strategic design pattern for model integrity.
- [Bounded Contexts](../bounded-contexts/index.md) - ACLs protect bounded context boundaries.
- [Context Map](../context-map/index.md) - ACLs appear as translation layers on the context map.
- [Data Collection Context](../data-collection-context/index.md) - Primary consumer of device vendor ACLs.
- [Clinical Integration Context](../clinical-integration-context/index.md) - Primary consumer of EHR integration ACLs.
- [Integration Patterns](../integration-patterns/index.md) - ACLs are one of several integration patterns.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 14 on anti-corruption layers.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 13 on integrating bounded contexts.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly, 2021. Chapter 9 on anti-corruption layers.
- Gamma, Erich et al. *Design Patterns: Elements of Reusable Object-Oriented Software*. Addison-Wesley, 1994. Adapter and Facade patterns.
- Benson, Tim. *Principles of Health Interoperability: HL7 and SNOMED*. Springer, 2012.
