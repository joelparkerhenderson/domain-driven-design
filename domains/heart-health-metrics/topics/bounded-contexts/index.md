# Bounded Contexts

Bounded contexts in the heart health metrics domain define the logical boundaries within which specific cardiac health models, terminology, and rules are consistent. Each bounded context encapsulates a coherent set of responsibilities and enforces its own invariants independently.

## Overview

A bounded context, as described by Eric Evans, is a boundary within which a particular domain model is defined and applicable. In the heart health metrics domain, bounded contexts separate the concerns of data acquisition from analysis, risk scoring from alerting, and clinical reporting from EHR integration. This separation allows each context to evolve independently, adopt its own persistence strategies, and maintain its own ubiquitous language refinements.

The heart health metrics domain contains six bounded contexts, each representing a distinct area of cardiac health monitoring functionality. These contexts communicate through well-defined interfaces and domain events rather than sharing internal models directly.

## Key Concepts

**Data Collection Context.** Responsible for interfacing with heart rate sensors, blood pressure monitors, ECG devices, and wearable platforms. It normalizes raw device data into standardized measurement streams. Its model includes concepts like Device, Sensor Reading, Signal Channel, and Calibration Profile.

**Cardiac Analysis Context.** Processes normalized cardiac data to extract clinically meaningful features. It performs rhythm classification, heart rate variability computation, QRS morphology analysis, and blood pressure trend detection. Its model includes concepts like ECG Tracing, Rhythm Classification, HRV Analysis Result, and Anomaly Finding.

**Risk Assessment Context.** Evaluates cardiovascular risk using validated scoring algorithms. It incorporates patient demographics, clinical history, biomarker values, and lifestyle factors. Its model includes concepts like Risk Profile, Scoring Model, Comorbidity Factor, and Risk Stratification.

**Monitoring and Alerts Context.** Provides real-time surveillance of cardiac metrics against configurable thresholds. It manages alert rules, notification channels, escalation policies, and acknowledgment workflows. Its model includes concepts like Alert Rule, Threshold Configuration, Notification, and Escalation Chain.

**Reporting and Visualization Context.** Generates trend reports, clinical dashboards, physician summaries, and patient portal views. It aggregates data across time periods and formats output for different audiences. Its model includes concepts like Report Template, Trend Summary, Dashboard Widget, and Export Format.

**Clinical Integration Context.** Handles interoperability with external clinical systems through HL7 FHIR resources, EHR synchronization protocols, and clinical decision support interfaces. Its model includes concepts like FHIR Observation, Clinical Encounter, CDS Hook, and Integration Endpoint.

## Domain Examples

When a patient wears a continuous heart rate monitor, the Data Collection Context receives PPG signals and produces normalized heart rate readings. These readings cross into the Cardiac Analysis Context where HRV metrics are computed. If the analysis detects an irregular rhythm, a domain event crosses into the Monitoring and Alerts Context, which evaluates the finding against the patient's alert rules and may trigger a clinician notification.

When a physician orders a cardiovascular risk assessment, the Risk Assessment Context gathers data from the Cardiac Analysis Context (recent BP trends, resting heart rate) and computes a Framingham or ASCVD risk score. The result is published to the Reporting and Visualization Context for inclusion in the physician's summary report.

## Relationships to Other Topics

- [Strategic Design](../strategic-design/index.md) - Bounded contexts are the primary structural element of strategic design.
- [Context Map](../context-map/index.md) - Documents how bounded contexts relate to each other.
- [Aggregates](../aggregates/index.md) - Each bounded context contains its own aggregate roots.
- [Anti-Corruption Layer](../anti-corruption-layer/index.md) - Protects bounded context models from external corruption.
- [Domain Events](../domain-events/index.md) - Primary mechanism for cross-context communication.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 14 on maintaining model integrity.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 2 on bounded contexts.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly, 2021. Chapter 6 on bounded context boundaries.
- Benson, Tim. *Principles of Health Interoperability: HL7 and SNOMED*. Springer, 2012.
- Mandel, J.C. et al. "SMART on FHIR." *JAMIA*, 23(5), 899-908, 2016.
