# Critical Care Context

## Overview

The Critical Care Context governs the management of acute respiratory failure within the pulmonolgy domain. It encompasses mechanical ventilation, ARDS management, ICU respiratory protocols, and ventilator weaning strategies. This context operates under extreme time constraints where clinical decisions are measured in minutes and hours, distinguishing it fundamentally from the chronic care tempo of other pulmonolgy contexts.

As a supporting subdomain, the Critical Care Context follows well-established evidence-based protocols (ARDSNet, lung-protective ventilation guidelines) but still requires domain modeling to capture the complexity of real-time ventilator management, multi-parameter physiological response tracking, and systematic weaning strategies.

## Clinical Scope

The Critical Care Context covers the following clinical activities:

- **Mechanical Ventilation Management**: Selection and optimization of ventilation modes (volume control, pressure control, pressure support, SIMV), tidal volume targeting, PEEP titration, FiO2 management, and inspiratory time optimization. Includes both invasive ventilation via endotracheal tube or tracheostomy and non-invasive ventilation.

- **ARDS Management**: Severity classification using the Berlin definition, lung-protective ventilation with low tidal volume (6 mL/kg predicted body weight), PEEP tables for oxygenation optimization, prone positioning protocols, neuromuscular blockade consideration, and fluid management strategies.

- **ICU Respiratory Protocols**: Standardized protocols for ventilator bundle compliance, daily spontaneous awakening and breathing trials, head-of-bed elevation, deep vein thrombosis prophylaxis coordination, and stress ulcer prophylaxis as they relate to respiratory care.

- **Ventilator Weaning**: Systematic assessment of weaning readiness, spontaneous breathing trial (SBT) protocols, pressure support reduction strategies, tracheostomy decision-making for prolonged ventilation, and post-extubation monitoring.

## Aggregates

**Ventilation Session Aggregate**: The primary aggregate for managing a continuous ventilation episode. Root entity is VentilationSession, containing VentilatorSetting value objects, ArterialBloodGasResult value objects, and WeaningAttempt entities. Enforces the invariant that ventilator setting changes are paired with physiological response documentation.

**ARDS Management Aggregate**: Manages acute respiratory distress syndrome episodes. Root entity is ARDSManagement, containing SeverityClassification value objects, LungProtectiveVentilationParameters, and PronePosSession entities. Enforces the invariant that ARDS severity is classified using Berlin definition criteria.

## Key Entities

- **Ventilation Episode**: Tracks the full duration of mechanical ventilation from intubation through extubation or tracheostomy.
- **Weaning Trial**: Represents individual attempts to reduce or discontinue ventilator support, with type, duration, and outcome.
- **ICU Respiratory Therapist**: Tracks the respiratory therapist managing ventilator care with shift assignments and assessments.

## Key Value Objects

- **VentilatorSettings**: Complete ventilator parameter set at a point in time, immutable once recorded.
- **ArterialBloodGasResult**: ABG values with FiO2 context for interpretation.
- **WeaningReadinessScore**: Standardized assessment using RSBI, MIP, and clinical stability criteria.
- **BerlinCriteria**: ARDS severity classification with timing, PaO2/FiO2 ratio, and bilateral opacity documentation.

## Domain Events Published

- **MechanicalVentilationInitiated**: Signals the start of mechanical ventilation with initial settings and indication.
- **WeaningProtocolInitiated**: Signals that a patient meets criteria to begin weaning.
- **WeaningTrialCompleted**: Signals the outcome of a spontaneous breathing trial or weaning attempt.
- **ExtubationCompleted**: Signals successful extubation with post-extubation status.

## Domain Services

- **VentilatorManagementService**: Evaluates settings against clinical targets and lung-protective guidelines.
- **WeaningReadinessAssessmentService**: Calculates weaning readiness using standardized criteria and physiological parameters.
- **SedationVentilationCoordinationService**: Coordinates sedation management with ventilation strategy and daily awakening trials.

## Context Relationships

The Critical Care Context is downstream of the Chronic Disease Management Context via a conformist relationship. When a chronic disease patient experiences acute respiratory failure, the critical care team receives and conforms to the chronic disease management model for historical data, adapting it to the real-time clinical model without negotiating upstream changes.

The context has an anti-corruption layer relationship with the Pulmonary Rehabilitation Context. Post-ICU patients transitioning to rehabilitation have their ICU data translated through the ACL, converting critical care concepts (ventilator modes, sedation scores) into rehabilitation-relevant concepts (functional status, exercise tolerance estimates).

The context may receive ProcedureComplicationOccurred events from the Respiratory Diagnostics Context when severe procedural complications require ICU-level care.

## Business Rules

- Lung-protective ventilation targets tidal volume of 6 mL/kg predicted body weight with plateau pressure not exceeding 30 cmH2O.
- ARDS severity is reassessed every 24 hours using the Berlin definition PaO2/FiO2 ratio criteria.
- Spontaneous breathing trials are attempted daily for patients meeting standard weaning readiness criteria unless clinically contraindicated.
- Ventilator setting changes must be documented with the clinical rationale and followed by an arterial blood gas within a defined time window.
- Prone positioning sessions for ARDS patients must be at least 12-16 hours in duration per protocol guidelines.
- Post-extubation monitoring requires documented respiratory assessment at defined intervals for the first 48 hours.
- Reintubation within 48 hours of extubation triggers review of the weaning assessment process for quality improvement.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- ARDS Definition Task Force. Acute Respiratory Distress Syndrome: The Berlin Definition. *JAMA*, 2012.
- Acute Respiratory Distress Syndrome Network. Ventilation with Lower Tidal Volumes as Compared with Traditional Tidal Volumes for Acute Lung Injury and ARDS. *NEJM*, 2000.
