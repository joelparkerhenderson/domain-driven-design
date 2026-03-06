# Anti-Corruption Layer in the Respirology Domain

## Overview

An anti-corruption layer (ACL) is a translation boundary that protects a bounded context from the models, assumptions, and data structures of external systems or other bounded contexts. In the respirology domain, anti-corruption layers are essential because the system must integrate with hospital information systems, laboratory systems, medical device interfaces, and imaging platforms, each of which uses its own data model and terminology. Without anti-corruption layers, external models would leak into the respirology domain, distorting the ubiquitous language and degrading model clarity.

## Purpose

The anti-corruption layer serves as an interpreter between two models. It accepts data in the external system's format, translates it into the internal model's terms, and presents it as if it originated natively within the bounded context. This translation preserves model integrity and ensures that changes in external systems do not cascade into the respirology domain model.

In respirology, the stakes are high. A laboratory system might represent arterial blood gas results using different field names, units, or reference ranges than those used in the Respiratory Diagnostics Context. An anti-corruption layer ensures that regardless of how the external system structures ABG data, the Respiratory Diagnostics Context always works with its own well-defined ABG value objects.

## Anti-Corruption Layers in the Respirology Domain

### Hospital ADT System to Clinical Assessment Context

The hospital admission, discharge, and transfer (ADT) system provides patient movement data in HL7 v2 or FHIR formats. The anti-corruption layer translates ADT messages into the Clinical Assessment Context's internal patient and encounter representations. It maps external patient identifiers to internal references, converts admission types to assessment-relevant categories, and filters irrelevant ADT events.

### Laboratory Information System to Respiratory Diagnostics Context

The laboratory information system (LIS) delivers test results using its own coding system and data structures. The anti-corruption layer translates laboratory result formats into the Respiratory Diagnostics Context's diagnostic result value objects. It maps LOINC codes to internal test type enumerations, converts units where necessary, and applies reference range interpretations specific to the respirology context.

### Medical Device APIs to Ventilatory Support Context

Ventilator manufacturers expose device data through proprietary APIs with manufacturer-specific parameter names, units, and alarm codes. The anti-corruption layer normalizes data from multiple manufacturers into the Ventilatory Support Context's unified ventilator configuration model. It translates manufacturer-specific mode names to the context's standardized VentilationMode value objects and maps proprietary alarm codes to domain-defined alarm categories.

### Imaging Systems to Respiratory Diagnostics Context

Radiology information systems and PACS deliver imaging reports using DICOM metadata and radiology-specific report formats. The anti-corruption layer translates imaging reports into the Respiratory Diagnostics Context's imaging result model, extracting relevant findings and mapping radiology terminology to the respirology ubiquitous language.

### Pharmacy System to Airway Management Context

The pharmacy system provides medication data using its own drug coding and dosing conventions. The anti-corruption layer translates pharmacy medication records into the Airway Management Context's bronchodilator therapy model, mapping drug codes to the context's medication value objects and converting dosing formats to internal representations.

## Design Principles

### Isolation

The anti-corruption layer is the only point of contact between the external system and the bounded context. No part of the bounded context's domain model should directly reference external system types, identifiers, or data structures.

### Bidirectional Translation

Some anti-corruption layers must translate in both directions. When the Ventilatory Support Context sends parameter changes to a ventilator, the ACL must translate internal commands into the manufacturer-specific format. Bidirectional ACLs are more complex and require careful testing.

### Failure Handling

Anti-corruption layers must handle external system failures gracefully. When a laboratory system is unavailable, the ACL should not propagate infrastructure errors into the domain model. Instead, it should signal unavailability through domain-appropriate mechanisms such as pending result states.

### Versioning

External systems evolve their APIs and data formats. The anti-corruption layer absorbs these changes, shielding the domain model from external versioning concerns. When a laboratory system upgrades from HL7 v2 to FHIR, only the ACL needs to change.

## Implementation Patterns

Anti-corruption layers in the respirology domain typically use the following patterns:

- **Adapter Pattern**: Translates external interfaces into internal interfaces without modifying either side.
- **Facade Pattern**: Provides a simplified interface to a complex external system.
- **Translator Objects**: Dedicated classes that convert between external and internal value objects.
- **Gateway Pattern**: Encapsulates communication protocol details (REST, HL7 messaging, device protocols) behind a clean internal API.

## References

- Evans, E. (2003). *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley. Chapter 14 on anti-corruption layers.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley. Chapter 3 on context mapping and integration.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media. Chapter 8 on protecting bounded context boundaries.
