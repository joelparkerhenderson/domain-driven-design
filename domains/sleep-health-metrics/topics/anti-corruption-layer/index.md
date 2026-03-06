# Anti-Corruption Layer - Sleep Health Metrics

## Overview

An anti-corruption layer (ACL) is a translation boundary that prevents external system models from contaminating the internal domain model. In the sleep health metrics domain, anti-corruption layers are critical at integration points where proprietary device formats, legacy EHR data models, insurance system schemas, and third-party API conventions differ substantially from the domain's ubiquitous language and model structure.

## Purpose in Sleep Health Metrics

Sleep health systems interact with numerous external systems, each carrying its own terminology, data structures, and assumptions. A polysomnography device manufacturer may represent sleep stages as numeric codes (0-5), while the domain model uses AASM-defined stage labels (W, N1, N2, N3, R). A wearable device API may report "deep sleep minutes" as a single number, while the domain model requires epoch-level N3 data. An EHR system may represent a sleep study as a flat diagnostic report, while the domain model maintains a rich aggregate with nested entities and value objects.

Without anti-corruption layers, these external representations would leak into the domain model, creating semantic confusion and tight coupling to external system changes.

## ACL at the Device Integration Boundary

The Sleep Data Collection Context employs an anti-corruption layer to translate between proprietary device data formats and the domain's canonical channel representation.

Translations handled by this ACL include:

- Polysomnography systems: Manufacturer-specific EDF/EDF+ file formats, proprietary channel naming conventions (e.g., "EEG1" vs. "C3-M2"), varying sampling rates, and device-specific metadata are translated into the domain's standardized channel model with AASM-compliant derivation names.
- Wearable sensors: Consumer device APIs (Fitbit, Apple Watch, Oura, Withings) report sleep data in simplified formats (total sleep, light/deep/REM categories, heart rate) that are translated into the domain's measurement model without assuming clinical-grade accuracy.
- Actigraphy devices: Proprietary activity count formats and vendor-specific epoch lengths (15-second, 30-second, 60-second) are normalized to the domain's 30-second epoch standard.

## ACL at the EHR Integration Boundary

The Clinical Integration Context maintains an anti-corruption layer between the domain model and external EHR systems. Each EHR vendor (Epic, Cerner, Meditech, Allscripts) has its own data model for representing sleep studies, diagnostic impressions, and treatment plans.

Translations handled by this ACL include:

- FHIR R4 resource mapping: Internal SleepStudy aggregates are mapped to FHIR DiagnosticReport, Observation (for individual metrics like AHI, sleep efficiency), Condition (for diagnosed sleep disorders), and Procedure (for interventions).
- Legacy HL7 v2 messaging: Older EHR interfaces using HL7 v2 ORU messages require translation of the domain's structured sleep study results into segment-based message formats.
- Clinical document architecture (CDA): Sleep study reports destined for health information exchanges are translated into CDA-compliant documents.

## ACL at the Insurance Boundary

The Intervention Tracking Context uses an anti-corruption layer when exchanging data with insurance and payer systems. Insurance platforms define CPAP adherence using specific criteria (CMS defines compliance as usage of at least 4 hours per night on 70 percent of consecutive 30-day periods). These external compliance definitions are translated at the boundary rather than embedded in the domain model, which tracks granular adherence data independently.

## ACL Design Principles

Anti-corruption layers in the sleep health metrics domain follow these principles:

- **Isolation**: The ACL exists as a distinct layer; no domain logic resides within it.
- **Bidirectional translation**: The ACL translates both inbound (external to domain) and outbound (domain to external) data.
- **No domain leakage**: External concepts (vendor IDs, proprietary codes, API pagination tokens) never appear in the domain model.
- **Graceful degradation**: When an external system provides incomplete data (e.g., a wearable omitting REM data), the ACL produces a domain representation that explicitly models the missing information rather than defaulting silently.
- **Version tolerance**: The ACL absorbs external API version changes, insulating the domain model from breaking changes in device firmware updates or EHR API revisions.

## Implementation Considerations

Each ACL is implemented as a set of translators (also called adapters or mappers) that convert between external and internal representations. These translators are co-located with the bounded context they protect but are architecturally distinct from the domain layer. They belong to the infrastructure or application layer, not the domain layer, preserving the purity of domain logic.

## References

- Evans, E. (2003). *Domain-Driven Design*. Addison-Wesley. Chapter 14: Maintaining Model Integrity -- Anti-Corruption Layer.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley. Chapter 3: Context Maps and ACL patterns.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media. Chapter 8: Anti-Corruption Layer pattern.
- HL7 International. (2019). *FHIR R4 Specification*. https://hl7.org/fhir/R4/
