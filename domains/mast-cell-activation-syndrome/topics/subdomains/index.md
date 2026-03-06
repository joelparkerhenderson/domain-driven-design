# Subdomains for Mast Cell Activation Syndrome

## Overview

Subdomains classify areas of a domain by their strategic importance to the business. Domain-Driven Design distinguishes three types of subdomains: core, supporting, and generic. For the MCAS domain, this classification determines where to invest the most design effort and where to leverage existing solutions.

The subdomain classification directly influences architectural decisions, team allocation, and the level of model sophistication applied to each area. Core subdomains receive the most attention because they represent the competitive or clinical differentiators of the system.

## Core Subdomain: Diagnostic Assessment

The Diagnostic Assessment Context is the core subdomain of the MCAS domain. Accurate diagnosis is the foundation upon which all subsequent management depends. Without a reliable diagnostic model, trigger management, medication protocols, and outcomes measurement lose their clinical validity.

The core subdomain warrants the most sophisticated domain modeling. The diagnostic process for MCAS is complex, involving multiple laboratory tests, evolving consensus criteria, and the need to differentiate MCAS from related conditions such as systemic mastocytosis, hereditary alpha-tryptasemia, and idiopathic anaphylaxis.

The Diagnostic Assessment model must capture nuances such as the requirement for acute mediator level elevations above patient-specific baselines, the timing constraints on specimen collection during flare events, and the integration of multiple diagnostic criteria frameworks. This complexity justifies the investment in a rich domain model.

## Supporting Subdomains

### Trigger Management

Trigger Management is a supporting subdomain because effective trigger identification and avoidance directly enables better patient outcomes, but it depends on the diagnostic foundation. The trigger model requires moderate sophistication to handle the diversity of trigger types, the personalized nature of trigger profiles, and the temporal relationships between exposures and reactions.

### Medication Protocol

Medication Protocol is a supporting subdomain with significant complexity due to the multi-drug nature of MCAS treatment, the need for compounded medications, and the careful titration requirements. While pharmacological management is critical to patient care, the medication concepts themselves are not unique to MCAS. What is unique is the specific combination of medications, the emphasis on excipient avoidance, and the titration sensitivity.

### Symptom Tracking

Symptom Tracking is a supporting subdomain that captures the patient experience across multiple organ systems. The model requires sufficient sophistication to handle multi-system symptom logging, severity scoring, and temporal pattern recognition. Symptom data is essential for informing decisions in other contexts.

### Specialist Coordination

Specialist Coordination is a supporting subdomain focused on the logistics of multi-provider care. While care coordination is important, the coordination patterns themselves are not unique to MCAS. The subdomain requires a model that handles referrals, shared care plans, and provider communication without excessive complexity.

### Outcomes Measurement

Outcomes Measurement is a supporting subdomain that aggregates data from other contexts to assess treatment effectiveness. Its model must support validated assessment instruments, longitudinal trend analysis, and population-level comparisons. The analytical models may be sophisticated, but they depend on data produced by other subdomains.

## Generic Subdomains

Several aspects of the MCAS management system are generic subdomains that do not contain MCAS-specific business logic. These include user authentication and authorization, notification and messaging services, audit logging and compliance tracking, document management and storage, and scheduling and calendar management.

Generic subdomains should be implemented using existing solutions rather than custom-built. They are necessary for system operation but do not contribute to the clinical value proposition of the MCAS domain.

## Strategic Implications

The subdomain classification guides several strategic decisions. The core subdomain (Diagnostic Assessment) should receive the most experienced team members and the most rigorous modeling effort. Supporting subdomains should be modeled with sufficient depth to support their clinical functions without over-engineering. Generic subdomains should use off-the-shelf solutions or well-established patterns.

Investment in the core subdomain yields the highest return because diagnostic accuracy directly determines the effectiveness of all downstream management activities. A misdiagnosis or missed diagnosis has cascading negative effects throughout the entire system.

## Evolution

Subdomain classifications may shift as the MCAS field evolves. If new treatment modalities emerge that require novel medication management approaches, the Medication Protocol subdomain could potentially be reclassified as core. Similarly, advances in biomarker discovery could expand the scope and strategic importance of the Diagnostic Assessment subdomain.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 15 on distillation and core domain.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 2 on subdomains.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 1 on core, supporting, and generic subdomains.
