# Aggregates in the Pulmonolgy Domain

## Overview

Aggregates are clusters of related domain objects treated as a single unit for the purpose of data changes and consistency enforcement. Each aggregate has a root entity that serves as the sole entry point for external references, and all modifications to the aggregate's internal objects pass through the root. In the pulmonolgy domain, aggregates model the natural consistency boundaries within respiratory care workflows.

Aggregate design in healthcare domains requires careful attention to clinical consistency requirements. A pulmonary function test result must be internally consistent: its component measurements (FEV1, FVC, DLCO) must reference the same patient encounter, the same testing conditions, and the same quality standards. The aggregate pattern ensures this consistency by grouping these related measurements under a single root.

## Aggregates by Bounded Context

### Pulmonary Assessment Context

**Patient Assessment Aggregate**: Root entity is the PatientAssessment. Contains SymptomEvaluation entities, PulmonaryFunctionTestOrder entities, and ImagingStudyInterpretation value objects. The aggregate enforces the invariant that an assessment must include at least one symptom evaluation and that all test orders reference the same patient encounter.

**PFT Session Aggregate**: Root entity is the PFTSession. Contains individual test measurements (SpirometryMeasurement, LungVolumeMeasurement, DiffusionCapacityMeasurement) as value objects. The aggregate enforces quality control invariants: all measurements must meet acceptability and reproducibility criteria before the session is marked as complete.

### Respiratory Diagnostics Context

**Diagnostic Procedure Aggregate**: Root entity is the DiagnosticProcedure. Contains ProceduralStep entities, SpecimenCollection records, and ComplicationEvent value objects. The aggregate enforces procedural workflow invariants: informed consent must be documented before procedure initiation, and all specimens must be linked to the originating procedure.

**Diagnostic Result Aggregate**: Root entity is the DiagnosticResult. Contains individual test interpretations, pathology findings, and clinical correlation notes. The aggregate enforces the invariant that a result cannot be finalized without a reviewing clinician's attestation.

### Chronic Disease Management Context

**Treatment Plan Aggregate**: Root entity is the TreatmentPlan. Contains MedicationRegimen entities, MonitoringSchedule value objects, and ActionPlanThreshold value objects. The aggregate enforces the invariant that every active treatment plan must have a defined monitoring schedule and that medication changes are recorded with clinical justification.

**Exacerbation Record Aggregate**: Root entity is the ExacerbationRecord. Contains SymptomSeverity assessments, TreatmentResponse entries, and OutcomeEvaluation value objects. The aggregate maintains the invariant that an exacerbation record progresses through defined states (detected, active, resolving, resolved) and that each state transition is clinically justified.

### Critical Care Context

**Ventilation Session Aggregate**: Root entity is the VentilationSession. Contains VentilatorSetting value objects, ArterialBloodGasResult value objects, and WeaningAttempt entities. The aggregate enforces the invariant that ventilator setting changes are paired with physiological response documentation and that weaning attempts are systematically recorded.

**ARDS Management Aggregate**: Root entity is the ARDSManagement episode. Contains SeverityClassification value objects, LungProtectiveVentilationParameters, and PronePosSession entities. The aggregate enforces the invariant that ARDS severity is classified using the Berlin definition criteria and that lung-protective ventilation targets are maintained.

### Sleep Medicine Context

**Sleep Study Aggregate**: Root entity is the SleepStudy. Contains SleepStageScoring records, RespiratoryEventScoring value objects, and OxygenDesaturation events. The aggregate enforces the invariant that scoring follows AASM criteria and that the AHI calculation is consistent with the scored respiratory events.

**Therapy Management Aggregate**: Root entity is the TherapyManagement record. Contains DevicePrescription entities, PressureTitration results, and ComplianceRecord value objects. The aggregate enforces the invariant that therapy prescriptions are linked to a diagnostic sleep study and that compliance data is associated with the correct device and settings.

### Pulmonary Rehabilitation Context

**Rehabilitation Program Aggregate**: Root entity is the RehabilitationProgram. Contains ExercisePrescription entities, BreathingTechniqueAssignment value objects, and EducationModule records. The aggregate enforces the invariant that every program includes a baseline functional assessment and that exercise prescriptions are within safe parameters based on the patient's clinical status.

**Progress Assessment Aggregate**: Root entity is the ProgressAssessment. Contains SixMinuteWalkTestResult value objects, DyspneaScore value objects, and QualityOfLifeScore value objects. The aggregate enforces the invariant that progress assessments use the same measurement tools across timepoints to enable valid comparison.

## Aggregate Design Principles

Aggregates in the pulmonolgy domain follow these design principles:

- **Small Aggregates**: Each aggregate is kept as small as possible while maintaining necessary consistency. Large clinical records are decomposed into multiple aggregates connected by identity references rather than direct object references.

- **Consistency Boundaries**: Invariants that must be enforced transactionally are kept within a single aggregate. Cross-aggregate consistency is achieved through eventual consistency using domain events.

- **Identity References**: Aggregates reference other aggregates by identity (ID) rather than direct object references. A TreatmentPlan references a Patient by PatientId, not by holding a Patient object.

- **Clinical Invariants**: Aggregate invariants reflect real clinical rules. A ventilator setting change without physiological response documentation is clinically invalid, and the aggregate enforces this through its business rules.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 6 on aggregates, aggregate roots, and consistency boundaries.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 10 on effective aggregate design with practical rules for sizing.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 5 on aggregate design patterns and consistency enforcement.
