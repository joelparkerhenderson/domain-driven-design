# Cardiology Domain - CLAUDE.md

## Overview

This domain applies Domain-Driven Design (DDD) to cardiology systems, encompassing clinical assessment, diagnostic imaging, interventional procedures, electrophysiology, heart failure management, and cardiac rehabilitation.

## Bounded Contexts

1. Clinical Assessment Context - history/physical, risk stratification, cardiac biomarkers, stress testing
2. Diagnostic Imaging Context - echocardiography, cardiac CT, cardiac MRI, nuclear cardiology
3. Interventional Procedures Context - cardiac catheterization, PCI/stenting, TAVR, structural heart
4. Electrophysiology Context - arrhythmia management, ablation, pacemaker/ICD implantation, EP studies
5. Heart Failure Management Context - HFrEF/HFpEF classification, GDMT, mechanical circulatory support
6. Cardiac Rehabilitation Context - exercise programs, risk factor modification, lifestyle counseling

## Domain Principles

- Model using shared ubiquitous language between cardiologists, cardiac nurses, cardiac technologists, and system developers.
- Group the system into bounded contexts reflecting clinical subspecialties.
- Business logic remains pure and independent of infrastructure (database, UI, web, mobile).
- Follow ACC/AHA guideline-directed terminology and clinical pathways.
- Maintain HIPAA compliance and patient safety as cross-cutting concerns.

## Key Topics

- strategic-design
- bounded-contexts
- context-map
- ubiquitous-language
- subdomains
- anti-corruption-layer
- aggregates
- entities
- value-objects
- domain-events
- repositories
- domain-services
- clinical-assessment-context
- diagnostic-imaging-context
- interventional-procedures-context
- electrophysiology-context
- heart-failure-management-context
- cardiac-rehabilitation-context
- event-driven-architecture
- integration-patterns
- cardiology-standards
- regulatory-compliance

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- ACC/AHA Clinical Practice Guidelines (various years).
