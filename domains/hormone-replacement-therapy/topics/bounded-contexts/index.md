# Bounded Contexts in the Hormone Replacement Therapy Domain

Bounded contexts are the central pattern in Domain-Driven Design for managing complexity in large systems. Each bounded context defines a linguistic and conceptual boundary within which a particular domain model is consistent and unambiguous. In the hormone replacement therapy (HRT) domain, six bounded contexts decompose the treatment lifecycle into cohesive units with well-defined interfaces.

## Purpose

HRT systems must model clinical assessment, pharmacological protocols, laboratory data, adverse events, treatment adjustment, and long-term outcomes. These concerns involve overlapping terminology with divergent semantics. For example, "hormone level" means a diagnostic indicator in the Clinical Assessment Context, a therapeutic target in the Hormone Protocol Context, and a data point for trend analysis in the Lab Monitoring Context. Bounded contexts ensure that each meaning is precise within its boundary.

## Clinical Assessment Context

This context owns the patient intake workflow, including symptom evaluation using validated instruments such as the Menopause Rating Scale, hormone panel interpretation with reference range comparison, medical history collection, and candidacy screening against clinical criteria. The primary aggregate is the ClinicalAssessment, which encapsulates symptom scores, panel results, risk factors, and the candidacy determination. This context produces AssessmentCompleted events consumed by downstream contexts.

Key concepts within this boundary include symptom score, hormone panel, candidacy status, risk factor profile, and assessment recommendation. The context does not manage treatment protocols or prescriptions; it only determines whether a patient is a candidate and characterizes their clinical profile.

## Hormone Protocol Context

This context manages the catalog of approved HRT regimens, delivery method specifications, dosing algorithms, and prescription generation. The primary aggregate is the TreatmentProtocol, which defines the specific hormone formulations, doses, schedules, and delivery methods for an individual patient. Supporting entities include FormulationCatalog and DeliveryMethodSpecification.

The context consumes assessment data from the Clinical Assessment Context through a customer-supplier relationship and translates it into protocol parameters. It publishes ProtocolPrescribed events that inform Lab Monitoring about required monitoring schedules and Side Effect Management about treatment-specific risk profiles.

## Lab Monitoring Context

This context governs the lifecycle of laboratory monitoring during active HRT treatment. It manages test ordering based on protocol-defined schedules, result capture and validation, trend analysis across sequential measurements, and threshold-based alerting when results fall outside therapeutic target ranges. The primary aggregate is the MonitoringPlan, which tracks scheduled tests, completed results, and alert status for a given patient-protocol pair.

Key value objects include TherapeuticTargetRange, LabResult, and TrendAnalysis. The context publishes LabResultRecorded and ThresholdExceeded events consumed by Side Effect Management and Treatment Optimization.

## Side Effect Management Context

This context tracks adverse events temporally associated with HRT, maintains contraindication databases, performs risk stratification, and coordinates mitigation strategies. The primary aggregate is the AdverseEventRecord, which captures event details, severity, causality assessment, and resolution status. The ContraindicationRegistry is a supporting aggregate that maintains the current set of absolute and relative contraindications.

This context consumes events from Lab Monitoring (threshold exceedances) and Clinical Assessment (risk factor updates) to proactively identify emerging complications. It publishes SideEffectReported and RiskLevelChanged events consumed by Treatment Optimization.

## Treatment Optimization Context

This context performs the analytical work of adjusting treatment protocols based on accumulated evidence. It handles dose titration calculations, formulation switching, combination therapy planning, and protocol modification recommendations. The primary aggregate is the OptimizationRecommendation, which synthesizes lab trends, side effect history, and outcomes data into actionable protocol change proposals.

The context operates as a downstream consumer of data from Lab Monitoring, Side Effect Management, and Outcomes Tracking. It publishes OptimizationRecommended events that the Hormone Protocol Context may accept or reject based on prescriber review.

## Outcomes Tracking Context

This context measures treatment effectiveness through symptom resolution rates, quality of life scores using validated instruments, and long-term health outcome tracking including bone density, cardiovascular health, and metabolic parameters. The primary aggregate is the TreatmentOutcome, which aggregates longitudinal measurements for a patient over their treatment history.

This context consumes data from all other contexts and publishes OutcomeAssessed events that feed back into Treatment Optimization for evidence-based protocol refinement.

## Boundary Integrity

Each bounded context maintains strict ownership of its domain model. Cross-context communication occurs exclusively through domain events and well-defined service interfaces. No context directly accesses another context's data store. Translation between context-specific representations occurs through anti-corruption layers or published language contracts.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 14 on maintaining model integrity.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 2 on domains, subdomains, and bounded contexts.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapters 6-7 on bounded contexts.
- Stuenkel, Cynthia A., et al. "Treatment of Symptoms of the Menopause." The Journal of Clinical Endocrinology & Metabolism, 2015.
