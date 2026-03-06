# Context Map - Sleep Health Metrics

## Overview

A context map provides a visual and conceptual representation of the relationships and interactions between bounded contexts within a domain. For the sleep health metrics domain, the context map documents how six bounded contexts exchange data, which contexts are upstream or downstream, and what integration patterns govern each relationship. This map is essential for understanding data flow from raw sleep signal acquisition through clinical reporting.

## Context Relationships

### Sleep Data Collection --> Sleep Stage Analysis

Relationship type: Customer-Supplier. The Sleep Data Collection Context is the upstream supplier of validated epoch segments to the Sleep Stage Analysis Context, which is the downstream customer. The analysis context defines what data format and quality it requires; the collection context fulfills those requirements. Data is transmitted as validated 30-second epoch segments with channel-specific signal quality indicators.

### Sleep Stage Analysis --> Sleep Quality Assessment

Relationship type: Customer-Supplier. The Sleep Stage Analysis Context supplies scored epoch data, sleep architecture summaries, arousal indices, and respiratory event counts to the Sleep Quality Assessment Context. The quality context uses these to compute sleep efficiency, latency, WASO, and composite indices. The downstream context conforms to the scoring model defined by the upstream context.

### Circadian Rhythm <--> Sleep Quality Assessment

Relationship type: Partnership. These two contexts operate as partners, exchanging data bidirectionally. The Circadian Rhythm Context provides chronotype classification, circadian phase markers (DLMO timing), and social jet lag estimates to enrich quality assessments. The Sleep Quality Assessment Context shares sleep timing data (actual sleep onset, offset) that the Circadian Rhythm Context uses for phase alignment calculations.

### Sleep Quality Assessment --> Intervention Tracking

Relationship type: Customer-Supplier. The Sleep Quality Assessment Context publishes quality metrics and trend data that the Intervention Tracking Context consumes to evaluate intervention efficacy. When sleep efficiency improves or WASO decreases following a CBT-I protocol adjustment, the intervention context captures this outcome. The intervention context conforms to the quality context's metric definitions.

### Circadian Rhythm --> Intervention Tracking

Relationship type: Conformist. The Intervention Tracking Context conforms to the Circadian Rhythm Context's model for circadian phase data. Light therapy timing, melatonin administration schedules, and chronotherapy protocols in the intervention context depend on circadian phase markers produced upstream.

### All Contexts --> Clinical Integration

Relationship type: Anti-Corruption Layer. The Clinical Integration Context sits downstream of all other contexts and translates their internal models into external clinical standards (FHIR R4, HL7, CDA). It uses anti-corruption layers to prevent external EHR data models, insurance system schemas, and device manufacturer APIs from contaminating the internal domain models.

### External Systems --> Sleep Data Collection

Relationship type: Anti-Corruption Layer. The Sleep Data Collection Context protects its internal model from the proprietary data formats of polysomnography equipment manufacturers, wearable device APIs, and actigraphy systems. An anti-corruption layer translates vendor-specific signal formats into the domain's canonical channel representation.

## Data Flow Summary

The primary data flow follows a pipeline pattern:

1. Raw signals enter through the Sleep Data Collection Context from PSG equipment, actigraphs, wearables, and patient diaries.
2. Validated epoch data flows downstream to the Sleep Stage Analysis Context for scoring.
3. Scored data flows to the Sleep Quality Assessment Context for metric computation.
4. Quality metrics flow to the Intervention Tracking Context for treatment evaluation.
5. Circadian data enriches both quality assessment and intervention planning.
6. The Clinical Integration Context aggregates data from all contexts for external reporting.

## Integration Mechanisms

- Published Language: All inter-context communication uses domain events with well-defined schemas.
- Domain Events: Asynchronous events (SleepStudyRecorded, EpochsScored, MetricsCalculated, InterventionUpdated) propagate changes across boundaries.
- Shared Kernel: Minimal shared types exist only where absolutely necessary (PatientIdentifier, StudyIdentifier) and are jointly owned.
- Open Host Service: The Clinical Integration Context exposes a FHIR-compliant API as an open host service for external consumers.

## Conflict Resolution

When bounded context models conflict, the context map specifies which model prevails. For example, the Sleep Stage Analysis Context owns the definition of what constitutes a valid sleep stage transition. If the Sleep Quality Assessment Context receives staged data that seems inconsistent, it does not re-score; it raises a domain event for review rather than overriding the upstream model.

## Evolution

The context map is a living document. As sleep medicine standards evolve (e.g., AASM scoring manual revisions), new device types emerge, or regulatory requirements change, the relationships and integration patterns must be reassessed. Adding new bounded contexts -- such as a future Sleep Disorder Diagnosis Context -- would require updating this map.

## References

- Evans, E. (2003). *Domain-Driven Design*. Addison-Wesley. Chapter 14: Maintaining Model Integrity.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley. Chapter 3: Context Maps.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media. Chapter 7: Context Mapping.
- Brandolini, A. (2021). Context mapping patterns in strategic DDD design.
