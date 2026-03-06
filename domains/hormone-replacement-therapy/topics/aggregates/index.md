# Aggregates in the Hormone Replacement Therapy Domain

Aggregates are clusters of related domain objects treated as a single unit for the purpose of data changes and consistency enforcement. Each aggregate has a root entity that serves as the sole entry point for external references, and the aggregate boundary defines the scope within which invariants must be satisfied after every transaction. In the hormone replacement therapy (HRT) domain, aggregates model the key transactional units across all six bounded contexts.

## Purpose

HRT systems involve complex clinical data that must maintain internal consistency. A treatment protocol must ensure that its hormone formulations, dosing schedules, and delivery methods are mutually compatible. A monitoring plan must keep its scheduled tests aligned with protocol requirements. Aggregates enforce these consistency rules by grouping related objects and processing changes through the aggregate root.

## Clinical Assessment Context Aggregates

### ClinicalAssessment Aggregate

The ClinicalAssessment aggregate is the root of the assessment workflow. It encapsulates the patient's symptom scores, hormone panel results, medical history summary, contraindication screen results, and the resulting candidacy determination. The aggregate root is the ClinicalAssessment entity, identified by an AssessmentId. Internal entities include SymptomEvaluation and HormonePanelInterpretation. Value objects include SymptomScore, CandidacyStatus, and RiskFactorProfile. The invariant is that candidacy status must be consistent with the contraindication screen and risk factor profile: a patient with absolute contraindications cannot have a candidacy status of "approved."

### PatientProfile Aggregate

The PatientProfile aggregate maintains the patient's demographic and clinical baseline information relevant to HRT assessment. It includes medical history entries, family history, and current medications. This aggregate is separate from ClinicalAssessment because patient profiles persist across multiple assessments and treatment episodes.

## Hormone Protocol Context Aggregates

### TreatmentProtocol Aggregate

The TreatmentProtocol aggregate defines the complete HRT regimen for a patient. The root entity is TreatmentProtocol, identified by a ProtocolId. It contains ProtocolComponent entities, each specifying a hormone, formulation, dose, delivery method, and schedule. Value objects include Dosage, DeliveryMethod, CyclingPattern, and ProtocolStatus. The primary invariant is that all protocol components must be pharmacologically compatible, meaning no contraindicated hormone combinations are permitted within a single protocol.

### FormulationCatalog Aggregate

The FormulationCatalog aggregate maintains the registry of approved hormone formulations available for protocol construction. It includes formulation specifications with active ingredients, concentrations, available delivery forms, and regulatory approval status. This aggregate ensures that protocols reference only currently approved formulations.

## Lab Monitoring Context Aggregates

### MonitoringPlan Aggregate

The MonitoringPlan aggregate tracks all scheduled and completed laboratory monitoring for a patient-protocol pair. The root entity is MonitoringPlan, identified by a MonitoringPlanId. It contains ScheduledTest entities and completed LabResult value objects. Value objects include TherapeuticTargetRange and MonitoringFrequency. The invariant is that every protocol-required test type must have a corresponding scheduled test entry, and no monitoring plan can exist without at least one scheduled test.

### LabResultSet Aggregate

The LabResultSet aggregate groups laboratory results from a single collection event. It ensures that results collected simultaneously are evaluated together for cross-test correlations, such as the relationship between estradiol and FSH levels that indicates treatment response.

## Side Effect Management Context Aggregates

### AdverseEventRecord Aggregate

The AdverseEventRecord aggregate captures a single adverse event occurrence. The root entity is AdverseEventRecord, identified by an EventId. It contains CausalityAssessment entities and MitigationAction entities. Value objects include SeverityGrade, EventClassification, and ResolutionStatus. The invariant is that a resolved event must have a resolution date and at least one documented outcome.

### ContraindicationRegistry Aggregate

The ContraindicationRegistry aggregate maintains the current set of absolute and relative contraindications for HRT. It ensures that contraindication rules are consistent, meaning that a condition cannot be simultaneously classified as both absolute and relative for the same hormone type.

## Treatment Optimization Context Aggregates

### OptimizationRecommendation Aggregate

The OptimizationRecommendation aggregate encapsulates a proposed protocol modification. The root entity is OptimizationRecommendation, identified by a RecommendationId. It contains supporting evidence references, proposed changes, and clinical rationale. Value objects include TitrationDirection, DoseAdjustment, and EvidenceSummary. The invariant is that every recommendation must include at least one supporting evidence reference and a clinical rationale.

## Outcomes Tracking Context Aggregates

### TreatmentOutcome Aggregate

The TreatmentOutcome aggregate tracks longitudinal treatment results for a patient. The root entity is TreatmentOutcome, identified by an OutcomeId. It contains OutcomeMeasurement entities recorded at different time points. Value objects include QualityOfLifeScore, SymptomResolutionRate, and EfficacyRating. The invariant is that outcome measurements must be in chronological order and each measurement must reference a valid assessment instrument.

## Design Principles

Aggregates in the HRT domain are designed to be as small as possible while still enforcing necessary invariants. Cross-aggregate consistency is achieved through eventual consistency via domain events rather than through enlarged aggregate boundaries. References between aggregates use identity references rather than direct object references, maintaining aggregate independence and supporting distributed deployment.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 6 on aggregates.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 10 on aggregates.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 5 on implementing business logic.
- Vernon, Vaughn. "Effective Aggregate Design." DDD Community Essay Series, 2011.
