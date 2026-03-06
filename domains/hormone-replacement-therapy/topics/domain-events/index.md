# Domain Events in the Hormone Replacement Therapy Domain

Domain events represent significant occurrences within the domain that other parts of the system need to know about. They are immutable records of something that happened at a specific point in time. In the hormone replacement therapy (HRT) domain, domain events drive communication between bounded contexts and trigger clinical workflows across the treatment lifecycle.

## Purpose

HRT treatment involves a chain of clinical activities where the completion of one activity triggers subsequent actions in other parts of the system. When a clinical assessment determines a patient is a candidate for HRT, the protocol context needs to begin protocol selection. When a lab result exceeds a therapeutic threshold, both side effect management and treatment optimization need to respond. Domain events model these triggers as first-class domain concepts, decoupling the producing context from consuming contexts while preserving the clinical workflow logic.

## Clinical Assessment Context Events

### AssessmentCompleted

Raised when a clinical assessment reaches its final determination. Payload includes the AssessmentId, PatientId, CandidacyStatus, RiskFactorProfile, symptom score summaries, and hormone panel interpretation highlights. Consumed by the Hormone Protocol Context to initiate protocol selection and by the Side Effect Management Context to establish baseline risk profiles.

### CandidacyStatusChanged

Raised when a patient's candidacy status changes due to new clinical information or reassessment. Payload includes the AssessmentId, PatientId, previous status, new status, and the reason for change. Consumed by the Hormone Protocol Context to suspend or modify active protocols when candidacy is revoked.

### RiskFactorIdentified

Raised when a new risk factor is identified during assessment or follow-up evaluation. Payload includes the PatientId, risk factor type, severity, and clinical details. Consumed by the Side Effect Management Context to update risk stratification models.

## Hormone Protocol Context Events

### ProtocolPrescribed

Raised when a new treatment protocol is finalized and approved for initiation. Payload includes the ProtocolId, PatientId, protocol components with hormone types, formulations, dosages, delivery methods, and the required monitoring schedule. Consumed by the Lab Monitoring Context to generate a monitoring plan and by the Side Effect Management Context to establish treatment-specific risk monitoring.

### ProtocolModified

Raised when an active protocol is changed through dose titration, formulation adjustment, or component addition or removal. Payload includes the ProtocolId, modification type, previous values, new values, and the clinical rationale. Consumed by the Lab Monitoring Context to adjust monitoring schedules and by the Outcomes Tracking Context to record the modification time point.

### ProtocolDiscontinued

Raised when a treatment protocol is permanently stopped. Payload includes the ProtocolId, PatientId, discontinuation reason, and effective date. Consumed by the Lab Monitoring Context to transition the monitoring plan, by the Side Effect Management Context to close active monitoring, and by the Outcomes Tracking Context to record the treatment endpoint.

## Lab Monitoring Context Events

### LabResultRecorded

Raised when a laboratory result is captured and validated. Payload includes the MonitoringPlanId, test type, result value, unit, reference range, therapeutic target range, and whether the result falls within the therapeutic target. Consumed by the Treatment Optimization Context for trend analysis and by the Outcomes Tracking Context for longitudinal tracking.

### ThresholdExceeded

Raised when a laboratory result falls outside the therapeutic target range. Payload includes the MonitoringPlanId, test type, result value, threshold exceeded (upper or lower), degree of exceedance, and clinical urgency classification. Consumed by the Side Effect Management Context for potential adverse event investigation and by the Treatment Optimization Context for dose adjustment consideration.

### MonitoringScheduleAdjusted

Raised when the monitoring frequency or test panel is changed. Payload includes the MonitoringPlanId, previous schedule, new schedule, and the reason for adjustment. Consumed by the Outcomes Tracking Context to record monitoring intensity changes.

## Side Effect Management Context Events

### SideEffectReported

Raised when a new adverse event is documented. Payload includes the EventId, PatientId, ProtocolId, event description, onset date, severity grade, and suspected causality. Consumed by the Treatment Optimization Context to incorporate side effect data into optimization calculations and by the Outcomes Tracking Context to record the adverse event in the treatment timeline.

### RiskLevelChanged

Raised when a patient's risk stratification changes. Payload includes the PatientId, previous risk level, new risk level, contributing factors, and effective date. Consumed by the Treatment Optimization Context to adjust recommendation thresholds and by the Hormone Protocol Context to review protocol appropriateness.

### ContraindicationDetected

Raised when a new contraindication is identified for a patient during active treatment. Payload includes the PatientId, contraindication type, severity (absolute or relative), and affected hormone types. Consumed by the Hormone Protocol Context to evaluate whether the current protocol must be modified or discontinued.

## Treatment Optimization Context Events

### OptimizationRecommended

Raised when the optimization analysis produces a protocol modification recommendation. Payload includes the RecommendationId, ProtocolId, proposed changes, supporting evidence summary, and confidence level. Consumed by the Hormone Protocol Context for prescriber review and action.

### TitrationCompleted

Raised when a dose titration sequence reaches its target or is concluded. Payload includes the ProtocolId, hormone type, initial dose, final dose, number of titration steps, and outcome assessment. Consumed by the Outcomes Tracking Context to record titration results.

## Outcomes Tracking Context Events

### OutcomeAssessed

Raised when a periodic outcome assessment is completed. Payload includes the OutcomeId, PatientId, assessment date, symptom resolution rates, quality of life scores, and efficacy rating. Consumed by the Treatment Optimization Context to inform evidence-based protocol adjustments.

### TreatmentMilestoneReached

Raised when a patient reaches a significant treatment milestone such as 3-month review, 6-month review, or annual assessment. Payload includes the PatientId, milestone type, summary metrics, and recommendation for continued treatment. Consumed by the Clinical Assessment Context to trigger reassessment if needed.

## Event Design Principles

Domain events in the HRT domain are immutable once created. They carry sufficient data for consumers to process without querying back to the producing context. They include correlation identifiers that link related events across contexts. They are timestamped with both the time the domain event occurred and the time it was published. They use a published language schema that is versioned to support consumer evolution.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 8 on domain events.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 8 on domain events.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 10 on domain events.
- Brandolini, Alberto. "Introducing EventStorming." Leanpub, 2021.
