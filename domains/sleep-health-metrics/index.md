# Sleep Health Metrics - Domain Driven Design

## Overview

The Sleep Health Metrics domain applies Domain-Driven Design principles to the complex landscape of sleep health monitoring, assessment, and intervention. Sleep medicine encompasses objective physiological measurement, subjective patient reporting, circadian biology, therapeutic intervention, and clinical data exchange. This domain model captures these concerns within clearly bounded contexts, using ubiquitous language drawn from sleep medicine, and structures the business logic using strategic and tactical DDD patterns.

The domain serves sleep clinicians, sleep technologists, researchers, and patients by providing a coherent model for collecting sleep data from multiple sources (polysomnography, actigraphy, wearable sensors, sleep diaries), analyzing sleep stages and architecture, computing standardized quality metrics, tracking circadian rhythms, managing interventions (CBT-I, CPAP, medications), and integrating with clinical information systems via standards such as FHIR R4.

## Bounded Contexts

The domain is decomposed into six bounded contexts, each with a well-defined model and clear boundaries:

1. **Sleep Data Collection Context** - Manages the ingestion, validation, and storage of raw sleep data from polysomnography, actigraphy, wearable sensors, and patient sleep diaries. This context owns the canonical representation of recorded sleep signals.

2. **Sleep Stage Analysis Context** - Performs epoch-by-epoch sleep stage scoring following AASM criteria, computes sleep architecture metrics, detects arousals and respiratory events, and produces hypnograms. Consumes raw signal data from the Sleep Data Collection Context.

3. **Sleep Quality Assessment Context** - Calculates standardized sleep quality metrics including sleep efficiency, sleep latency, WASO, total sleep time, and composite indices such as the Pittsburgh Sleep Quality Index (PSQI) and the Epworth Sleepiness Scale (ESS).

4. **Circadian Rhythm Context** - Tracks circadian phase markers including dim light melatonin onset (DLMO), light exposure patterns, chronotype classifications, and shift work schedules. Models the interaction between circadian biology and sleep timing.

5. **Intervention Tracking Context** - Manages therapeutic interventions for sleep disorders, including CBT-I treatment protocols, sleep hygiene recommendations, medication schedules, and device therapies such as CPAP with adherence monitoring.

6. **Clinical Integration Context** - Handles bidirectional data exchange with electronic health records (EHRs), generates standardized sleep study reports, maps domain concepts to FHIR R4 resources, and manages clinical referral workflows.

## Strategic Design

- [Strategic Design](topics/strategic-design/) - Principles governing domain decomposition and bounded context identification.
- [Bounded Contexts](topics/bounded-contexts/) - Detailed definitions and boundaries for each context.
- [Context Map](topics/context-map/) - Relationships and integration patterns between contexts.
- [Ubiquitous Language](topics/ubiquitous-language/) - Shared terminology from sleep medicine used across the domain.
- [Subdomains](topics/subdomains/) - Classification of domain areas by strategic importance.
- [Anti-Corruption Layer](topics/anti-corruption-layer/) - Translation boundaries protecting internal models from external system concepts.

## Tactical Design

- [Aggregates](topics/aggregates/) - Consistency boundaries grouping related domain objects.
- [Entities](topics/entities/) - Objects with unique identity that persist over time.
- [Value Objects](topics/value-objects/) - Immutable objects defined by their attributes.
- [Domain Events](topics/domain-events/) - Significant occurrences that trigger cross-context actions.
- [Repositories](topics/repositories/) - Abstractions for persisting and retrieving aggregate roots.
- [Domain Services](topics/domain-services/) - Stateless operations spanning multiple aggregates.

## Bounded Context Details

- [Sleep Data Collection Context](topics/sleep-data-collection-context/) - Polysomnography, actigraphy, wearable sensors, sleep diary.
- [Sleep Stage Analysis Context](topics/sleep-stage-analysis-context/) - REM/NREM staging, sleep architecture, arousal detection.
- [Sleep Quality Assessment Context](topics/sleep-quality-assessment-context/) - Sleep efficiency, latency, WASO, PSQI scoring.
- [Circadian Rhythm Context](topics/circadian-rhythm-context/) - Chronotype, light exposure, melatonin cycles, shift work.
- [Intervention Tracking Context](topics/intervention-tracking-context/) - CBT-I, sleep hygiene, medication, CPAP therapy.
- [Clinical Integration Context](topics/clinical-integration-context/) - EHR integration, FHIR data exchange, sleep study reporting.

## Cross-Cutting Concerns

- [Event-Driven Architecture](topics/event-driven-architecture/) - Asynchronous communication between bounded contexts.
- [Integration Patterns](topics/integration-patterns/) - Strategies for inter-context and external system integration.
- [Sleep Health Metrics Standards](topics/sleep-health-metrics-standards/) - AASM, ICSD-3, FHIR R4, and other applicable standards.
- [Regulatory Compliance](topics/regulatory-compliance/) - HIPAA, FDA, GDPR, and governance requirements.

## References

- Evans, E. (2003). *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media.
- Berry, R. B. et al. (2020). *The AASM Manual for the Scoring of Sleep and Associated Events: Rules, Terminology and Technical Specifications*. Version 2.6. American Academy of Sleep Medicine.
- American Academy of Sleep Medicine. (2014). *International Classification of Sleep Disorders*, 3rd Edition (ICSD-3).
- Buysse, D. J. et al. (1989). The Pittsburgh Sleep Quality Index: A New Instrument for Psychiatric Practice and Research. *Psychiatry Research*, 28(2), 193-213.
- Johns, M. W. (1991). A New Method for Measuring Daytime Sleepiness: The Epworth Sleepiness Scale. *Sleep*, 14(6), 540-545.
- HL7 International. (2019). *FHIR R4 Specification*. https://hl7.org/fhir/R4/
