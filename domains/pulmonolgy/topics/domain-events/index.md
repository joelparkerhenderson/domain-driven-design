# Domain Events in the Pulmonolgy Domain

## Overview

Domain events represent significant occurrences within the domain that domain experts care about. They capture something that happened in the past tense and can trigger reactions in other parts of the system. In the pulmonolgy domain, domain events signal clinical milestones, state transitions, and cross-context notifications that drive respiratory care workflows.

Domain events are fundamental to decoupling bounded contexts in the pulmonolgy domain. When a pulmonary assessment is completed, the Pulmonary Assessment Context does not directly call into the Respiratory Diagnostics Context. Instead, it publishes an AssessmentCompleted event that interested contexts can subscribe to and react to independently.

## Domain Events by Bounded Context

### Pulmonary Assessment Context Events

**AssessmentCompleted**: Published when a patient assessment encounter is finalized with all findings documented. Carries the assessment identifier, patient identifier, primary findings summary, and recommended next steps. Consumed by the Respiratory Diagnostics Context to initiate diagnostic workup and by the Chronic Disease Management Context to update baseline data.

**PFTSessionCompleted**: Published when a pulmonary function testing session meets quality criteria and results are finalized. Carries the session identifier, patient identifier, spirometry results, lung volume results, and DLCO results. Consumed by downstream contexts that need pulmonary function data.

**AbnormalFindingDetected**: Published when a clinically significant abnormality is identified during assessment, such as a new pulmonary nodule on imaging or severely reduced FEV1. Carries the finding type, severity, and recommended urgency of follow-up.

### Respiratory Diagnostics Context Events

**DiagnosticProcedureCompleted**: Published when a bronchoscopy, thoracentesis, or lung biopsy is completed with all specimens collected. Carries the procedure identifier, procedure type, complications if any, and specimen identifiers.

**DiagnosticResultAvailable**: Published when final diagnostic results, including pathology, are available for clinical review. Carries the result identifier, diagnostic classification, and clinical significance. Consumed by the Chronic Disease Management Context for disease classification and treatment planning.

**ProcedureComplicationOccurred**: Published when a complication occurs during or after a diagnostic procedure. Carries the complication type, severity, and management actions taken. May trigger Critical Care Context involvement for severe complications.

### Chronic Disease Management Context Events

**ExacerbationDetected**: Published when an exacerbation of a chronic respiratory condition is identified. Carries the patient identifier, disease type, exacerbation severity, and triggering factors. May trigger Critical Care Context involvement for severe exacerbations and Pulmonary Rehabilitation Context program adjustments.

**TreatmentPlanRevised**: Published when a treatment plan is modified due to clinical indication. Carries the plan identifier, revision type (medication change, monitoring adjustment, action plan update), and clinical justification. Consumed by the Pulmonary Rehabilitation Context to adjust exercise prescriptions if needed.

**DiseaseProgressionDetected**: Published when longitudinal trend analysis indicates disease progression. Carries the disease type, progression metric (declining FEV1, worsening diffusion capacity), and rate of decline.

**SeverityReclassified**: Published when a patient's disease severity classification changes, such as moving from GOLD stage II to GOLD stage III. Carries the old classification, new classification, and basis for reclassification.

### Critical Care Context Events

**MechanicalVentilationInitiated**: Published when a patient is placed on mechanical ventilation. Carries the patient identifier, intubation details, initial ventilator settings, and clinical indication. Consumed by the Chronic Disease Management Context to update disease records.

**WeaningProtocolInitiated**: Published when a patient meets criteria to begin ventilator weaning. Carries the weaning readiness assessment results and planned weaning approach.

**WeaningTrialCompleted**: Published when a spontaneous breathing trial or other weaning attempt concludes. Carries the trial type, duration, outcome (success or failure), and physiological data during the trial.

**ExtubationCompleted**: Published when a patient is successfully extubated. Carries the ventilation duration, weaning trajectory, and post-extubation status. Consumed by the Pulmonary Rehabilitation Context to initiate post-ICU rehabilitation planning.

### Sleep Medicine Context Events

**SleepStudyScored**: Published when a polysomnography or home sleep test is fully scored. Carries the study identifier, AHI result, sleep architecture summary, and diagnostic classification.

**OSADiagnosisConfirmed**: Published when obstructive sleep apnea is diagnosed based on sleep study results. Carries the severity classification and recommended therapy type.

**TherapyComplianceReported**: Published periodically when CPAP/BiPAP compliance data is collected. Carries the compliance period, usage statistics, and compliance status relative to clinical and insurance thresholds.

**TherapySettingsOptimized**: Published when positive airway pressure settings are adjusted based on titration or clinical response. Carries the old settings, new settings, and optimization rationale.

### Pulmonary Rehabilitation Context Events

**RehabilitationEnrollmentCompleted**: Published when a patient completes enrollment and baseline assessment for a rehabilitation program. Carries the enrollment identifier, baseline functional capacity measures, and program plan.

**FunctionalCapacityAssessed**: Published when a progress assessment (such as a six-minute walk test) is completed. Carries the assessment results and comparison to baseline.

**RehabilitationProgramCompleted**: Published when a patient completes a rehabilitation program. Carries the program outcomes, pre- and post-program comparisons, and maintenance plan recommendations.

## Event Design Principles

Domain events in the pulmonolgy domain follow these principles:

- **Past Tense Naming**: Events describe something that has already happened. "AssessmentCompleted" rather than "CompleteAssessment."

- **Immutability**: Published events are immutable records of what occurred. They are never modified after publication.

- **Self-Contained**: Events carry sufficient data for consumers to react without needing to query the publishing context. This reduces coupling between contexts.

- **Temporal Ordering**: Events include timestamps that enable consumers to process them in the correct clinical sequence.

- **Idempotent Handling**: Event consumers are designed to handle duplicate event delivery gracefully, which is essential in distributed healthcare systems.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 8 on domain events and their role in modeling significant occurrences.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 8 on domain event design, publication, and subscription patterns.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 6 on domain events and event-driven communication between bounded contexts.
