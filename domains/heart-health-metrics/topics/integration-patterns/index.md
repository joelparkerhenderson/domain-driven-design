# Integration Patterns

Integration patterns in the heart health metrics domain define how bounded contexts communicate with each other and with external systems. These patterns govern data exchange, dependency management, and translation strategies across the cardiac monitoring architecture.

## Overview

DDD integration patterns, as cataloged by Eric Evans and elaborated by Vaughn Vernon, provide a vocabulary for describing the relationships between bounded contexts. In the heart health metrics domain, these patterns determine how cardiac data flows from devices through analysis to clinical systems, how teams coordinate model changes, and how external system dependencies are managed.

Choosing the right integration pattern for each relationship is a strategic decision that affects system resilience, team autonomy, and maintainability. The heart health metrics domain uses multiple integration patterns simultaneously, selecting the most appropriate pattern for each context relationship based on coupling requirements, team structures, and data flow characteristics.

## Key Concepts

**Published Language.** A well-documented, shared data format used for inter-context communication. The heart health metrics domain defines published languages for cardiac measurements, analysis findings, and risk assessments. HL7 FHIR serves as a published language for clinical integration. Published languages decouple contexts by providing a stable contract that both sides agree upon.

**Open Host Service.** A bounded context exposes its functionality through a well-defined protocol or API that any consuming context can use. The Cardiac Analysis Context may expose an open host service for querying analysis results. The Risk Assessment Context may expose an open host service for requesting risk score calculations.

**Shared Kernel.** A small, explicitly defined subset of the domain model shared between two bounded contexts. In the heart health metrics domain, the Cardiac Analysis Context and the Monitoring and Alerts Context share a kernel of value objects defining cardiac measurement types, severity levels, and clinical threshold categories. Changes to the shared kernel require coordination between both teams.

**Customer-Supplier.** An upstream context (supplier) provides data or services that a downstream context (customer) depends on. The Data Collection Context is a supplier to the Cardiac Analysis Context. The supplier accommodates reasonable requests from the customer and provides advance notice of changes. The customer accepts the supplier's model without attempting to influence it beyond agreed-upon needs.

**Conformist.** A downstream context adopts the upstream context's model without translation. The Clinical Integration Context may adopt a conformist stance toward external EHR systems, using their FHIR resource definitions as-is rather than negotiating changes. This pattern is appropriate when the upstream system is an industry standard or a dominant platform.

**Anti-Corruption Layer.** A translation layer that converts between an external system's model and the domain's internal model. The Data Collection Context uses ACLs for each device vendor. The Clinical Integration Context uses ACLs for each EHR integration point. ACLs prevent external concepts from corrupting the domain model.

**Separate Ways.** When two contexts have no meaningful integration need, they proceed independently. Within the heart health metrics domain, certain administrative functions (device inventory management, user account management) may follow separate ways from the clinical monitoring contexts.

## Domain Examples

The Data Collection Context acts as a customer-supplier to the Cardiac Analysis Context. When the analysis team needs ECG data normalized at a specific sampling rate, they negotiate with the data collection team to add a resampling option to the normalized output. The data collection team implements the change and updates the published language specification.

The Clinical Integration Context adopts a conformist pattern toward the hospital's Epic EHR FHIR server. Rather than negotiating custom resource profiles, the integration team conforms to Epic's supported FHIR R4 profiles for Observation, DiagnosticReport, and RiskAssessment resources. This simplifies integration at the cost of some model fidelity.

The Cardiac Analysis Context and Monitoring and Alerts Context maintain a shared kernel of approximately 15 value objects representing cardiac metric types and severity classifications. When the analysis team adds a new arrhythmia type, both teams coordinate to update the shared kernel and release compatible versions simultaneously.

## Relationships to Other Topics

- [Context Map](../context-map/index.md) - Integration patterns are depicted on the context map.
- [Anti-Corruption Layer](../anti-corruption-layer/index.md) - One of the integration patterns in detail.
- [Bounded Contexts](../bounded-contexts/index.md) - Integration patterns connect bounded contexts.
- [Event-Driven Architecture](../event-driven-architecture/index.md) - Events are a mechanism for implementing integration patterns.
- [Clinical Integration Context](../clinical-integration-context/index.md) - Uses multiple integration patterns for EHR connectivity.
- [Data Collection Context](../data-collection-context/index.md) - Customer-supplier relationship with downstream contexts.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 14 on model integrity patterns.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 13 on integrating bounded contexts.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly, 2021. Chapter 9 on integration patterns.
- Hohpe, Gregor and Woolf, Bobby. *Enterprise Integration Patterns*. Addison-Wesley, 2003.
- Benson, Tim. *Principles of Health Interoperability: HL7 and SNOMED*. Springer, 2012.
