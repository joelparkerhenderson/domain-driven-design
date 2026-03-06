# Bounded Contexts in the Respirology Domain

## Overview

A bounded context is a logical boundary within which a particular domain model is defined and applicable. In the respirology domain, six bounded contexts represent distinct areas of clinical responsibility, each with its own ubiquitous language, aggregate structure, and business rules. These boundaries are drawn where the meaning of terms changes or where different teams (pulmonologists, respiratory therapists, rehabilitation specialists) maintain independent workflows.

## The Six Bounded Contexts

### 1. Clinical Assessment Context

This context manages the initial and ongoing respiratory assessment of patients. It encompasses respiratory examination workflows, structured history taking, symptom scoring using standardized instruments such as the mMRC dyspnea scale and CAT score, and risk stratification for treatment planning.

Key concepts within this context include the Respiratory Assessment aggregate, the Symptom Score value object, and the Patient History entity. The context is responsible for determining a patient's current respiratory status and communicating that status to downstream contexts through domain events.

### 2. Respiratory Diagnostics Context

This context governs the ordering, execution, and interpretation of respiratory diagnostic tests. It handles pulmonary function tests (spirometry, plethysmography, diffusion capacity), chest imaging interpretation, bronchoscopy procedures, and sputum analysis.

Key concepts include the Diagnostic Order aggregate, the Spirometry Result value object, and the Bronchoscopy Report entity. The context publishes diagnostic results as domain events that other contexts consume for clinical decision-making.

### 3. Airway Management Context

This context handles all procedures related to establishing, maintaining, and protecting the patient's airway. It covers intubation, tracheostomy, airway clearance techniques (chest physiotherapy, oscillatory devices), and bronchodilator therapy administration.

Key concepts include the Airway Procedure aggregate, the Intubation Record entity, and the Bronchodilator Dose value object. This context must coordinate closely with the Ventilatory Support Context when mechanical ventilation follows airway establishment.

### 4. Chronic Disease Management Context

This context manages long-term care plans for patients with chronic respiratory conditions. It handles COPD action plans with zone-based self-management instructions, interstitial lung disease monitoring protocols, bronchiectasis management including exacerbation tracking, and sarcoidosis surveillance.

Key concepts include the Care Plan aggregate, the Action Plan entity, the GOLD Classification value object, and the Exacerbation Record entity. This context consumes diagnostic results and clinical assessments to update and adapt care plans over time.

### 5. Ventilatory Support Context

This context manages the prescription, configuration, monitoring, and weaning of ventilatory support. It covers non-invasive ventilation (NIV) using BiPAP and CPAP, invasive mechanical ventilation, high-flow nasal therapy, and home ventilation programs.

Key concepts include the Ventilator Configuration aggregate, the Ventilation Mode value object, the Weaning Protocol entity, and the Home Ventilation Program entity. This context generates alarm events and weaning readiness assessments.

### 6. Pulmonary Rehabilitation Context

This context coordinates structured rehabilitation programs for patients with chronic respiratory disease. It encompasses exercise program design, breathing retraining techniques, patient education curricula, outcome measurement using the 6MWT and quality-of-life instruments, and discharge planning.

Key concepts include the Rehabilitation Program aggregate, the Exercise Prescription entity, the Functional Assessment value object, and the Discharge Plan entity. This context consumes data from multiple upstream contexts to tailor programs to individual patients.

## Boundary Characteristics

Each bounded context exhibits three defining characteristics:

**Linguistic Boundary**: Terms carry specific meanings within each context. "Assessment" in the Clinical Assessment Context means a structured respiratory examination, while in the Pulmonary Rehabilitation Context it means a functional capacity evaluation. These differences are intentional and reflect the distinct concerns of each area.

**Model Boundary**: Each context maintains its own domain model with its own aggregates, entities, and value objects. A Patient entity in the Clinical Assessment Context carries assessment history, while the same real-world patient appears in the Chronic Disease Management Context as a Care Plan holder with different attributes.

**Team Boundary**: Each context can be developed and maintained by a team with the appropriate clinical expertise. The Ventilatory Support Context benefits from collaboration with respiratory therapists and intensivists, while the Pulmonary Rehabilitation Context benefits from collaboration with rehabilitation specialists and exercise physiologists.

## Inter-Context Communication

Bounded contexts communicate through well-defined interfaces. Domain events are the primary mechanism: when a context completes a significant action, it publishes an event that other contexts may consume. Direct coupling between context internals is prohibited. Anti-corruption layers translate between the models of different contexts when integration is necessary.

## References

- Evans, E. (2003). *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley. Chapter 14 on maintaining model integrity.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley. Chapter 2 on bounded contexts.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media. Chapter 6 on bounded context boundaries.
