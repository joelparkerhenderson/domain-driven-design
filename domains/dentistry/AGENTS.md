# Dentistry Domain - AGENTS.md

## Overview

This domain applies Domain-Driven Design (DDD) to dentistry and oral healthcare systems, encompassing clinical examination and diagnosis, treatment planning and procedures, preventive care and oral hygiene, dental imaging and radiology, periodontal assessment and management, and prosthodontic and restorative services.

## Bounded Contexts

1. Clinical Examination - Manages the patient intake, oral examination, dental charting, periodontal probing, and diagnostic assessment workflows that form the basis of treatment planning.
2. Treatment Planning - Handles the creation, sequencing, costing, and authorization of multi-stage treatment plans, including priority classification and patient consent for proposed interventions.
3. Dental Procedures - Governs the execution and documentation of clinical procedures across specialties, including restorative, endodontic, prosthodontic, orthodontic, oral surgical, and periodontal treatments.
4. Preventive Care - Tracks preventive interventions such as prophylaxis, fluoride application, sealants, oral hygiene instruction, and risk-based recall scheduling.

## Domain Principles

- Model using shared ubiquitous language between dentists, dental hygienists, dental nurses, orthodontists, oral surgeons, and system developers.
- Group the system into bounded contexts reflecting clinical workflows from examination through treatment and ongoing preventive care.
- Business logic remains pure and independent of infrastructure (database, UI, web, mobile).
- Follow the ADA Current Dental Terminology (CDT), FDI World Dental Federation notation, and applicable national dental practice standards.

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
- clinical-examination
- treatment-planning
- dental-procedures
- preventive-care
- event-driven-architecture
- integration-patterns
- dentistry-standards
- regulatory-compliance

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- American Dental Association. CDT: Code on Dental Procedures and Nomenclature. ADA, published annually.
- FDI World Dental Federation. FDI Two-Digit Notation for Dental Charting. FDI, 1970.
- Summitt, James B., et al. Fundamentals of Operative Dentistry: A Contemporary Approach. 4th ed. Quintessence, 2013.
