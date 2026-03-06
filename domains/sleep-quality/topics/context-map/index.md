# Context Map for the Sleep Quality Domain

## Overview

A context map is a visual and textual representation of the relationships and interactions between bounded contexts within a domain. In the sleep quality domain, the context map reveals how six bounded contexts collaborate, exchange information, and maintain their autonomy. Understanding these relationships is essential for designing integration points and managing dependencies.

## Context Relationships

### Sleep Assessment to Sleep Disorders (Customer-Supplier)

The Sleep Assessment Context acts as a supplier of assessment results to the Sleep Disorders Context, which consumes them as a customer. Assessment data, including questionnaire scores and polysomnography findings, feed into diagnostic workflows. The Disorders Context depends on the Assessment Context for clinical measurements but defines its own criteria for how those measurements inform diagnosis.

### Sleep Assessment to Outcomes Tracking (Customer-Supplier)

The Sleep Assessment Context supplies baseline and periodic assessment results to the Outcomes Tracking Context. These assessment snapshots become data points in longitudinal trend analysis. The Outcomes Tracking Context consumes assessment events but transforms them into its own outcome record model, applying temporal aggregation and trend computation logic.

### Sleep Disorders to Behavioral Interventions (Customer-Supplier)

The Sleep Disorders Context supplies disorder diagnoses to the Behavioral Interventions Context, which selects appropriate intervention protocols based on the diagnosed condition. An insomnia diagnosis triggers CBT-I protocol eligibility. An OSA diagnosis may trigger behavioral adjunct recommendations alongside device-based therapy.

### Device Integration to Sleep Assessment (Conformist)

The Device Integration Context translates raw device data into domain-standard formats. The Sleep Assessment Context conforms to the published data formats produced by Device Integration for actigraphy and wearable-derived sleep metrics. This conformist relationship means Assessment accepts the data model produced by Device Integration without requiring further transformation.

### Device Integration to Sleep Environment (Conformist)

Environmental sensor data flows from Device Integration to the Sleep Environment Context. Ambient sensors provide light, noise, temperature, and humidity readings that the Environment Context consumes. The Environment Context conforms to the standardized sensor data format published by Device Integration.

### Device Integration to Outcomes Tracking (Customer-Supplier)

The Device Integration Context supplies normalized device data to the Outcomes Tracking Context for inclusion in longitudinal analyses. CPAP compliance data, wearable-derived sleep efficiency, and smart mattress movement data contribute to outcome trend computation.

### Behavioral Interventions to Outcomes Tracking (Customer-Supplier)

The Behavioral Interventions Context publishes events about intervention progress, session completion, and adherence metrics. The Outcomes Tracking Context consumes these events to correlate treatment activities with changes in sleep quality outcomes.

### Sleep Environment to Behavioral Interventions (Shared Kernel)

The Sleep Environment and Behavioral Interventions contexts share a small kernel of concepts related to sleep hygiene. Environmental recommendations overlap with sleep hygiene education components of CBT-I. The shared kernel includes a common model for sleep hygiene factors to avoid duplication and inconsistency.

## Anti-Corruption Layers

The Device Integration Context maintains anti-corruption layers at its upstream boundary with external device systems. Each device manufacturer API has its own data model, units, and terminology. The anti-corruption layer translates these proprietary representations into the canonical domain model before data enters the domain.

The Sleep Disorders Context maintains an anti-corruption layer at its boundary with external electronic health record (EHR) systems. EHR systems use ICD-10 codes and their own clinical data models, which must be translated into the ICSD-3-based disorder model used within the domain.

## Upstream and Downstream

The Device Integration Context is the primary upstream context, feeding normalized data to Assessment, Environment, and Outcomes Tracking. The Outcomes Tracking Context is the primary downstream context, consuming events from Assessment, Disorders, Behavioral Interventions, and Device Integration. The Sleep Disorders Context sits in the middle, consuming assessment data and supplying diagnostic information to Behavioral Interventions.

## Open Host Services

The Outcomes Tracking Context exposes an open host service for reporting and analytics. External systems such as clinical dashboards, research databases, and patient-facing applications consume aggregated outcomes data through this published interface. The service defines a published language for outcome metrics that external consumers implement.

## Communication Patterns

All cross-context communication in the sleep quality domain uses asynchronous domain events as the primary mechanism. This event-driven approach decouples contexts temporally and allows each context to process incoming information at its own pace. Synchronous queries are used only for read-only reference data lookups where eventual consistency is insufficient.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley. Chapter 14 on context maps.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley. Chapter 3 on context maps and integration.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media. Chapter 8 on context mapping patterns.
