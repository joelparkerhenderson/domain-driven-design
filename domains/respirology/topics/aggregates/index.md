# Aggregates in the Respirology Domain

## Overview

An aggregate is a cluster of related domain objects treated as a single unit for data consistency. Each aggregate has a root entity (the aggregate root) through which all external access is mediated. Aggregates define consistency boundaries: invariants within an aggregate are enforced immediately, while consistency across aggregates is maintained through eventual consistency using domain events.

In the respirology domain, aggregates model the core clinical concepts that must maintain internal consistency during operations such as recording an assessment, ordering a diagnostic test, configuring a ventilator, or updating a care plan.

## Aggregate Design Principles

### Consistency Boundaries

Each aggregate defines the boundary within which business rules must be satisfied in a single transaction. In the respirology domain, a Respiratory Assessment aggregate ensures that all symptom scores, examination findings, and risk stratification results are consistent when the assessment is completed. An external reference to a diagnostic result does not need to be transactionally consistent with the assessment -- eventual consistency through events is sufficient.

### Small Aggregates

Aggregates should be kept small to reduce contention and improve performance. Rather than creating a large Patient aggregate that encompasses all respiratory data, the respirology domain distributes patient-related data across context-specific aggregates: Respiratory Assessment, Diagnostic Order, Care Plan, Ventilator Configuration, and Rehabilitation Program. Each aggregate holds only the data necessary to enforce its own invariants.

### Reference by Identity

Aggregates reference other aggregates by identity (ID) rather than by direct object reference. A Diagnostic Order aggregate references the patient by PatientId and the requesting assessment by AssessmentId, not by holding direct references to Patient or Assessment objects. This decoupling allows aggregates to be persisted, loaded, and scaled independently.

## Aggregates by Bounded Context

### Clinical Assessment Context

**Respiratory Assessment** (Aggregate Root): Encompasses the examination findings, symptom scores, vital signs, auscultation results, and risk stratification output for a single patient encounter. Invariants ensure that a completed assessment has all required scores and that risk stratification is consistent with the recorded findings.

### Respiratory Diagnostics Context

**Diagnostic Order** (Aggregate Root): Encompasses the ordered tests, clinical indications, patient preparation requirements, and order status for a diagnostic workup. Invariants ensure that ordered tests are clinically indicated and that required preparation steps are documented before the order is activated.

**Diagnostic Report** (Aggregate Root): Encompasses the results, interpretations, and quality indicators for a completed set of diagnostic tests. Invariants ensure that all results are validated and that the interpretation is consistent with the measured values.

### Airway Management Context

**Airway Procedure** (Aggregate Root): Encompasses the procedure type, technique details, device specifications, outcome, and complications for an airway management intervention. Invariants ensure that required pre-procedure checks are documented and that outcome recording follows protocol.

### Chronic Disease Management Context

**Care Plan** (Aggregate Root): Encompasses the disease-specific management plan, action zones, medication regimen, monitoring schedule, and review dates for a patient's chronic respiratory condition. Invariants ensure that action plan zones are complete, that escalation criteria are defined, and that the plan reflects the current disease classification.

### Ventilatory Support Context

**Ventilator Configuration** (Aggregate Root): Encompasses the ventilation mode, parameter settings, alarm thresholds, and adjustment history for a patient on ventilatory support. Invariants ensure that parameter combinations are clinically valid (e.g., tidal volume within safe range for the patient's ideal body weight) and that alarm thresholds are set.

### Pulmonary Rehabilitation Context

**Rehabilitation Program** (Aggregate Root): Encompasses the exercise prescription, breathing retraining plan, education modules, session schedule, and outcome measurements for a patient enrolled in pulmonary rehabilitation. Invariants ensure that exercise intensity is appropriate for the patient's functional capacity and that mandatory education modules are included.

## Aggregate Invariants in Respirology

Aggregate invariants encode clinical safety rules:

- A Ventilator Configuration must not allow tidal volume to exceed safe limits based on predicted body weight.
- A Care Plan for COPD must include all three action plan zones (green, yellow, red) with corresponding instructions.
- A Diagnostic Order for spirometry must include documentation of bronchodilator withholding if a reversibility test is ordered.
- A Rehabilitation Program must not prescribe exercise intensity exceeding the patient's assessed functional capacity.
- An Airway Procedure for intubation must record confirmation of tube placement.

## Aggregate Lifecycle

Aggregates in the respirology domain follow predictable lifecycles:

1. **Creation**: An aggregate is created when a clinical action initiates it (e.g., a new assessment is started, a diagnostic order is placed).
2. **Modification**: The aggregate is updated as clinical data is collected and decisions are made.
3. **Completion**: The aggregate reaches a terminal state (e.g., assessment completed, diagnostic report finalized, care plan archived).
4. **Event Publication**: Upon significant state changes, the aggregate publishes domain events for consumption by other contexts.

## References

- Evans, E. (2003). *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley. Chapter 6 on aggregates.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley. Chapter 10 on aggregate design rules.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media. Chapter 10 on aggregate design heuristics.
