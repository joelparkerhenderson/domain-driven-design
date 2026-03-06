# Strategic Design for Mast Cell Activation Syndrome

## Overview

Strategic design in Domain-Driven Design addresses how to decompose a large system into manageable bounded contexts. For the Mast Cell Activation Syndrome (MCAS) domain, strategic design determines how the complex landscape of diagnosis, treatment, tracking, and coordination is partitioned into distinct areas of responsibility, each with clear boundaries and well-defined relationships.

MCAS management inherently spans multiple clinical disciplines and operational concerns. Strategic design ensures that each area maintains internal consistency while enabling meaningful collaboration across boundaries. The goal is a system architecture that mirrors the natural structure of MCAS clinical practice.

## Decomposition Strategy

The MCAS domain decomposes into six bounded contexts based on clinical workflow boundaries. Each context corresponds to a distinct phase or aspect of MCAS management that operates under its own rules and terminology.

The Diagnostic Assessment Context handles the journey from clinical suspicion through confirmed diagnosis. This context owns laboratory test ordering, result interpretation, and criteria evaluation. It operates independently but publishes diagnostic outcomes that other contexts consume.

The Trigger Management Context focuses on the ongoing work of identifying, categorizing, and mitigating triggers. This context has its own lifecycle distinct from diagnosis, as trigger discovery continues throughout a patient's care journey.

The Medication Protocol Context manages the pharmacological dimension of treatment. MCAS medication management is unusually complex due to the need for compounded formulations, multi-drug regimens, and careful titration, justifying its separation as a distinct context.

The Symptom Tracking Context captures the patient experience across multiple organ systems. Symptom data flows into other contexts but is owned and validated within this boundary.

The Specialist Coordination Context manages the multi-provider nature of MCAS care. Referrals, shared care plans, and inter-provider communication have their own models and rules.

The Outcomes Measurement Context aggregates data from other contexts to assess treatment effectiveness and quality of life. It consumes events but does not modify data in other contexts.

## Context Relationships

The six contexts relate to one another through well-defined integration patterns. The Diagnostic Assessment Context serves as an upstream context, publishing diagnostic conclusions that downstream contexts consume. The Symptom Tracking Context acts as a shared data source, publishing symptom events that inform Trigger Management, Medication Protocol, and Outcomes Measurement.

The Specialist Coordination Context operates as a coordination layer, consuming information from multiple contexts to maintain shared care plans. The Outcomes Measurement Context is a downstream consumer that aggregates data without modifying source contexts.

## Subdomain Classification

Strategic design classifies the MCAS domain into core, supporting, and generic subdomains. The Diagnostic Assessment Context is the core subdomain because accurate diagnosis is the foundation of all MCAS management. Trigger Management, Medication Protocol, and Symptom Tracking are supporting subdomains that enable effective treatment. Specialist Coordination and Outcomes Measurement are additional supporting subdomains that enhance the overall system.

Generic subdomains such as user authentication, notification delivery, and audit logging are deliberately excluded from the MCAS domain model as they do not contain MCAS-specific business logic.

## Design Principles

Strategic design for MCAS follows several guiding principles. First, clinical workflow boundaries determine context boundaries, not technical or organizational constraints. Second, the ubiquitous language within each context reflects the terminology used by the clinical experts who operate in that area. Third, integration between contexts uses domain events to maintain loose coupling. Fourth, anti-corruption layers protect each context from external system concepts, particularly when integrating with electronic health records, laboratory information systems, and pharmacy systems.

## Alignment with Clinical Practice

The strategic design reflects how MCAS care is actually delivered. Patients typically move through a diagnostic phase, then enter ongoing management involving trigger identification, medication optimization, and symptom monitoring. Multiple specialists contribute to care, and outcomes are measured over time. The bounded context structure mirrors this clinical reality.

## Key Considerations

MCAS presents unique strategic design challenges. The condition is relatively newly recognized, so diagnostic criteria continue to evolve. The ubiquitous language must accommodate this evolution without destabilizing the domain model. Additionally, MCAS management is highly individualized, requiring the domain model to support patient-specific variations in triggers, medication tolerability, and symptom presentation.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapters 14-15 on strategic design and bounded contexts.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 2 on domains, subdomains, and bounded contexts.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapters 7-9 on strategic design patterns.
- Valent, P. et al. "Proposed Diagnostic Algorithm for Patients with Suspected Mast Cell Activation Syndrome." Journal of Allergy and Clinical Immunology: In Practice, 2019.
