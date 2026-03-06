# Sleep Health Metrics - Topics

## Strategic Design Topics

- [Strategic Design](strategic-design/) - Principles and patterns for decomposing the sleep health metrics domain into bounded contexts, identifying subdomains, and establishing the ubiquitous language of sleep medicine.
- [Bounded Contexts](bounded-contexts/) - Definitions and boundaries for the six contexts: Sleep Data Collection, Sleep Stage Analysis, Sleep Quality Assessment, Circadian Rhythm, Intervention Tracking, and Clinical Integration.
- [Context Map](context-map/) - Relationships between bounded contexts, including upstream/downstream dependencies, conformist patterns, and anti-corruption layers.
- [Ubiquitous Language](ubiquitous-language/) - The shared vocabulary of sleep medicine terms used consistently across domain experts, developers, and stakeholders.
- [Subdomains](subdomains/) - Classification of sleep health domain areas into core, supporting, and generic subdomains based on strategic importance.
- [Anti-Corruption Layer](anti-corruption-layer/) - Translation boundaries that protect internal sleep health models from concepts in external systems such as EHRs, device APIs, and insurance platforms.

## Tactical Design Topics

- [Aggregates](aggregates/) - Consistency boundaries for sleep health domain objects, including SleepStudy, SleepSession, InterventionPlan, and CircadianProfile aggregates.
- [Entities](entities/) - Domain objects with unique identity: Patient, SleepEpisode, TherapyDevice, Clinician, and others that persist and evolve over time.
- [Value Objects](value-objects/) - Immutable domain concepts such as SleepStage, EpworthScore, SleepEfficiency, ChronotypeClassification, and AHI severity levels.
- [Domain Events](domain-events/) - Significant occurrences including SleepStudyCompleted, SleepStageScored, QualityMetricCalculated, InterventionStarted, and ReportGenerated.
- [Repositories](repositories/) - Persistence abstractions for aggregate roots, providing retrieval by patient, date range, study identifier, and clinical criteria.
- [Domain Services](domain-services/) - Stateless operations spanning aggregates, such as cross-night trend analysis, intervention efficacy computation, and composite score calculation.

## Bounded Context Topics

- [Sleep Data Collection Context](sleep-data-collection-context/) - Ingestion and management of polysomnography channels, actigraphy streams, wearable sensor data, and patient sleep diary entries.
- [Sleep Stage Analysis Context](sleep-stage-analysis-context/) - Epoch-by-epoch sleep staging following AASM rules, sleep architecture computation, arousal detection, and hypnogram generation.
- [Sleep Quality Assessment Context](sleep-quality-assessment-context/) - Calculation of sleep efficiency, latency, WASO, total sleep time, PSQI global scores, and Epworth Sleepiness Scale results.
- [Circadian Rhythm Context](circadian-rhythm-context/) - Chronotype assessment, light exposure tracking, dim light melatonin onset estimation, and shift work schedule modeling.
- [Intervention Tracking Context](intervention-tracking-context/) - Management of CBT-I protocols, sleep hygiene programs, medication schedules, CPAP device therapy, and adherence monitoring.
- [Clinical Integration Context](clinical-integration-context/) - EHR connectivity, FHIR R4 resource mapping, sleep study report generation, and clinical referral workflows.

## Cross-Cutting Topics

- [Event-Driven Architecture](event-driven-architecture/) - Asynchronous messaging patterns for communication between sleep health bounded contexts.
- [Integration Patterns](integration-patterns/) - Shared kernel, published language, open host service, and customer-supplier patterns applied to sleep health systems.
- [Sleep Health Metrics Standards](sleep-health-metrics-standards/) - AASM scoring manual, ICSD-3 diagnostic taxonomy, FHIR R4 observation profiles, and other clinical standards.
- [Regulatory Compliance](regulatory-compliance/) - HIPAA privacy and security rules, FDA medical device regulations, GDPR health data protections, and clinical governance requirements.
