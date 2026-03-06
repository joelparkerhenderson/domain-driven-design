# Domain Events in the Respirology Domain

## Overview

A domain event is a record of something significant that happened within the domain. Domain events capture the fact that a state change has occurred, carrying the relevant data about what changed and when. In Domain-Driven Design, domain events are the primary mechanism for communication between bounded contexts, enabling loose coupling and eventual consistency.

In the respirology domain, domain events signal clinically meaningful occurrences: an assessment has been completed, a diagnostic result is available, an exacerbation has been detected, a ventilator alarm has been triggered, or a rehabilitation milestone has been achieved. These events drive workflows across the system without requiring direct coupling between contexts.

## Domain Event Characteristics

### Past Tense Naming

Domain events are named in the past tense because they represent facts that have already occurred. AssessmentCompleted, DiagnosticResultAvailable, ExacerbationDetected, and VentilatorAlarmTriggered are statements of fact, not requests or commands.

### Immutability

Once published, a domain event cannot be changed. It represents a historical fact. If a correction is needed, a new compensating event is published rather than modifying the original event.

### Self-Contained

Each domain event carries sufficient data for consumers to act on it without needing to query the publishing context. An AssessmentCompleted event includes the assessment summary, patient identifier, clinician identifier, and timestamp -- enough for downstream contexts to process it.

## Key Domain Events by Bounded Context

### Clinical Assessment Context

**AssessmentCompleted**: Published when a respiratory assessment is finalized. Carries the patient ID, encounter ID, symptom scores (mMRC, CAT), key examination findings, vital signs summary, risk stratification result, and the assessing clinician ID. Consumed by the Respiratory Diagnostics Context (to inform test ordering), the Chronic Disease Management Context (to update care plans), and the Pulmonary Rehabilitation Context (to adjust program parameters).

**RiskStratificationUpdated**: Published when a patient's respiratory risk level changes. Carries the patient ID, previous risk level, new risk level, and the factors driving the change. Consumed by the Chronic Disease Management Context to trigger care plan reviews.

**SymptomDeteriorationIdentified**: Published when assessment scoring reveals a significant worsening of symptoms compared to baseline. Carries the patient ID, affected scores, magnitude of change, and clinical urgency. Consumed by the Chronic Disease Management Context to evaluate exacerbation criteria.

### Respiratory Diagnostics Context

**DiagnosticOrderCreated**: Published when a new set of diagnostic tests is ordered. Carries the order ID, patient ID, ordered tests, clinical indications, and ordering clinician. Consumed internally and by scheduling systems.

**DiagnosticResultAvailable**: Published when diagnostic test results are finalized and validated. Carries the report ID, patient ID, test types, key results (e.g., FEV1 value, FEV1/FVC ratio), interpretation, and quality indicators. Consumed by the Chronic Disease Management Context and Clinical Assessment Context.

**AbnormalResultFlagged**: Published when a diagnostic result falls outside expected parameters or indicates a clinically significant finding. Carries the report ID, patient ID, flagged parameters, severity, and recommended follow-up. Consumed by the Clinical Assessment Context for urgent review.

### Airway Management Context

**AirwayEstablished**: Published when an airway is successfully secured through intubation or tracheostomy. Carries the procedure ID, patient ID, procedure type, device specifications, and confirmation details. Consumed by the Ventilatory Support Context to initiate ventilator configuration.

**AirwayClearanceCompleted**: Published when an airway clearance session is completed. Carries the session ID, patient ID, technique used, duration, sputum characteristics, and patient tolerance. Consumed by the Chronic Disease Management Context for tracking.

**BronchodilatorAdministered**: Published when a bronchodilator treatment is delivered. Carries the administration ID, patient ID, medication details, pre- and post-treatment peak flow measurements, and patient response. Consumed by the Chronic Disease Management Context.

### Chronic Disease Management Context

**ExacerbationDetected**: Published when clinical data indicates that a patient is experiencing an exacerbation. Carries the exacerbation ID, patient ID, severity classification, triggering criteria, and recommended actions. Consumed by the Clinical Assessment Context (to request urgent assessment) and the Ventilatory Support Context (to prepare for potential ventilatory needs).

**CarePlanUpdated**: Published when a care plan is revised based on new clinical information. Carries the care plan ID, patient ID, change summary, new GOLD classification if applicable, and effective date. Consumed by the Pulmonary Rehabilitation Context.

**ActionPlanRevised**: Published when a patient's self-management action plan is modified. Carries the action plan ID, patient ID, revised zone criteria, and updated medication instructions.

### Ventilatory Support Context

**VentilationStarted**: Published when ventilatory support is initiated for a patient. Carries the configuration ID, patient ID, ventilation mode, initial parameters, and start timestamp.

**VentilatorAlarmTriggered**: Published when a ventilator alarm condition is detected. Carries the alarm ID, patient ID, alarm type, parameter values at the time of alarm, severity, and timestamp. Consumed by clinical alerting systems.

**WeaningTrialCompleted**: Published when a spontaneous breathing trial or weaning step is completed. Carries the trial ID, patient ID, trial parameters, duration, outcome (success/failure), and vital signs during the trial. Consumed by the Pulmonary Rehabilitation Context when weaning succeeds.

**WeaningCompleted**: Published when a patient is successfully weaned from ventilatory support. Carries the patient ID, weaning duration, final trial details, and post-extubation status. Consumed by the Pulmonary Rehabilitation Context to initiate rehabilitation referral.

### Pulmonary Rehabilitation Context

**RehabilitationProgramStarted**: Published when a patient begins a rehabilitation program. Carries the program ID, patient ID, program type, start date, and initial functional assessment results.

**RehabilitationMilestoneAchieved**: Published when a patient reaches a defined milestone such as a target 6MWT distance or completion of a module. Carries the milestone type, patient ID, and achievement details.

**PatientDischarged**: Published when a patient completes or exits the rehabilitation program. Carries the program ID, patient ID, discharge reason, final outcomes, and follow-up plan.

## Event Handling Patterns

Domain events in the respirology domain follow publish-subscribe patterns. Publishing contexts do not know which contexts consume their events. This decoupling allows new consumers to be added without modifying the publisher. Event handlers in consuming contexts translate incoming events into commands or queries within their own bounded context.

## References

- Evans, E. (2003). *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley. Chapter 8 on domain events.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley. Chapter 8 on domain events and event-driven architecture.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media. Chapter 11 on domain events and integration.
