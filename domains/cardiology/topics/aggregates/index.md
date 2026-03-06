# Aggregates in Cardiology

## Overview

Aggregates are clusters of related domain objects that are treated as a single unit for data consistency purposes. In the cardiology domain, aggregates enforce invariants that ensure clinical data integrity. Each aggregate has a root entity that controls access to all objects within the aggregate boundary. External objects may only hold references to the aggregate root, never to internal entities or value objects.

## Aggregate Design Principles for Cardiology

Cardiology aggregates must balance clinical completeness with consistency boundary sizing. A common mistake is creating aggregates that are too large -- for example, modeling an entire patient encounter as a single aggregate. Instead, aggregates should be sized to enforce specific clinical invariants while remaining small enough for efficient operations.

Key principles:

- **Clinical invariant enforcement**: Each aggregate protects rules that must always be true. For example, a stress test aggregate ensures that contraindication checks are completed before a test protocol is assigned.
- **Transactional consistency**: All changes within an aggregate are committed atomically. If a cardiac biomarker panel includes troponin, BNP, and CK-MB, all values are recorded together.
- **Eventual consistency between aggregates**: Cross-aggregate consistency is achieved through domain events. When a diagnostic imaging study is completed, it publishes an event that the clinical assessment aggregate eventually processes.

## Aggregates by Bounded Context

### Clinical Assessment Context

**PatientAssessment Aggregate**: Root entity is PatientAssessment, containing clinical findings (value objects), vital signs (value objects), and the assessment status. Invariant: an assessment cannot be finalized without at least one clinical finding recorded.

**RiskStratification Aggregate**: Root entity is RiskStratification, encapsulating the risk scoring model (Framingham, ASCVD, HEART), input parameters, and calculated scores. Invariant: a risk score cannot be calculated with missing required input parameters.

**StressTest Aggregate**: Root entity is StressTest, containing the test protocol, contraindication checklist, stage progression data, and test results. Invariant: a stress test cannot proceed if active contraindications are present.

### Diagnostic Imaging Context

**ImagingStudy Aggregate**: Root entity is ImagingStudy, containing study metadata, acquisition protocol, structured measurements, and the interpretation report. Invariant: a study cannot be finalized without all required measurements for the protocol completed.

**ImagingOrder Aggregate**: Root entity is ImagingOrder, containing clinical indication, priority, and scheduling requirements. Invariant: an order must have a valid clinical indication before submission.

### Interventional Procedures Context

**CatheterizationProcedure Aggregate**: Root entity is CatheterizationProcedure, containing vascular access details, hemodynamic measurements, angiographic findings, and interventions performed. Invariant: all interventions must be linked to documented angiographic findings.

**DeviceImplantation Aggregate**: Root entity is DeviceImplantation (for stents, valves, closure devices), containing device specifications, implantation site, deployment parameters, and immediate post-deployment assessment. Invariant: device serial number and lot must be recorded before procedure completion.

### Electrophysiology Context

**EPStudy Aggregate**: Root entity is EPStudy, containing intracardiac recordings, pacing maneuvers, inducibility results, and mapping data. Invariant: baseline recordings must be obtained before any pacing maneuvers.

**AblationProcedure Aggregate**: Root entity is AblationProcedure, containing ablation lesion locations, energy delivery parameters, and endpoint achievement. Invariant: ablation endpoint criteria must be defined before lesion delivery begins.

**CIEDImplantation Aggregate**: Root entity is CIEDImplantation, containing device model, lead placement data, sensing/pacing thresholds, and initial programming. Invariant: all implanted lead thresholds must be within acceptable ranges before procedure completion.

### Heart Failure Management Context

**HeartFailureProfile Aggregate**: Root entity is HeartFailureProfile, containing phenotype classification, NYHA class, ACC/AHA stage, ejection fraction history, and comorbidity list. Invariant: phenotype classification must be consistent with the most recent ejection fraction measurement.

**GDMTRegimen Aggregate**: Root entity is GDMTRegimen, containing current medications, target doses, titration history, and contraindications. Invariant: no medication may exceed its maximum recommended dose.

### Cardiac Rehabilitation Context

**RehabilitationProgram Aggregate**: Root entity is RehabilitationProgram, containing exercise prescription, risk factor goals, session schedule, and phase designation. Invariant: exercise intensity must not exceed the patient's cleared MET capacity.

**RehabilitationSession Aggregate**: Root entity is RehabilitationSession, containing exercise performance data, vital signs during exercise, symptoms reported, and session outcome. Invariant: a session must be terminated if vital signs exceed safety thresholds.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapters 5-6 on aggregates.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 10 on aggregate design.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 7 on aggregate patterns.
- ACC/AHA Clinical Data Standards for structured cardiology data collection.
