# Aggregates in the Urology Domain

## Overview

An aggregate is a cluster of related domain objects treated as a single unit for data consistency. Each aggregate has a root entity that serves as the sole entry point for external access. In the urology domain, aggregates encapsulate the clinical concepts that must maintain internal consistency, such as a patient's oncologic staging profile, a stone disease episode, or a surgical case record.

## Aggregate Design Principles

Aggregates in the urology domain follow core DDD principles: they enforce invariants within their boundary, they are loaded and persisted as a whole, and they communicate with other aggregates through domain events rather than direct references. The aggregate boundary defines a transactional consistency boundary, meaning all changes within an aggregate are committed atomically.

## Clinical Assessment Aggregates

### DiagnosticWorkup Aggregate

The DiagnosticWorkup aggregate root encompasses a patient's complete urological evaluation for a presenting complaint. It contains SymptomScore value objects (IPSS, AUA-SI), PhysicalExamination entities, UrodynamicStudy entities, CystoscopyFinding entities, and ImagingResult entities. The aggregate enforces the invariant that a workup must have at least one diagnostic modality before a diagnostic conclusion can be rendered. The DiagnosticConclusion value object can only be set when sufficient evidence exists within the aggregate.

### UrodynamicSession Aggregate

The UrodynamicSession aggregate root manages a single urodynamic testing session. It contains Uroflowmetry, Cystometrogram, PressureFlowStudy, and ElectromyographyRecording entities. The aggregate enforces proper sequencing of test phases (filling phase must precede voiding phase) and ensures that all measurements reference the same session with consistent calibration baselines.

## Surgical Management Aggregates

### SurgicalCase Aggregate

The SurgicalCase aggregate root represents a single surgical episode from planning through post-operative recovery. It contains PreoperativeAssessment, OperativePlan, IntraoperativeRecord, and PostoperativeRecovery entities. The aggregate enforces invariants such as requiring a completed pre-operative assessment before the operative plan is finalized, and ensuring that surgical complications are recorded against the correct operative step.

### RoboticProcedure Aggregate

The RoboticProcedure aggregate is a specialized surgical case aggregate for robotic-assisted procedures. It adds ConsoleTime tracking, PortPlacement configuration, and RoboticInstrument usage records. The aggregate enforces invariants specific to robotic surgery such as required instrument counts and docking configuration validation.

## Oncologic Urology Aggregates

### CancerDiagnosis Aggregate

The CancerDiagnosis aggregate root manages a patient's urological cancer from initial detection through staging. It contains BiopsyResult entities, StagingClassification value objects (TNM), GleasonScore or FuhrmanGrade value objects, and RiskStratification value objects. The aggregate enforces staging consistency: the TNM stage must be compatible with the available pathological and imaging evidence.

### SurveillancePlan Aggregate

The SurveillancePlan aggregate root manages the long-term monitoring of a patient with a known or treated urological malignancy. It contains ScheduledVisit entities, SurveillanceResult entities, and ReclassificationEvent domain events. The aggregate enforces protocol adherence: surveillance intervals must follow the prescribed protocol, and reclassification triggers must be evaluated against each new result.

## Stone Disease Aggregates

### StoneEpisode Aggregate

The StoneEpisode aggregate root represents a single nephrolithiasis event from presentation through treatment and follow-up. It contains StoneCharacterization value objects (size, location, density in Hounsfield units, composition), TreatmentDecision entities, and ProcedureOutcome entities. The aggregate enforces the invariant that treatment selection must be consistent with stone characteristics (for example, ESWL is contraindicated for stones larger than 2 cm or cystine composition).

### MetabolicProfile Aggregate

The MetabolicProfile aggregate root manages a patient's stone-forming metabolic risk factors. It contains TwentyFourHourUrine value objects, SerumChemistry value objects, and DietaryAssessment entities. The aggregate enforces completeness invariants: a metabolic evaluation requires at least two 24-hour urine collections before a definitive metabolic diagnosis is rendered.

## Incontinence Management Aggregates

### ContinenceAssessment Aggregate

The ContinenceAssessment aggregate root captures the evaluation of a patient's incontinence. It contains IncontinenceClassification value objects, BladderDiary entities, PadWeightTest value objects, and PelvicFloorAssessment entities. The aggregate enforces classification consistency: the incontinence type must be supported by objective testing evidence.

## Male Reproductive Health Aggregates

### FertilityEvaluation Aggregate

The FertilityEvaluation aggregate root manages a male infertility workup. It contains SemenAnalysis value objects, HormonalProfile value objects, GeneticTestResult entities, and PhysicalExamination entities. The aggregate enforces completeness rules: at least two abnormal semen analyses separated by adequate intervals are required before proceeding to advanced evaluation.

## Aggregate Size Guidelines

Urology aggregates should be small enough to ensure performance and reduce contention, but large enough to enforce meaningful invariants. A SurgicalCase aggregate includes the operative record and its complications but does not include the patient's entire medical history. Cross-aggregate references use identifiers rather than direct object references, enabling loose coupling and independent persistence.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 6.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 10.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 6.
- Vernon, Vaughn. "Effective Aggregate Design." DDD Community, 2011.
