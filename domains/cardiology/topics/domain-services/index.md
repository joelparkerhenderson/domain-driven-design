# Domain Services in Cardiology

## Overview

Domain services encapsulate stateless operations that do not naturally belong to a single entity or value object. In the cardiology domain, domain services represent clinical operations that span multiple aggregates, enforce cross-aggregate business rules, or perform complex calculations requiring data from multiple sources. Unlike entities or value objects, domain services do not hold state between invocations; they receive input, perform an operation, and return a result.

## When to Use Domain Services

A domain service is appropriate in cardiology when:

- The operation involves multiple aggregates within the same bounded context that cannot be meaningfully placed in either aggregate.
- The operation implements a clinical algorithm or guideline-directed decision logic that requires data from multiple sources.
- The operation coordinates a workflow across multiple entities while enforcing business rules.
- Placing the logic in an entity would create an unnatural responsibility that does not align with the entity's clinical purpose.

## Domain Services by Bounded Context

### Clinical Assessment Context

**RiskCalculationService**: Calculates cardiovascular risk scores (Framingham, ASCVD, HEART, TIMI, GRACE) from patient demographic data, clinical findings, and laboratory results. This service spans the PatientAssessment aggregate (for clinical findings) and biomarker value objects. It applies the specific mathematical formulas defined in each scoring instrument's validation studies and returns a RiskScore value object.

**ClinicalDecisionSupportService**: Evaluates clinical findings, risk scores, and biomarker results against ACC/AHA guideline criteria to generate diagnostic and therapeutic recommendations. This service coordinates data from multiple aggregates (assessments, risk stratifications, stress tests) and applies guideline decision trees to produce recommendation sets.

**ContraindicationCheckService**: Evaluates a proposed diagnostic test or therapy against a patient's comorbidity list, medication list, and allergy history to identify contraindications. Used before ordering stress tests, initiating anticoagulation, or prescribing GDMT.

### Diagnostic Imaging Context

**MeasurementNormalizationService**: Normalizes cardiac imaging measurements by body surface area (BSA) to generate indexed values. Takes raw chamber dimensions, volumes, and mass measurements and returns BSA-indexed values with reference range comparisons based on age and sex. Spans the ImagingStudy aggregate and patient demographic data.

**StudyComparisonService**: Compares measurements from a current imaging study against prior studies for the same patient to identify clinically significant changes. Returns a set of measurement deltas with clinical significance flags (e.g., LVEF decreased by more than 10 percentage points).

**ReportGenerationService**: Assembles structured imaging measurements, clinical correlation data, and automated interpretive statements into a formatted imaging report. Coordinates data from the ImagingStudy aggregate and reference range databases.

### Interventional Procedures Context

**SYNTAXScoringService**: Calculates the SYNTAX score from angiographic lesion characteristics documented during cardiac catheterization. Aggregates individual lesion scores across all diseased coronary segments and returns the composite score with complexity classification. This service spans multiple CoronaryLesion entities within a CatheterizationProcedure aggregate.

**RevascularizationStrategyService**: Evaluates documented coronary anatomy (SYNTAX score), patient comorbidities, and clinical presentation against guideline criteria to recommend between PCI, CABG, or medical therapy. Implements Heart Team decision algorithms.

**ContrastDoseMonitoringService**: Tracks cumulative contrast agent volume during a procedure against the patient's estimated safe dose threshold (based on renal function). Alerts when approaching or exceeding the maximum recommended contrast volume.

### Electrophysiology Context

**ArrhythmiaClassificationService**: Classifies recorded cardiac rhythms based on electrophysiological characteristics (rate, regularity, P-wave morphology, QRS width, conduction pattern). Applies standard arrhythmia classification algorithms to intracardiac recordings.

**DeviceCompatibilityService**: Evaluates whether a specific pacemaker or ICD model is compatible with a patient's existing leads, anatomy, and clinical requirements. Considers lead connector standards (IS-1, DF-4), MRI conditional status, and device longevity projections.

**StrokeRiskAssessmentService**: Calculates CHA2DS2-VASc and HAS-BLED scores for atrial fibrillation patients to guide anticoagulation decisions. Combines demographic data, comorbidity data, and bleeding risk factors from multiple sources.

### Heart Failure Management Context

**GDMTOptimizationService**: Evaluates a patient's current GDMT regimen against guideline-recommended target doses, identifies medications eligible for uptitration, checks contraindications and laboratory parameters (potassium, creatinine, blood pressure), and generates a prioritized titration plan. This is a central domain service that spans the GDMTRegimen aggregate, HeartFailureProfile aggregate, and laboratory value objects.

**HeartFailureClassificationService**: Determines a patient's heart failure phenotype (HFrEF, HFmrEF, HFpEF) and ACC/AHA stage based on current ejection fraction, symptom status, and clinical history. Applies the ACC/AHA 2022 classification criteria.

**AdvancedTherapyEligibilityService**: Evaluates whether a heart failure patient meets criteria for advanced therapies (VAD implantation, cardiac transplantation) based on NYHA class, peak VO2, hemodynamic parameters, and failure of optimal GDMT. Implements INTERMACS profiling.

### Cardiac Rehabilitation Context

**ExercisePrescriptionService**: Generates an individualized exercise prescription based on the patient's functional capacity (MET level), cardiac diagnosis, procedural history, current medications (particularly beta-blockers affecting heart rate response), and rehabilitation phase. Applies ACSM exercise prescription guidelines.

**OutcomesCalculationService**: Calculates standardized rehabilitation outcome measures including change in functional capacity, risk factor goal attainment rates, program adherence percentages, and composite outcome scores. Aggregates data across multiple RehabilitationSession entities.

**RiskFactorGoalSettingService**: Establishes individualized risk factor modification targets (LDL, blood pressure, HbA1c, BMI) based on the patient's cardiovascular risk profile and current ACC/AHA prevention guidelines.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 5 on domain services.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 7 on domain services.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 7 on domain service patterns.
- ACC/AHA 2022 Guideline for the Diagnosis and Management of Heart Failure.
- ACSM Guidelines for Exercise Testing and Prescription, 11th Edition.
