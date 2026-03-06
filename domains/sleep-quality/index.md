# Sleep Quality Domain

## Overview

The Sleep Quality domain applies Domain-Driven Design (DDD) to sleep quality management systems. This domain encompasses sleep assessment, sleep disorder management, sleep environment optimization, behavioral interventions, device integration, and outcomes tracking. The domain model serves clinicians, researchers, device manufacturers, and individuals seeking to understand and improve their sleep.

## Bounded Contexts

### Sleep Assessment Context

Handles the evaluation of sleep quality through standardized instruments and clinical measurements. This context manages sleep diaries, validated questionnaires such as the Pittsburgh Sleep Quality Index (PSQI) and the Epworth Sleepiness Scale (ESS), actigraphy data collection, and polysomnography study coordination. It produces structured assessment results that feed into diagnosis and treatment planning.

### Sleep Disorders Context

Manages the identification, classification, and clinical tracking of sleep disorders according to the International Classification of Sleep Disorders (ICSD-3). This context covers insomnia, obstructive sleep apnea, restless legs syndrome, narcolepsy, and parasomnias. It maintains disorder-specific clinical models and tracks diagnostic criteria fulfillment.

### Sleep Environment Context

Models the environmental factors that influence sleep quality and provides optimization guidance. This context addresses bedroom conditions including light exposure, ambient noise levels, temperature regulation, air quality, and bedding selection. It captures environmental assessments and generates evidence-based recommendations.

### Behavioral Interventions Context

Captures evidence-based behavioral treatments for sleep disorders. This context primarily models Cognitive Behavioral Therapy for Insomnia (CBT-I), sleep restriction therapy, stimulus control instructions, sleep hygiene education, and relaxation techniques. It tracks intervention protocols, session progression, and adherence.

### Device Integration Context

Handles data ingestion and normalization from sleep-related devices. This context manages consumer wearables, smart mattresses and pillows, CPAP/BiPAP machines, and ambient environment sensors. It provides anti-corruption layers to translate device-specific data models into domain-standard concepts.

### Outcomes Tracking Context

Tracks longitudinal sleep quality outcomes and supports clinical decision-making. This context manages sleep efficiency trends, composite quality scores, daytime functioning assessments, and patient satisfaction measures. It provides reporting and analytics capabilities for individual and population-level insights.

## Domain Files

- [CLAUDE.md](CLAUDE.md) - Domain guidelines and structure.
- [plan.md](plan.md) - Strategic plan for domain modeling.
- [tasks.md](tasks.md) - Task tracking for domain development.
- [ubiquitous-language.md](ubiquitous-language.md) - Shared vocabulary across all bounded contexts.

## Topics

### Strategic Design

- [Strategic Design](topics/strategic-design/index.md)
- [Bounded Contexts](topics/bounded-contexts/index.md)
- [Context Map](topics/context-map/index.md)
- [Ubiquitous Language](topics/ubiquitous-language/index.md)
- [Subdomains](topics/subdomains/index.md)
- [Anti-Corruption Layer](topics/anti-corruption-layer/index.md)

### Tactical Design

- [Aggregates](topics/aggregates/index.md)
- [Entities](topics/entities/index.md)
- [Value Objects](topics/value-objects/index.md)
- [Domain Events](topics/domain-events/index.md)
- [Repositories](topics/repositories/index.md)
- [Domain Services](topics/domain-services/index.md)

### Bounded Context Details

- [Sleep Assessment Context](topics/sleep-assessment-context/index.md)
- [Sleep Disorders Context](topics/sleep-disorders-context/index.md)
- [Sleep Environment Context](topics/sleep-environment-context/index.md)
- [Behavioral Interventions Context](topics/behavioral-interventions-context/index.md)
- [Device Integration Context](topics/device-integration-context/index.md)
- [Outcomes Tracking Context](topics/outcomes-tracking-context/index.md)

### Cross-Cutting Concerns

- [Event-Driven Architecture](topics/event-driven-architecture/index.md)
- [Integration Patterns](topics/integration-patterns/index.md)
- [Sleep Quality Standards](topics/sleep-quality-standards/index.md)
- [Regulatory Compliance](topics/regulatory-compliance/index.md)

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media.
- Buysse, D.J. et al. (1989). The Pittsburgh Sleep Quality Index: A New Instrument for Psychiatric Practice and Research. Psychiatry Research, 28(2), 193-213.
- American Academy of Sleep Medicine. (2014). International Classification of Sleep Disorders, 3rd Edition (ICSD-3).
- Johns, M.W. (1991). A New Method for Measuring Daytime Sleepiness: The Epworth Sleepiness Scale. Sleep, 14(6), 540-545.
