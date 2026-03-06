# Bounded Contexts in the Pulmonolgy Domain

## Overview

Bounded contexts are the central pattern in Domain-Driven Design's strategic design. Each bounded context defines a logical boundary within which a particular domain model is consistent and well-defined. In the pulmonolgy domain, six bounded contexts represent the natural clinical and operational divisions of respiratory medicine.

A bounded context is not merely an organizational grouping. It is a linguistic boundary where every term, every concept, and every rule has a single, unambiguous meaning. The same word may appear in multiple contexts but carry different semantics. For example, "compliance" in the Sleep Medicine Context refers to a patient's adherence to CPAP therapy, while in the Regulatory Compliance cross-cutting concern it refers to adherence to legal and governance requirements.

## Pulmonary Assessment Context

The Pulmonary Assessment Context manages the initial clinical evaluation of patients presenting with respiratory symptoms. This context encompasses pulmonary function testing (PFTs), chest imaging interpretation, and structured symptom evaluation.

Key responsibilities include ordering and interpreting spirometry, lung volume measurements, and diffusion capacity tests. The context maintains its own model of patient presentation, symptom severity, and preliminary clinical impression. It produces assessment summaries that downstream contexts consume for diagnostic and therapeutic decision-making.

The Pulmonary Assessment Context serves as the primary upstream context in the domain, generating the initial clinical data that drives workflows in the Respiratory Diagnostics and Chronic Disease Management contexts.

## Respiratory Diagnostics Context

The Respiratory Diagnostics Context handles advanced diagnostic procedures including spirometry interpretation, DLCO testing, bronchoscopy, thoracentesis, and lung biopsy. This context encapsulates procedural workflows, specimen tracking, and diagnostic result interpretation.

This context maintains detailed models for procedure scheduling, informed consent, procedural documentation, specimen handling, and pathology result integration. It operates with specialized terminology around procedural techniques, complication management, and diagnostic classification systems.

The Respiratory Diagnostics Context has a partnership relationship with the Chronic Disease Management Context, where diagnostic results directly inform disease classification and treatment plan adjustments.

## Chronic Disease Management Context

The Chronic Disease Management Context supports longitudinal care for patients with chronic respiratory conditions: COPD, asthma, interstitial lung disease (ILD), pulmonary hypertension, and cystic fibrosis. It manages treatment plans, medication regimens, exacerbation tracking, and clinical outcome monitoring.

This context models disease-specific treatment protocols, severity classifications (such as GOLD staging for COPD or GINA step therapy for asthma), and care plan adherence. It maintains longitudinal patient histories with trend analysis for key clinical metrics.

As a core subdomain, the Chronic Disease Management Context receives the most modeling investment because it represents the highest ongoing clinical value in pulmonary care.

## Critical Care Context

The Critical Care Context governs acute respiratory failure management including mechanical ventilation, ARDS management, ICU respiratory protocols, and ventilator weaning strategies. This context operates under extreme time constraints where clinical decisions are measured in minutes and hours rather than days and weeks.

Key models include ventilation mode selection, ventilator parameter optimization, lung-protective ventilation protocols, sedation management, and systematic weaning readiness assessment. The context maintains real-time physiological data models and alarm threshold management.

The Critical Care Context has distinct characteristics that justify its separation: real-time decision-making tempo, multi-organ system interactions, and high-acuity resource utilization patterns.

## Sleep Medicine Context

The Sleep Medicine Context manages the diagnosis and treatment of sleep-related breathing disorders. It encompasses polysomnography (PSG), home sleep testing, obstructive sleep apnea (OSA) management, CPAP/BiPAP therapy optimization, and narcolepsy evaluation.

This context maintains models for sleep study ordering, scoring, and interpretation following AASM criteria. It tracks therapy compliance data, mask fitting records, pressure titration results, and long-term treatment outcomes.

The Sleep Medicine Context uses an anti-corruption layer when integrating with external sleep lab data systems, translating between vendor-specific data formats and the domain's internal models.

## Pulmonary Rehabilitation Context

The Pulmonary Rehabilitation Context coordinates multidisciplinary rehabilitation programs for patients with chronic lung disease. It manages exercise training prescriptions, breathing technique instruction, patient education programs, and self-management action plans.

Key models include exercise capacity assessment (six-minute walk test, cardiopulmonary exercise testing), individualized training programs, progress tracking, and program outcome measurement. The context also manages group education sessions and psychological support referrals.

As a generic subdomain, the Pulmonary Rehabilitation Context follows well-established, widely standardized protocols defined by ATS/ERS guidelines, making it suitable for less intensive modeling investment.

## Context Boundary Enforcement

Each bounded context enforces its boundaries through several mechanisms:

- **Separate Models**: Each context maintains its own domain model. A Patient entity in the Pulmonary Assessment Context contains different attributes than a Patient entity in the Critical Care Context.

- **Translation at Boundaries**: When data crosses context boundaries, it passes through translation layers that map between the source context's model and the target context's model.

- **Ownership**: Each context has clear ownership by a specific team or set of domain experts, ensuring that model changes are governed by those with the deepest knowledge.

- **Independent Evolution**: Contexts can evolve their internal models without forcing changes in other contexts, as long as published interfaces remain stable.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 14 on maintaining model integrity through bounded contexts.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 2 on bounded context definition and implementation strategies.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 7 on discovering and defining bounded context boundaries.
