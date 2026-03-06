# Strategic Design in the Psychiatry Domain

## Overview

Strategic design in Domain-Driven Design addresses the high-level architectural decisions that decompose a large, complex system into manageable bounded contexts. In the psychiatry domain, strategic design determines how clinical workflows, organizational boundaries, and regulatory requirements map to software architecture. The goal is to create a system where each bounded context has a clear responsibility, a consistent internal model, and well-defined integration points with other contexts.

## Decomposition Rationale

Psychiatry systems involve multiple clinical disciplines, regulatory frameworks, and care delivery models. A monolithic model attempting to represent all psychiatric concepts uniformly would quickly become inconsistent and unmanageable. Strategic design addresses this by identifying natural boundaries where terminology, rules, and workflows diverge.

The psychiatry domain decomposes into six bounded contexts based on the following criteria:

- **Clinical workflow separation**: Diagnostic assessment, medication prescribing, psychotherapy delivery, crisis response, rehabilitation, and outcome measurement each follow distinct clinical workflows with different participants, timelines, and decision points.
- **Regulatory alignment**: Different regulatory requirements govern medication management (DEA scheduling, FDA labeling), involuntary commitment (state mental health codes), and data privacy (42 CFR Part 2 for substance use records).
- **Team composition**: Each context corresponds to different clinical roles. Psychiatrists lead diagnostic and medication contexts, psychologists and therapists lead the psychotherapy context, social workers lead rehabilitation, and quality officers lead outcomes measurement.
- **Temporal boundaries**: Diagnostic assessments occur episodically, medication management is ongoing, psychotherapy sessions are scheduled, crises are event-driven, rehabilitation is longitudinal, and outcomes are measured periodically.

## Context Map Relationships

Strategic design also defines how bounded contexts interact. In the psychiatry domain, the primary relationships are:

- **Diagnostic Assessment upstream to Medication Management**: Diagnoses drive prescribing decisions. The medication context consumes diagnostic information but does not modify it.
- **Diagnostic Assessment upstream to Psychotherapy**: Diagnoses inform therapy modality selection. The psychotherapy context uses diagnostic information to select treatment protocols.
- **Crisis Management as a cross-cutting concern**: Crisis events can originate from any context and require rapid coordination across diagnostic, medication, and psychotherapy contexts.
- **Outcomes Measurement downstream from all contexts**: Outcomes measurement aggregates data from all other contexts to produce clinical quality metrics.

## Subdomain Classification

Strategic design classifies subdomains by their strategic importance to the organization:

- **Core subdomains** (Diagnostic Assessment, Medication Management) represent the highest clinical complexity and competitive differentiation. These require deep domain expertise and custom modeling.
- **Supporting subdomains** (Psychotherapy, Crisis Management, Rehabilitation Services) enable core activities with specialized but more standardized workflows.
- **Generic subdomains** (Outcomes Measurement) apply general measurement and reporting patterns adaptable from other healthcare domains.

## Distillation

The core domain in psychiatry centers on the diagnostic-to-treatment pipeline: how a psychiatric evaluation produces a diagnosis, how that diagnosis informs medication selection and psychotherapy modality, and how treatment effectiveness is monitored. This pipeline is the most clinically complex and the most differentiated aspect of any psychiatry system. Strategic design ensures that this pipeline receives the deepest modeling investment while supporting and generic subdomains are modeled with appropriate simplicity.

## Key Decisions

- Each bounded context owns its data and exposes only published interfaces to other contexts.
- The ubiquitous language is defined per context, with explicit translation at context boundaries.
- Anti-corruption layers protect contexts from external system terminology (EHR systems, insurance billing, pharmacy systems).
- Event-driven communication is preferred for inter-context integration to maintain loose coupling.

## Alignment with Organizational Structure

Strategic design in psychiatry aligns bounded contexts with clinical department structures. This alignment follows Conway's Law: the system architecture mirrors the organizational communication patterns. Diagnostic assessment aligns with the evaluation clinic, medication management with the prescribing team, psychotherapy with the therapy department, crisis management with the emergency psychiatric service, rehabilitation with the community support team, and outcomes with the quality improvement office.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapters 14-15 on strategic design and distillation.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 2 on domains, subdomains, and bounded contexts.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapters 7-9 on strategic design patterns.
- American Psychiatric Association. *Practice Guidelines for the Treatment of Psychiatric Disorders*. APA Publishing, 2006.
