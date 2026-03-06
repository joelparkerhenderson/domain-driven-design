# Integration Patterns - Sleep Health Metrics

## Overview

Integration patterns define how bounded contexts communicate and share data within the sleep health metrics domain and with external systems. Domain-Driven Design identifies several integration patterns -- shared kernel, published language, open host service, customer-supplier, conformist, and anti-corruption layer -- each appropriate for different relationship dynamics. The sleep health metrics domain employs multiple patterns based on the nature of each inter-context relationship.

## Customer-Supplier Pattern

The customer-supplier pattern governs relationships where an upstream context supplies data that a downstream context depends on. The downstream context (customer) defines its requirements, and the upstream context (supplier) commits to meeting them.

### Sleep Data Collection (Supplier) --> Sleep Stage Analysis (Customer)

The Sleep Stage Analysis Context requires validated 30-second epoch data with specific channel configurations. The Sleep Data Collection Context supplies this data in a format that meets the analysis context's requirements. When the analysis context needs additional channel metadata or different sampling rate normalization, it negotiates these requirements with the collection context.

### Sleep Stage Analysis (Supplier) --> Sleep Quality Assessment (Customer)

The Sleep Quality Assessment Context requires scored epoch data, sleep architecture summaries, arousal indices, and respiratory event counts. The Sleep Stage Analysis Context delivers these in a defined contract format. Changes to the scoring model (e.g., adopting a new AASM rule version) are coordinated between the two teams to ensure the quality assessment context can handle the updated output.

### Sleep Quality Assessment (Supplier) --> Intervention Tracking (Customer)

The Intervention Tracking Context requires quality metrics to evaluate intervention efficacy. It consumes sleep efficiency, latency, WASO, PSQI scores, and AHI values from the Sleep Quality Assessment Context. The intervention context defines the minimum metric set it needs; the quality assessment context ensures these are always available.

## Partnership Pattern

The partnership pattern governs relationships where two contexts have mutual dependencies and co-evolve their models cooperatively.

### Circadian Rhythm <--> Sleep Quality Assessment

These two contexts operate as partners. The Circadian Rhythm Context provides chronotype data and circadian phase markers that enrich quality assessments. The Sleep Quality Assessment Context provides sleep timing data that the Circadian Rhythm Context uses for phase alignment calculations. Neither context is purely upstream or downstream; they coordinate model changes jointly.

## Conformist Pattern

The conformist pattern applies when a downstream context adopts the upstream context's model without modification, accepting whatever the upstream provides.

### Circadian Rhythm (Upstream) --> Intervention Tracking (Conformist)

The Intervention Tracking Context conforms to the Circadian Rhythm Context's model for circadian phase data. It uses chronotype classifications, DLMO timing, and phase alignment metrics exactly as the Circadian Rhythm Context defines them, without translating or adapting the model.

## Published Language Pattern

The published language pattern uses a well-documented, shared data format for communication between contexts. In the sleep health metrics domain, published language is used at several boundaries:

- **Domain events**: All inter-context domain events use a published schema that is versioned and documented. Event schemas constitute the published language between producer and consumer contexts.
- **FHIR R4**: The Clinical Integration Context uses FHIR R4 as the published language for external system integration. FHIR resource profiles, extensions, and value sets define the language.
- **LOINC codes**: Sleep health observations use LOINC (Logical Observation Identifiers Names and Codes) as the published language for clinical measurement types.
- **ICD-10-CM codes**: Sleep disorder diagnoses use ICD-10-CM as the published language for condition classification.

## Open Host Service Pattern

The open host service pattern exposes a bounded context's functionality through a well-defined, stable protocol that multiple consumers can use without bilateral negotiation.

### Clinical Integration Context as Open Host

The Clinical Integration Context exposes a FHIR-compliant API as an open host service. External EHR systems, health information exchanges, research platforms, and patient portals consume this service using standard FHIR operations (read, search, create) without needing to understand the internal domain model. The open host service translates between the rich internal model and the standardized FHIR representation.

## Shared Kernel Pattern

The shared kernel pattern designates a small set of domain objects that are jointly owned by two or more contexts. Changes to the shared kernel require agreement from all participating contexts.

In the sleep health metrics domain, the shared kernel is minimal:

- **PatientIdentifier**: A value object representing the patient's unique identifier, shared across all six bounded contexts to ensure consistent patient reference.
- **StudyIdentifier**: A value object representing a unique sleep study identifier, shared between Sleep Data Collection, Sleep Stage Analysis, Sleep Quality Assessment, and Clinical Integration Contexts.
- **DateRange**: A value object representing a temporal range, used consistently across contexts for querying and filtering.

The shared kernel is deliberately kept small to minimize coupling. Domain-specific concepts (e.g., what constitutes a "sleep episode" or how "quality" is defined) are not shared but are context-specific.

## Anti-Corruption Layer Pattern

Anti-corruption layers are documented in detail in the [Anti-Corruption Layer](../anti-corruption-layer/) topic. They are used at boundaries with external systems to prevent foreign models from contaminating internal domain concepts.

## Integration Governance

Integration patterns are governed by the context map, which documents all relationships and their types. When a new integration is needed or an existing pattern requires modification, the affected bounded context teams coordinate changes through shared documentation and versioned contracts. Breaking changes to published event schemas follow a deprecation protocol with backward compatibility windows.

## References

- Evans, E. (2003). *Domain-Driven Design*. Addison-Wesley. Chapter 14: Maintaining Model Integrity.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley. Chapter 3: Context Maps.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media. Chapter 7: Context Mapping Patterns.
- Hohpe, G., Woolf, B. (2003). *Enterprise Integration Patterns*. Addison-Wesley.
