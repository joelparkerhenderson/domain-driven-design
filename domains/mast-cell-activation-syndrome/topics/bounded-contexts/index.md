# Bounded Contexts for Mast Cell Activation Syndrome

## Overview

Bounded contexts define logical boundaries where specific models and terminologies remain consistent. In the MCAS domain, six bounded contexts partition the management system into areas that reflect distinct clinical responsibilities, each maintaining its own domain model, invariants, and ubiquitous language.

Each bounded context encapsulates a coherent set of business rules and concepts. Terms that appear in multiple contexts may carry different meanings depending on the context in which they are used. For example, "severity" in the Symptom Tracking Context refers to an individual symptom intensity rating, while in the Outcomes Measurement Context it refers to an aggregate disease burden classification.

## Diagnostic Assessment Context

This context manages the diagnostic journey from initial clinical suspicion through confirmed MCAS diagnosis. It owns laboratory test ordering, specimen tracking, result interpretation, and consensus criteria evaluation.

Key concepts within this context include tryptase level measurement, histamine metabolite quantification, prostaglandin D2 assessment, bone marrow biopsy evaluation, and the application of consensus diagnostic criteria. The context maintains its own model of what constitutes a valid diagnostic workup and how results are interpreted against established thresholds.

The Diagnostic Assessment Context publishes diagnostic conclusions as domain events. Other contexts consume these events but cannot modify diagnostic data. This context has upstream authority over the patient's diagnostic status.

## Trigger Management Context

This context handles the identification, categorization, and mitigation of factors that provoke mast cell degranulation. It manages trigger profiles, avoidance strategies, environmental control plans, and dietary management protocols.

Triggers are classified by type: environmental (fragrances, chemicals, temperature), dietary (high-histamine foods, additives), pharmacological (NSAIDs, opioids, contrast dyes), physical (exercise, vibration, pressure), and emotional (stress, anxiety). Each trigger type has its own assessment and management model.

The Trigger Management Context consumes symptom events from the Symptom Tracking Context to correlate flares with potential triggers. It also consumes diagnostic events to understand the patient's specific mast cell activation patterns.

## Medication Protocol Context

This context governs the pharmacological management of MCAS, including medication selection, dosing, titration, and compounding. It manages complex multi-drug regimens that typically include H1 antihistamines, H2 antihistamines, mast cell stabilizers, and leukotriene inhibitors.

A distinguishing feature of this context is the management of compounded medications. Many MCAS patients react to inactive ingredients in commercial formulations, requiring custom compounding without problematic dyes, fillers, or excipients. The context tracks formulation details, compounding pharmacy relationships, and patient-specific tolerability information.

The Medication Protocol Context publishes medication change events that the Outcomes Measurement Context consumes to assess treatment effectiveness.

## Symptom Tracking Context

This context captures the patient's symptom experience across multiple organ systems. MCAS manifests with symptoms spanning dermatological, gastrointestinal, cardiovascular, neurological, respiratory, and musculoskeletal systems.

Key responsibilities include symptom logging with timestamps and severity scores, flare event documentation with duration and trigger associations, baseline symptom level tracking, and pattern analysis over time. The context supports both patient-reported and clinician-documented symptom data.

The Symptom Tracking Context is a prolific publisher of domain events. Symptom data flows to Trigger Management for correlation analysis, to Medication Protocol for treatment response assessment, and to Outcomes Measurement for burden scoring.

## Specialist Coordination Context

This context manages the multi-provider nature of MCAS care. Patients typically see allergists, immunologists, gastroenterologists, dermatologists, cardiologists, and primary care providers. Coordinating care across these specialists requires shared care plans, referral management, and communication tracking.

The context maintains a care team model that tracks each provider's role, specialty, and involvement level. Referrals are managed as first-class domain concepts with their own lifecycle from request through completion. Shared care plans aggregate treatment goals and interventions from multiple providers into a coherent whole.

## Outcomes Measurement Context

This context assesses treatment effectiveness and patient well-being over time. It consumes data from other contexts to compute symptom burden scores, quality of life metrics, medication effectiveness ratings, and functional status assessments.

The Outcomes Measurement Context does not own source data but rather aggregates and analyzes data published by other contexts. It maintains its own models for scoring algorithms, assessment instruments, and longitudinal trend analysis.

## Context Boundaries

Each bounded context maintains strict boundaries. Data exchange between contexts occurs through domain events and published interfaces, never through shared databases or direct model access. This ensures that each context can evolve its internal model independently without breaking other contexts.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 14 on maintaining model integrity.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 2 on bounded contexts.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 7 on bounded context boundaries.
