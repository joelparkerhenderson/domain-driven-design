# Ubiquitous Language for Mast Cell Activation Syndrome

## Overview

Ubiquitous language is a shared vocabulary used consistently by domain experts, developers, and stakeholders within a bounded context. For the MCAS domain, the ubiquitous language bridges the gap between clinical immunology terminology and software development concepts, ensuring that all participants in the system understand one another without ambiguity.

The MCAS ubiquitous language is particularly important because the condition spans multiple medical specialties, each with its own professional vocabulary. A unified language prevents misunderstandings that could arise from the same term carrying different meanings across clinical disciplines.

## Principles

The ubiquitous language for the MCAS domain follows several guiding principles. Every term used in domain models, code, documentation, and conversations must have a single, well-defined meaning within its bounded context. When a term has different meanings in different contexts, this is explicitly acknowledged and documented.

Clinical terminology is preferred over software abstractions when naming domain concepts. A "Flare" is called a Flare, not an "IncidentEvent." A "Tryptase Level" is called a Tryptase Level, not a "BiomarkerReading." This ensures that clinicians can read and validate the domain model without translation.

The language evolves through collaboration between clinical experts and developers. As understanding deepens, terms may be refined, added, or deprecated. All changes are propagated consistently through models, code, and documentation.

## Diagnostic Assessment Terms

Within the Diagnostic Assessment Context, the language includes terms for laboratory measurements, diagnostic criteria, and clinical evaluation. A "Mediator Panel" refers to the complete set of laboratory tests ordered to assess mast cell activation. A "Baseline Level" is the mediator measurement obtained when the patient is not experiencing a flare. An "Acute Level" is the measurement obtained during or shortly after a flare episode.

"Consensus Criteria" refers to the specific diagnostic framework used to confirm MCAS. The term encompasses the three required elements: episodic multi-system symptoms consistent with mast cell mediator release, documented elevation of validated mast cell mediators above baseline, and response to medications targeting mast cell mediators.

## Trigger Management Terms

The Trigger Management Context uses terms that describe the classification and mitigation of triggers. A "Trigger Profile" is the comprehensive record of all identified triggers for an individual patient. An "Exposure Event" is a documented instance of contact with a known or suspected trigger. A "Reaction Window" is the time period between exposure and symptom onset.

"Environmental Controls" refers to modifications made to a patient's living or working environment to reduce trigger exposure. "Dietary Restrictions" describes the specific food avoidance protocols derived from trigger identification.

## Medication Protocol Terms

The Medication Protocol Context employs terms specific to MCAS pharmacotherapy. A "Medication Regimen" is the complete set of medications prescribed for MCAS management. A "Titration Schedule" defines the step-wise dosage adjustments planned for a medication. "Tolerability" describes whether a patient can take a medication without adverse reaction to its active or inactive ingredients.

"Compounding Specification" is the detailed formulation instructions for a custom-prepared medication, including the active ingredient, dosage form, and excluded excipients.

## Symptom Tracking Terms

The Symptom Tracking Context uses terms for multi-system symptom documentation. A "Symptom Entry" is a single recorded observation of a symptom at a point in time. A "Flare Record" documents an acute exacerbation including onset, duration, severity, suspected triggers, and interventions used. A "Symptom Pattern" is a recurring combination of symptoms identified through longitudinal analysis.

"Organ System" categorizes symptoms by the affected body system: dermatological, gastrointestinal, cardiovascular, neurological, respiratory, or musculoskeletal.

## Specialist Coordination Terms

The Specialist Coordination Context defines terms for multi-provider care. A "Care Team" is the set of providers actively involved in a patient's MCAS management. A "Care Plan" is the coordinated treatment strategy agreed upon by the care team. A "Consultation Request" is a formal referral from one provider to a specialist for evaluation.

## Outcomes Measurement Terms

The Outcomes Measurement Context uses terms for evaluating treatment success. "Symptom Burden Score" quantifies the cumulative impact of symptoms over a defined period. "Quality of Life Assessment" measures the patient's subjective well-being using validated instruments. "Treatment Response" classifies the patient's reaction to a medication change as improved, unchanged, or worsened.

## Language Governance

The ubiquitous language is maintained collaboratively. Proposed changes are reviewed by both clinical experts and developers. Deprecated terms are documented with migration guidance. Context-specific variations are explicitly noted to prevent confusion.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 2 on ubiquitous language.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 1 on the importance of shared language.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 2 on discovering domain knowledge.
