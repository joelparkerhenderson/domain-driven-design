# Context Map

The context map for the heart health metrics domain provides a visual and structural representation of how the six bounded contexts interact, depend on each other, and exchange information. It documents upstream/downstream relationships, integration styles, and translation boundaries.

## Overview

A context map, as described by Eric Evans, makes explicit the relationships between bounded contexts in a system. In the heart health metrics domain, the context map reveals how cardiac data flows from device sensors through analysis pipelines, risk models, monitoring systems, reports, and ultimately into clinical systems. Each relationship is characterized by its integration pattern and the degree of coupling between contexts.

Understanding these relationships is critical for managing change across the cardiac monitoring system. When a new ECG device format is introduced, the context map shows exactly which downstream contexts are affected and which anti-corruption layers must be updated.

## Key Concepts

**Upstream/Downstream Relationships.** The Data Collection Context is upstream of the Cardiac Analysis Context, meaning it produces the normalized measurement data that analysis consumes. The Cardiac Analysis Context is upstream of both the Risk Assessment Context and the Monitoring and Alerts Context.

**Customer-Supplier Pattern.** The Cardiac Analysis Context acts as a supplier to the Risk Assessment Context. The analysis team provides stable APIs for retrieving processed cardiac metrics (HRV results, rhythm classifications, BP trends) that the risk assessment team consumes to compute cardiovascular risk scores.

**Published Language.** The Clinical Integration Context uses HL7 FHIR as a published language for communicating with external EHR systems. This standardized language ensures that cardiac observations, patient demographics, and clinical findings are exchanged in a universally understood format.

**Conformist Pattern.** When integrating with established clinical systems, the Clinical Integration Context may adopt a conformist stance, adapting its models to match the external system's FHIR resource definitions rather than attempting to negotiate changes.

**Anti-Corruption Layer.** The Data Collection Context employs an anti-corruption layer to translate vendor-specific device protocols (proprietary Bluetooth profiles, device SDKs) into the domain's normalized measurement model. This prevents device vendor concepts from leaking into the cardiac analysis domain.

**Shared Kernel.** The Cardiac Analysis Context and the Monitoring and Alerts Context share a small kernel of value objects representing cardiac measurement types, severity classifications, and clinical thresholds. Changes to this shared kernel require coordination between both teams.

## Domain Examples

Data flows from a wearable heart rate sensor through the Data Collection Context, which normalizes the PPG-derived heart rate readings. The Cardiac Analysis Context receives these readings as a downstream consumer and performs HRV computation. When the analysis produces an anomaly finding, the Monitoring and Alerts Context receives a domain event and evaluates it against alert rules. If an alert is triggered, the Clinical Integration Context translates the alert into a FHIR Communication resource for delivery to the hospital EHR.

A Reporting and Visualization Context acts as a downstream consumer of multiple contexts, aggregating cardiac analysis results, risk scores, and alert histories to produce comprehensive patient reports.

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/index.md) - The entities connected by the context map.
- [Strategic Design](../strategic-design/index.md) - Context mapping is a core strategic design activity.
- [Anti-Corruption Layer](../anti-corruption-layer/index.md) - Translation boundaries shown on the context map.
- [Integration Patterns](../integration-patterns/index.md) - Patterns used in context relationships.
- [Domain Events](../domain-events/index.md) - Primary mechanism for cross-context data flow.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 14 on context maps.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 3 on context maps.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly, 2021. Chapter 7 on context mapping patterns.
- Brandolini, Alberto. "Introducing EventStorming." Leanpub, 2021.
- HL7 FHIR Specification. https://www.hl7.org/fhir/
