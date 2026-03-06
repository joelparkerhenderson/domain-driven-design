# Strategic Design - Asthma Domain

## Overview

Strategic design in Domain-Driven Design addresses how to decompose a large, complex system into manageable bounded contexts. In the asthma management domain, strategic design determines how clinical assessment, treatment planning, trigger monitoring, action planning, medication management, and outcomes tracking are organized into coherent, independently evolvable subsystems.

The asthma domain is particularly well-suited to strategic design because asthma management inherently involves multiple specialized disciplines -- pulmonology, allergy/immunology, pharmacology, environmental health, and patient education -- each with distinct models, vocabularies, and workflows. Strategic design ensures these disciplines are represented faithfully while enabling integration across their boundaries.

## Domain Decomposition

The asthma management domain is decomposed into six bounded contexts based on organizational boundaries, clinical workflows, and data ownership patterns:

1. **Clinical Assessment Context** owns the diagnostic and monitoring models. Clinicians performing spirometry, peak flow measurement, and FeNO testing operate within this context. The GINA severity classification system provides the authoritative model for categorizing asthma severity and control levels.

2. **Treatment Management Context** owns the therapeutic planning models. This context applies the GINA stepwise approach, managing treatment escalation and de-escalation decisions, biologic agent eligibility criteria, and immunotherapy protocols.

3. **Trigger Monitoring Context** owns environmental and exposure data models. This context tracks allergens, air quality, occupational exposures, and other factors that provoke asthma symptoms.

4. **Action Planning Context** owns patient self-management models. The zone system (green, yellow, red) and personalized action plans are the core concepts within this context.

5. **Medication Management Context** owns prescription and adherence models. Controller medication schedules, rescue inhaler usage, and adherence monitoring belong to this context.

6. **Outcomes Tracking Context** owns longitudinal outcome measurement models. ACT scores, exacerbation records, lung function trends, and quality of life assessments reside here.

## Strategic Design Principles

**Alignment with Clinical Boundaries.** Each bounded context maps to a recognizable clinical function. Respiratory technicians performing spirometry operate in the Clinical Assessment Context, while pharmacists managing prescriptions operate in the Medication Management Context. This alignment ensures domain experts can validate the model within their area of expertise.

**Ubiquitous Language Per Context.** While terms like "FEV1" appear across contexts, their meaning and usage may differ. In the Clinical Assessment Context, FEV1 is a measured value with testing metadata. In the Outcomes Tracking Context, FEV1 is a data point in a longitudinal trend. Strategic design acknowledges these differences rather than forcing a single universal model.

**Autonomous Evolution.** Each bounded context can evolve independently. When GINA updates its classification criteria, only the Clinical Assessment Context requires modification. When new biologic agents receive approval, only the Treatment Management Context needs updating.

**Context Relationships.** The context map defines how bounded contexts interact. The Clinical Assessment Context publishes assessment results that the Treatment Management Context consumes. The Medication Management Context publishes adherence data that the Outcomes Tracking Context aggregates. These relationships are explicitly modeled to prevent hidden coupling.

## Subdomain Classification

Strategic design classifies subdomains by business value:

- **Core Subdomains**: Clinical Assessment and Treatment Management represent the highest-value, most differentiating capabilities. These require deep domain expertise and custom modeling.
- **Supporting Subdomains**: Action Planning and Trigger Monitoring support core clinical functions with domain-specific logic that enables better patient outcomes.
- **Generic Subdomains**: Medication Management and portions of Outcomes Tracking follow well-established patterns that can leverage existing healthcare standards and frameworks.

## Distillation

The domain vision statement for the asthma management system is: "Enable evidence-based, personalized asthma care through structured clinical assessment, adaptive treatment management, proactive trigger identification, and measurable outcomes tracking."

This vision statement guides strategic decisions about which aspects of the domain deserve the most modeling investment. Clinical assessment and treatment management are the core domain -- they embody the specialized clinical knowledge that differentiates an asthma management system from a generic chronic disease platform.

## Strategic Design Decisions

**Separate Contexts for Assessment and Treatment.** Although clinicians often perform assessment and treatment adjustments in the same visit, these are modeled as separate bounded contexts because they serve different purposes, evolve at different rates, and are governed by different clinical guidelines. Assessment follows diagnostic protocols; treatment follows therapeutic guidelines.

**Trigger Monitoring as a Distinct Context.** Environmental monitoring could be embedded within Clinical Assessment, but it is separated because it involves different data sources (air quality APIs, allergen databases), different update frequencies (real-time vs. visit-based), and different stakeholders (environmental health specialists vs. pulmonologists).

**Action Planning Bridges Clinical and Patient Domains.** The Action Planning Context translates clinical assessment results and treatment plans into patient-facing self-management instructions. It serves as a critical boundary between professional clinical models and patient-understandable models.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapters 14-15 on distillation and strategic design.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 2 on domains, subdomains, and bounded contexts.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapters 7-9 on strategic design patterns.
- Global Initiative for Asthma (GINA). Global Strategy for Asthma Management and Prevention, 2023.
