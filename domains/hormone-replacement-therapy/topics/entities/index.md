# Entities in the Hormone Replacement Therapy Domain

Entities are domain objects defined by their unique identity rather than their attributes. An entity maintains continuity through its lifecycle even as its properties change. In the hormone replacement therapy (HRT) domain, entities represent the persistent, identifiable objects that anchor clinical workflows, treatment management, and patient care across all bounded contexts.

## Purpose

HRT systems track objects that evolve over time while retaining their fundamental identity. A treatment protocol changes as doses are titrated, but it remains the same protocol. An adverse event record is updated with new causality assessments and resolution data, but it is the same event. Entities model these concepts by associating a stable identity with mutable state, ensuring that the system can track changes, maintain audit trails, and reference specific objects across transactions.

## Clinical Assessment Context Entities

### ClinicalAssessment

The ClinicalAssessment entity represents a single comprehensive evaluation of a patient for HRT candidacy. It is identified by an AssessmentId. Its mutable state includes symptom scores (which may be recalculated as additional data arrives), candidacy status (which transitions through screening stages), and assessment notes. The entity's lifecycle begins with intake initiation and progresses through symptom evaluation, panel interpretation, contraindication screening, and candidacy determination.

### SymptomEvaluation

The SymptomEvaluation entity represents the administration and scoring of a validated symptom assessment instrument during a clinical assessment. It is identified by an EvaluationId. Its state includes the instrument type, individual item responses, and the calculated composite score. It exists within the ClinicalAssessment aggregate and cannot be referenced independently outside that aggregate.

### HormonePanelInterpretation

The HormonePanelInterpretation entity represents the clinical interpretation of a set of hormone laboratory results. Identified by an InterpretationId, it captures the clinician's findings, flagged abnormalities, and overall endocrine status assessment. It evolves as additional tests are added or existing results are re-evaluated with new clinical context.

## Hormone Protocol Context Entities

### TreatmentProtocol

The TreatmentProtocol entity is the central entity in the Protocol Context. Identified by a ProtocolId, it represents the complete prescribed HRT regimen for a patient. Its state includes protocol status (draft, active, suspended, completed, discontinued), version number, prescriber identity, and effective dates. The protocol evolves through dose titrations, formulation changes, and status transitions over the treatment duration.

### ProtocolComponent

The ProtocolComponent entity represents a single hormone within a treatment protocol. Identified by a ComponentId, it specifies the hormone type, formulation, dosage, delivery method, and administration schedule. A protocol may have multiple components for combination therapy. Each component can be independently adjusted during treatment optimization while maintaining its identity as the same component.

## Lab Monitoring Context Entities

### MonitoringPlan

The MonitoringPlan entity tracks the complete monitoring schedule for a patient on an active treatment protocol. Identified by a MonitoringPlanId, it maintains the list of scheduled tests, completed results, and current alert status. The plan evolves as tests are completed, new tests are scheduled, and monitoring frequency is adjusted based on clinical response.

### ScheduledTest

The ScheduledTest entity represents a specific laboratory test scheduled for a specific date. Identified by a ScheduledTestId, it tracks the test type, scheduled date, completion status, and any rescheduling history. Its lifecycle moves from scheduled through collected to resulted, with possible cancellation or rescheduling at any point.

## Side Effect Management Context Entities

### AdverseEventRecord

The AdverseEventRecord entity captures a reported adverse event associated with HRT. Identified by an EventId, it tracks event description, onset date, severity grade, suspected cause, and resolution status. The entity evolves as causality assessments are performed, mitigation actions are taken, and the event progresses toward resolution or becomes chronic.

### CausalityAssessment

The CausalityAssessment entity represents a formal evaluation of the relationship between an adverse event and hormone therapy. Identified by an AssessmentId, it captures the assessor, assessment methodology, and determination (definite, probable, possible, unlikely, unrelated). Multiple assessments may exist for a single adverse event as different evaluators contribute their analyses.

### MitigationAction

The MitigationAction entity records a specific intervention taken to address an adverse event. Identified by an ActionId, it specifies the action type (dose reduction, formulation change, supportive medication, treatment pause), implementation date, and outcome assessment. It tracks whether the mitigation was effective and whether further action is needed.

## Treatment Optimization Context Entities

### OptimizationRecommendation

The OptimizationRecommendation entity encapsulates a proposed modification to an active treatment protocol. Identified by a RecommendationId, it specifies the proposed changes, supporting evidence, clinical rationale, and recommendation status (proposed, accepted, rejected, implemented). The entity transitions through review stages as clinicians evaluate and act on the recommendation.

## Outcomes Tracking Context Entities

### TreatmentOutcome

The TreatmentOutcome entity represents the longitudinal outcome record for a patient's treatment episode. Identified by an OutcomeId, it accumulates measurement data points over time, tracking symptom resolution, quality of life changes, and health outcome indicators. It grows throughout the treatment lifecycle and may persist after treatment completion for long-term follow-up.

### OutcomeMeasurement

The OutcomeMeasurement entity captures a single measurement at a specific point in time. Identified by a MeasurementId, it records the measurement date, instrument used, raw scores, and calculated results. Each measurement is an immutable snapshot once recorded, but the entity maintains its identity for reference and audit purposes.

## Identity Management

Entity identities in the HRT domain are generated as universally unique identifiers (UUIDs) to support distributed systems and prevent identity collisions. Identities are assigned at creation and never change. Cross-context references use identity values rather than direct object references, maintaining bounded context independence.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 5 on entities.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 5 on entities and value objects.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 5 on implementing business logic.
