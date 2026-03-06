# Oncology Domain - Domain-Driven Design

## Overview

This domain applies Domain-Driven Design (DDD) to oncology systems, covering diagnosis and staging, treatment planning, chemotherapy management, radiation therapy, surgical oncology, and survivorship care.

## Bounded Contexts

1. Diagnosis & Staging Context - cancer screening, biopsy, pathology, TNM staging, molecular profiling
2. Treatment Planning Context - tumor board, multidisciplinary planning, clinical trials, NCCN guidelines
3. Chemotherapy Management Context - regimen selection, dose calculation, infusion scheduling, toxicity monitoring
4. Radiation Therapy Context - simulation, treatment planning, dose delivery, IGRT, brachytherapy
5. Surgical Oncology Context - resection planning, margin assessment, sentinel node biopsy, reconstruction
6. Survivorship Care Context - follow-up protocols, late effects monitoring, psychosocial support, care plans

## Domain Topics

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
- diagnosis-staging-context
- treatment-planning-context
- chemotherapy-management-context
- radiation-therapy-context
- surgical-oncology-context
- survivorship-care-context
- event-driven-architecture
- integration-patterns
- oncology-standards
- regulatory-compliance

## Conventions

- All documentation uses ubiquitous language shared among oncologists, nurses, pharmacists, radiation therapists, and software developers.
- Business logic is pure and independent of infrastructure concerns.
- Each bounded context maintains its own internal consistency and communicates through domain events.
- TNM staging, NCCN guidelines, and CTCAE grading are treated as published languages.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
