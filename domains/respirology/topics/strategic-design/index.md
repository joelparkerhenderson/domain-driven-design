# Strategic Design in the Respirology Domain

## Overview

Strategic design in Domain-Driven Design addresses how a large, complex domain is decomposed into manageable parts that can be developed, maintained, and evolved independently. In the respirology domain, strategic design determines how clinical assessment, diagnostics, airway management, chronic disease management, ventilatory support, and pulmonary rehabilitation are separated into bounded contexts with well-defined relationships.

The respirology domain is inherently complex. It spans acute care scenarios such as emergency intubation, chronic management programs for COPD and ILD, diagnostic workflows involving sophisticated equipment, and rehabilitative interventions that unfold over weeks or months. Strategic design ensures that each of these areas maintains its own coherent model without being corrupted by the concerns of other areas.

## Bounded Contexts as Clinical Boundaries

The six bounded contexts in the respirology domain reflect natural clinical and operational boundaries. A pulmonologist performing a clinical assessment operates with different concepts and terminology than a respiratory therapist managing a ventilator. Strategic design formalizes these differences rather than forcing a single unified model that would dilute the precision of each area.

Each bounded context owns its model. The Clinical Assessment Context defines what a "respiratory examination" means internally. The Ventilatory Support Context defines what a "ventilator configuration" means internally. These definitions may reference similar real-world phenomena but are modeled differently because they serve different purposes within their respective contexts.

## Subdomain Classification

Strategic design classifies parts of the domain by their business importance:

- **Core Subdomains** represent the areas where the respirology system provides its greatest competitive and clinical value. Clinical Assessment, Respiratory Diagnostics, and Chronic Disease Management are core because they encode specialized clinical decision-making logic that differentiates the system.

- **Supporting Subdomains** are necessary for the system to function but do not represent unique clinical reasoning. Airway Management and Ventilatory Support provide essential procedural capabilities but follow well-established protocols that are less subject to competitive differentiation.

- **Generic Subdomains** handle concerns common across healthcare systems: patient identity management, appointment scheduling, billing, and authentication. These are best served by external systems or standard solutions.

## Context Mapping

The context map documents how bounded contexts relate to one another. In the respirology domain, key relationships include:

- Clinical Assessment Context publishes assessment results that the Respiratory Diagnostics Context consumes to determine which tests to order (customer-supplier relationship).
- Respiratory Diagnostics Context publishes diagnostic results that the Chronic Disease Management Context uses to update care plans (published language via standardized result formats).
- Ventilatory Support Context and Airway Management Context share a kernel of airway-related concepts (shared kernel for device and procedure types).
- Pulmonary Rehabilitation Context consumes events from multiple upstream contexts to tailor rehabilitation programs.

## Distillation

Domain distillation in the respirology domain means identifying the essential clinical logic that must be custom-built versus the generic capabilities that can be acquired. The risk stratification algorithms in Clinical Assessment, the diagnostic interpretation rules in Respiratory Diagnostics, and the action plan logic in Chronic Disease Management represent the distilled core. These encode the clinical expertise of pulmonologists and respiratory therapists in a form that software can execute.

## Protecting Model Integrity

Anti-corruption layers protect each bounded context from external model contamination. When the respirology system integrates with hospital information systems, laboratory systems, or medical device APIs, translation layers ensure that external concepts are mapped into the internal ubiquitous language of each context. This prevents the respirology domain model from being distorted by the data structures and assumptions of external systems.

## Strategic Design Decisions

Key strategic decisions in the respirology domain include:

- Choosing event-driven communication between contexts rather than synchronous calls, which enables each context to evolve independently and supports eventual consistency appropriate for clinical workflows.
- Defining published languages for inter-context communication based on HL7 FHIR respiratory resources, ensuring standardized data exchange without tight coupling.
- Establishing conformist relationships with external systems where the respirology domain must accept the model of an upstream system (such as a hospital ADT system) without modification.

## Evolutionary Architecture

Strategic design supports the evolution of the respirology domain as clinical knowledge advances. When new respiratory conditions are identified, new diagnostic techniques emerge, or treatment guidelines are updated, the bounded context structure allows changes to be localized. A change in COPD classification criteria affects the Chronic Disease Management Context without requiring changes to the Ventilatory Support Context.

## References

- Evans, E. (2003). *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley. Chapters 14-16 on strategic design, distillation, and large-scale structure.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley. Chapter 2 on domains, subdomains, and bounded contexts.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media. Part II on strategic design patterns.
