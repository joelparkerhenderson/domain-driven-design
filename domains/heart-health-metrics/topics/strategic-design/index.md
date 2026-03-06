# Strategic Design

Strategic design in the heart health metrics domain addresses how to decompose the complex landscape of cardiac health monitoring into manageable, well-bounded subsystems. It provides the high-level architecture that governs how heart rate tracking, blood pressure monitoring, ECG analysis, risk scoring, and clinical integration coexist as independently evolvable modules.

## Overview

Strategic design, as defined by Eric Evans, focuses on the large-scale structure of a software system. In the heart health metrics domain, strategic design determines how cardiac data flows from sensors and devices through analysis pipelines to clinician dashboards and electronic health records. It ensures that each part of the system uses consistent terminology, maintains clear boundaries, and communicates through well-defined interfaces.

The heart health metrics domain is inherently complex because it spans consumer wearable technology, clinical-grade medical devices, validated risk scoring algorithms, real-time monitoring infrastructure, and healthcare interoperability standards. Strategic design provides the framework to manage this complexity without creating a monolithic, tangled system.

## Key Concepts

**Domain Decomposition.** The heart health metrics system is decomposed into six bounded contexts: Data Collection, Cardiac Analysis, Risk Assessment, Monitoring and Alerts, Reporting and Visualization, and Clinical Integration. Each context owns its models and enforces its own invariants.

**Ubiquitous Language Alignment.** Cardiologists, biomedical engineers, nurses, and software developers must share a common vocabulary. Terms like "arrhythmia," "QRS complex," "heart rate variability," and "Framingham Risk Score" carry precise clinical meanings that the software model must faithfully represent.

**Subdomain Classification.** The cardiac analysis and risk assessment subdomains are core subdomains because they embody the differentiating clinical logic. Monitoring and reporting are supporting subdomains. Data collection infrastructure and clinical integration protocols are generic subdomains that leverage established standards.

**Context Mapping.** Relationships between bounded contexts are documented in a context map that specifies upstream/downstream dependencies, shared kernels, and anti-corruption layers. For example, the Cardiac Analysis Context consumes normalized data from the Data Collection Context through a published language.

## Domain Examples

A hospital deploying a cardiac monitoring platform uses strategic design to separate the concerns of device management (Data Collection) from the clinical algorithms that detect atrial fibrillation (Cardiac Analysis). When a new wearable vendor is onboarded, only the Data Collection Context changes, while analysis algorithms remain untouched.

A telehealth company offering remote blood pressure monitoring uses strategic design to keep its risk scoring models (Risk Assessment) independent from its patient-facing dashboards (Reporting and Visualization). Clinicians can update risk thresholds without modifying the reporting infrastructure.

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/index.md) - Defines the six logical boundaries within the heart health metrics domain.
- [Context Map](../context-map/index.md) - Documents relationships and data flows between bounded contexts.
- [Subdomains](../subdomains/index.md) - Classifies cardiac health areas by strategic importance.
- [Ubiquitous Language](../ubiquitous-language/index.md) - Establishes the shared vocabulary for cardiac health monitoring.
- [Anti-Corruption Layer](../anti-corruption-layer/index.md) - Protects domain models from external system concepts.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapters 14-16 on strategic design.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 2 on domains, subdomains, and bounded contexts.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly, 2021. Part II on strategic design decisions.
- Yancy, C.W. et al. "2013 ACC/AHA Guideline on the Assessment of Cardiovascular Risk." *Circulation*, 2014.
- Topol, Eric. *Deep Medicine: How Artificial Intelligence Can Make Healthcare Human Again*. Basic Books, 2019.
