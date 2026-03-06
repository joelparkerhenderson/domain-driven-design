# Clinical Assessment Context - Asthma Domain

## Overview

The Clinical Assessment Context is responsible for all diagnostic testing, severity classification, and ongoing assessment of asthma control. This bounded context owns the models for spirometry testing, peak flow monitoring, fractional exhaled nitric oxide (FeNO) measurement, bronchial challenge testing, and the GINA-based severity and control classification system. It is one of two core subdomains in the asthma management domain.

Clinical assessment forms the foundation of evidence-based asthma management. Every treatment decision, action plan modification, and outcome evaluation depends on accurate, standardized clinical assessment data. The Clinical Assessment Context ensures that diagnostic data is captured with appropriate quality controls, validated against clinical standards, and classified using internationally recognized criteria.

## Key Concepts

**Spirometry** is the primary objective measurement tool. A spirometry session consists of multiple forced expiratory maneuvers, from which the best FEV1 (forced expiratory volume in one second) and best FVC (forced vital capacity) are selected. The FEV1/FVC ratio below the lower limit of normal confirms obstructive airway disease. Bronchodilator reversibility testing (improvement of FEV1 by at least 12% and 200mL after SABA administration) supports asthma diagnosis.

**Peak Expiratory Flow (PEF)** monitoring provides daily self-assessment data. Patients measure PEF using portable peak flow meters, typically morning and evening. The personal best PEF (highest value achieved during well-controlled periods) serves as the reference for action plan zone thresholds. PEF variability greater than 10% in adults suggests variable airflow limitation consistent with asthma.

**Fractional Exhaled Nitric Oxide (FeNO)** measures airway inflammation biomarkers. Elevated FeNO (above 50 ppb in adults) indicates eosinophilic airway inflammation that is typically responsive to inhaled corticosteroids. FeNO helps guide treatment decisions by identifying patients likely to benefit from anti-inflammatory therapy and monitoring treatment response.

**GINA Classification** uses a two-dimensional framework: severity classification (based on treatment required to control symptoms) and control assessment (based on current symptom burden and future risk). Severity is classified retrospectively after a period of treatment as intermittent, mild persistent, moderate persistent, or severe persistent. Control is assessed at each visit as well-controlled, partly controlled, or uncontrolled.

## Aggregates

### PatientAssessment

The central aggregate of this context. Contains the GINA classification, control level assessment, risk factor evaluation, and clinical notes. Invariants ensure that classifications are supported by objective data and that assessments are signed off by qualified clinicians.

### SpirometrySession

Encapsulates a complete spirometry testing session with multiple maneuvers, quality grading, and calculated results. Enforces ATS/ERS quality standards: at least three acceptable maneuvers, reproducibility criteria, and quality grading (A through F).

### PeakFlowRecord

Captures longitudinal peak flow measurements with personal best tracking. Enforces physiological plausibility ranges and personal best update rules.

## Entities

- **PatientAssessment:** Identity by AssessmentID. Lifecycle: draft, finalized, amended.
- **SpirometryManeuver:** Identity within a session. Contains individual maneuver measurements and quality flags.
- **AssessmentNote:** Immutable clinical observations attached to an assessment.

## Value Objects

- **FEV1:** Actual value, predicted value, percent predicted.
- **FVC:** Actual value, predicted value, percent predicted.
- **FEV1FVCRatio:** Ratio value, lower limit of normal.
- **PEFReading:** Value in L/min, timestamp, measurement context.
- **FeNOReading:** Value in ppb, clinical interpretation.
- **GINAClassification:** Severity level (intermittent through severe persistent).
- **ControlLevel:** Well-controlled, partly controlled, uncontrolled.
- **QualityGrade:** ATS/ERS spirometry quality grade (A through F).
- **PredictedValues:** Reference values adjusted for age, sex, height, ethnicity using GLI-2012 equations.

## Domain Events Published

- **AssessmentFinalized:** Triggers treatment review and action plan evaluation.
- **SpirometrySessionCompleted:** Provides data for lung function trending.
- **SeverityReclassified:** Triggers treatment plan and action plan revision.
- **ControlLevelChanged:** Alerts treatment management to evaluate therapy adequacy.

## Domain Services

- **AsthmaClassificationService:** Applies GINA criteria to classify severity and assess control.
- **PhenotypingService:** Determines asthma phenotype and endotype based on clinical and biomarker data.

## Integration Points

- **Upstream to Treatment Management:** Assessment results drive treatment step decisions. Published via AssessmentFinalized events.
- **Upstream to Action Planning:** PEF personal best and control level inform zone thresholds. Conformist relationship.
- **Shared Kernel with Outcomes Tracking:** Lung function value objects (FEV1, FVC, PEF) are shared for consistent longitudinal tracking.
- **ACL to EHR Systems:** FHIR Observation and DiagnosticReport resources are translated to domain objects.
- **ACL to Laboratory Systems:** FeNO and blood eosinophil results are received and translated.

## Clinical Standards Alignment

- **ATS/ERS Spirometry Standards (2019):** Quality criteria, acceptability, reproducibility, and grading.
- **GINA 2023:** Severity classification and control assessment criteria.
- **ATS Clinical Practice Guidelines for FeNO:** Interpretation thresholds and clinical use.
- **GLI-2012 Reference Equations:** Predicted spirometry values for diverse populations.

## Business Rules

1. Spirometry sessions require at least three acceptable maneuvers per ATS/ERS standards.
2. GINA classification cannot be assigned without at least one objective measurement (spirometry or PEF).
3. Bronchodilator reversibility requires documentation of pre-BD and post-BD FEV1.
4. FeNO interpretation must account for known confounders (smoking, atopy, age, recent infection).
5. Control assessment must evaluate both impairment (symptoms, reliever use) and risk (exacerbation history, lung function decline).
6. Amended assessments must preserve the original assessment with documented amendment rationale.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- Global Initiative for Asthma (GINA). Global Strategy for Asthma Management and Prevention, 2023.
- Graham et al. ATS/ERS Standardization of Spirometry 2019 Update. American Journal of Respiratory and Critical Care Medicine, 2019.
- Dweik et al. ATS Clinical Practice Guideline: Interpretation of Exhaled Nitric Oxide Levels. American Journal of Respiratory and Critical Care Medicine, 2011.
