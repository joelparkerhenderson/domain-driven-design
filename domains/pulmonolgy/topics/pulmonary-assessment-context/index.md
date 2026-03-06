# Pulmonary Assessment Context

## Overview

The Pulmonary Assessment Context manages the initial clinical evaluation of patients presenting with respiratory symptoms. It encompasses pulmonary function testing (PFTs), chest imaging interpretation, and structured symptom evaluation. This context serves as the primary upstream bounded context in the pulmonolgy domain, producing the assessment data that drives diagnostic, therapeutic, and rehabilitative decisions downstream.

Within this context, the term "assessment" specifically means the comprehensive initial evaluation of a patient's respiratory status, including history, physical examination findings, standardized symptom scores, pulmonary function test results, and chest imaging interpretation. This definition is distinct from the use of "assessment" in other contexts such as rehabilitation functional assessment or critical care weaning readiness assessment.

## Clinical Scope

The Pulmonary Assessment Context covers the following clinical activities:

- **Symptom Evaluation**: Structured collection of respiratory symptoms including cough, dyspnea, wheezing, chest pain, hemoptysis, and sputum production. Uses validated instruments such as the mMRC dyspnea scale, CAT score, and ACQ (Asthma Control Questionnaire).

- **Pulmonary Function Testing**: Ordering, conducting, and interpreting spirometry, lung volume measurements, diffusion capacity (DLCO) testing, and bronchodilator reversibility testing. Includes quality assurance for test acceptability and reproducibility.

- **Chest Imaging Interpretation**: Integrating chest radiograph and CT scan findings into the clinical assessment. The context does not perform radiology reads but interprets radiologist reports in the pulmonary clinical context.

- **Clinical Impression Formation**: Synthesizing symptoms, PFT results, and imaging findings into a preliminary clinical impression that guides downstream diagnostic and management decisions.

## Aggregates

**Patient Assessment Aggregate**: The primary aggregate in this context. The root entity is PatientAssessment, which contains SymptomEvaluation entities, PulmonaryFunctionTestOrder entities, and ImagingStudyInterpretation value objects. The aggregate enforces the invariant that an assessment must include at least one symptom evaluation before it can be finalized.

**PFT Session Aggregate**: Manages the technical execution and quality control of pulmonary function testing. The root entity is PFTSession, containing individual test measurement value objects (SpirometryResult, LungVolumeResult, DLCOResult). Enforces ATS/ERS quality criteria for acceptability and reproducibility.

## Key Entities

- **Patient**: Carries demographic data, referral source, presenting complaint, and assessment history within this context.
- **Assessment Encounter**: Tracks the lifecycle of a specific assessment visit from scheduling through completion.
- **PFT Technician**: Represents the qualified technician performing pulmonary function tests, with certification tracking.

## Key Value Objects

- **SpirometryResult**: FEV1, FVC, FEV1/FVC ratio, and percent predicted values with quality grade.
- **LungVolumeResult**: TLC, RV, FRC, and RV/TLC ratio with percent predicted values.
- **SymptomSeverityScore**: Standardized symptom assessment score with instrument identification.
- **ImagingFinding**: Structured chest imaging finding with location, type, and clinical significance.

## Domain Events Published

- **AssessmentCompleted**: Signals that an assessment encounter has been finalized with all findings documented.
- **PFTSessionCompleted**: Signals that a PFT session meets quality criteria and results are finalized.
- **AbnormalFindingDetected**: Signals identification of a clinically significant abnormality requiring urgent follow-up.

## Domain Services

- **VentilatoryDefectClassificationService**: Classifies ventilatory defect patterns using ATS/ERS algorithms.
- **PFTQualityAssuranceService**: Evaluates PFT session quality against standardized criteria.
- **ClinicalSeverityAssessmentService**: Produces integrated severity assessments using multidimensional frameworks.

## Context Relationships

The Pulmonary Assessment Context is upstream to both the Respiratory Diagnostics Context and the Chronic Disease Management Context via customer-supplier relationships. Assessment findings and PFT results flow downstream to inform diagnostic procedure selection and chronic disease baseline establishment.

The context has a shared kernel relationship with the Sleep Medicine Context for respiratory symptom evaluation. Both contexts use the same symptom severity scales and daytime sleepiness assessments (Epworth Sleepiness Scale), and changes to this shared subset require bilateral agreement.

The context integrates with external radiology PACS systems through an anti-corruption layer that translates DICOM metadata and radiology report formats into the domain's internal imaging interpretation model.

## Business Rules

- An assessment encounter cannot be finalized without at least one documented symptom evaluation.
- PFT results must meet ATS/ERS quality criteria (minimum three acceptable maneuvers, two reproducible within 150 mL for FEV1 and FVC) before being included in the assessment.
- Bronchodilator reversibility testing is required when spirometry shows an obstructive pattern, unless clinically contraindicated.
- Abnormal findings exceeding defined severity thresholds must generate an AbnormalFindingDetected event for expedited follow-up.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- American Thoracic Society/European Respiratory Society. Standardisation of Spirometry. *European Respiratory Journal*, 2005.
- Global Initiative for Chronic Obstructive Lung Disease (GOLD). Global Strategy for the Diagnosis, Management, and Prevention of COPD, 2024.
