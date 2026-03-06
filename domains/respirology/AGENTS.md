# Respirology Domain - DDD Project Instructions

## Overview

This domain applies Domain-Driven Design (DDD) to respirology systems, encompassing clinical assessment, respiratory diagnostics, airway management, chronic disease management, ventilatory support, and pulmonary rehabilitation.

## Bounded Contexts

1. Clinical Assessment Context - respiratory examination, history taking, symptom scoring, risk stratification
2. Respiratory Diagnostics Context - pulmonary function tests, imaging, bronchoscopy, sputum analysis
3. Airway Management Context - intubation, tracheostomy, airway clearance, bronchodilator therapy
4. Chronic Disease Management Context - COPD action plans, ILD monitoring, bronchiectasis, sarcoidosis
5. Ventilatory Support Context - NIV, mechanical ventilation, high-flow therapy, home ventilation
6. Pulmonary Rehabilitation Context - exercise programs, breathing retraining, patient education, discharge planning

## Domain Principles

- Model respiratory care using ubiquitous language shared among pulmonologists, respiratory therapists, nurses, and system developers.
- Separate clinical reasoning from infrastructure concerns such as databases, user interfaces, and device integrations.
- Use CQRS to separate read-only queries (e.g., viewing spirometry trends) from write commands (e.g., recording a new ventilator setting).
- Use domain events for communication between bounded contexts (e.g., a diagnostic result triggering a care plan update).

## File Structure

- CLAUDE.md: project instructions (this file)
- plan.md: project plan
- tasks.md: task checklist
- index.md: domain overview
- ubiquitous-language.md: shared terminology
- topics/index.md: topic listing
- topics/{topic}/index.md: individual topic documentation

## Standards and Compliance

- ATS/ERS guidelines for pulmonary function testing
- GOLD framework for COPD classification
- HIPAA and regional health data privacy regulations
- HL7 FHIR for clinical data interoperability

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software.
- Vernon, V. (2013). Implementing Domain-Driven Design.
- Khononov, V. (2021). Learning Domain-Driven Design.
