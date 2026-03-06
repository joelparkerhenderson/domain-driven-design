# Integration Patterns for Body Health Metrics

## Overview

Integration patterns define how the bounded contexts in the body health metrics domain interact, share data, and maintain their respective model integrity. Each relationship between contexts follows a specific integration pattern that balances autonomy with collaboration. These patterns, drawn from Domain-Driven Design strategic design, govern data flow, model translation, and organizational coordination across the body health measurement system.

## Patterns in Use

### Shared Kernel

A shared kernel is a small, explicitly defined subset of the domain model that two or more bounded contexts agree to share. Changes to the shared kernel require coordination between the owning teams.

**Application in body health metrics**:

The body health metrics domain maintains a minimal shared kernel containing:

- **PersonId**: The universal identifier for an individual across all contexts
- **MeasurementSessionId**: The identifier linking measurements captured during the same session
- **Timestamp**: A temporal value object with timezone awareness used consistently across contexts
- **UnitOfMeasure**: An enumeration of supported measurement units (kg, lb, cm, m, mmHg, bpm)

This shared kernel is deliberately small. Adding a type to the shared kernel requires agreement from all consuming context teams. The threshold for inclusion is high: only types that every context needs and that have identical semantics across all contexts qualify.

### Published Language

A published language is a well-documented, versioned data format used for communication between contexts. It provides a stable, shared vocabulary for inter-context data exchange without requiring contexts to share their internal models.

**Application in body health metrics**:

- **Domain event schemas**: Each domain event type (BiometricMeasurementRecorded, HealthScoreCalculated, MilestoneAchieved) has a published schema that consuming contexts can rely upon. The schemas are versioned and follow backward-compatible evolution rules.
- **FHIR as external published language**: The Clinical Integration Context uses HL7 FHIR as a published language for communication with external clinical systems. FHIR provides the standard vocabulary, resource structures, and exchange protocols.
- **Measurement data transfer objects**: Standardized representations of measurement data that carry value, unit, precision, and timestamp without exposing internal aggregate structure.

### Customer-Supplier

In a customer-supplier relationship, the upstream context (supplier) provides data that the downstream context (customer) needs. The supplier accommodates reasonable requests from the customer, and both teams negotiate the interface.

**Application in body health metrics**:

- **Biometric Collection (supplier) to Body Composition (customer)**: Body Composition needs specific measurement types (weight, impedance values, skinfold measurements) from Biometric Collection. The teams negotiate which measurements are published and in what format.
- **Biometric Collection (supplier) to Fitness Assessment (customer)**: Fitness Assessment needs baseline vital signs (resting heart rate) and anthropometric data (weight, height) to conduct certain assessment protocols.
- **Health Scoring (supplier) to Progress Tracking (customer)**: Progress Tracking needs composite score values and score change notifications to update goal progress and detect trends.

### Conformist

In a conformist relationship, the downstream context adopts the upstream context's model without negotiation. The downstream team conforms to whatever the upstream provides.

**Application in body health metrics**:

- **Body Composition conforming to Biometric Collection measurement format**: When Biometric Collection changes its measurement event structure, Body Composition adapts. Body Composition does not have sufficient leverage to demand format changes from the measurement capture context.
- **Clinical Integration conforming to FHIR**: The Clinical Integration Context conforms to the FHIR specification. It does not negotiate changes to the FHIR standard but adapts to FHIR releases and profile requirements.

### Anti-Corruption Layer

An anti-corruption layer translates between an external model and the internal domain model, protecting the internal model from external concepts.

**Application in body health metrics**:

- **Clinical Integration Context**: The entire Clinical Integration Context functions as an anti-corruption layer between the body health metrics domain and external clinical systems. It translates FHIR resources to internal domain objects and vice versa.
- **Wearable device integration**: An ACL translates between vendor-specific wearable device data formats and internal measurement value objects.

### Open Host Service

An open host service exposes a bounded context's capabilities through a well-defined, public protocol that any authorized consumer can use.

**Application in body health metrics**:

- **Biometric Collection Open Host Service**: Exposes a standardized interface for submitting measurements, used by both the Clinical Integration Context (for externally sourced measurements) and direct measurement device integrations.
- **Health Scoring Query Service**: Exposes a read-only interface for retrieving current and historical health scores, used by Progress Tracking and Clinical Integration contexts.

## Pattern Selection Guidelines

The choice of integration pattern depends on the relationship between contexts:

- Use **shared kernel** only for truly universal concepts (identifiers, timestamps) that all contexts need identically
- Use **published language** for inter-context events where a stable, versioned contract is needed
- Use **customer-supplier** when both teams can negotiate and the upstream team accommodates downstream needs
- Use **conformist** when the upstream context is outside organizational control (external standards, third-party systems)
- Use **anti-corruption layer** when external models are fundamentally different from internal models
- Use **open host service** when a context needs to serve multiple consumers with a uniform interface

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 14 on context map patterns.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 13 on integration.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 8 on integration patterns.
