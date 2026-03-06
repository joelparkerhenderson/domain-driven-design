# Chronic Disease Management Context

## Overview

The Chronic Disease Management Context supports the longitudinal care of patients with chronic respiratory conditions. It manages COPD, asthma, interstitial lung disease (ILD), pulmonary hypertension, and cystic fibrosis through treatment plan design, exacerbation tracking, medication management, and clinical outcome monitoring. This context represents a core subdomain because long-term patient outcomes depend on sophisticated disease-specific modeling.

Within this context, the term "management" encompasses the full lifecycle of chronic disease care: initial disease classification, treatment plan creation, ongoing monitoring, exacerbation detection and response, treatment optimization, and outcome measurement. The context maintains the patient's longitudinal respiratory disease history and serves as the central coordination point for chronic care workflows.

## Clinical Scope

The Chronic Disease Management Context covers the following clinical activities:

- **COPD Management**: GOLD staging, pharmacological step therapy, supplemental oxygen qualification, exacerbation prevention strategies, and comorbidity management. Tracks FEV1 decline trajectory and exacerbation frequency.

- **Asthma Management**: GINA step therapy, asthma control assessment, trigger identification, action plan management, and biologic therapy eligibility. Monitors symptom control and peak flow variability.

- **Interstitial Lung Disease Management**: Disease-specific classification (IPF, NSIP, hypersensitivity pneumonitis), antifibrotic therapy management, disease progression monitoring via FVC and DLCO trends, and transplant evaluation timing.

- **Pulmonary Hypertension Management**: WHO functional class assessment, right heart catheterization result integration, targeted therapy management (endothelin receptor antagonists, PDE-5 inhibitors, prostacyclin analogues), and hemodynamic monitoring.

- **Cystic Fibrosis Management**: CFTR modulator therapy management, airway clearance regimen design, infection surveillance, nutritional optimization, and transplant readiness assessment.

## Aggregates

**Treatment Plan Aggregate**: The central aggregate for care coordination. Root entity is TreatmentPlan, containing MedicationRegimen entities, MonitoringSchedule value objects, and ActionPlanThreshold value objects. Enforces the invariant that every active treatment plan must have a defined monitoring schedule and that medication changes are recorded with clinical justification.

**Exacerbation Record Aggregate**: Tracks acute episodes of symptom worsening. Root entity is ExacerbationRecord, containing SymptomSeverity assessments, TreatmentResponse entries, and OutcomeEvaluation value objects. Maintains the invariant that exacerbation records progress through defined states with clinically justified transitions.

**Chronic Disease Record Aggregate**: Maintains the longitudinal disease history. Root entity is ChronicDiseaseRecord, containing SeverityClassification history, diagnostic milestones, and disease-specific metrics. Enforces the invariant that severity reclassification requires documented clinical basis.

## Key Entities

- **Chronic Disease Record**: Tracks disease type, severity classification, diagnosis date, and progression history for each chronic condition a patient carries.
- **Treatment Plan**: Manages the active treatment strategy including medications, monitoring, and action plans.
- **Exacerbation**: Tracks discrete episodes of acute worsening with onset, severity, treatment, and resolution.

## Key Value Objects

- **GOLDStage**: COPD severity classification with spirometric grade, symptom group, and exacerbation risk.
- **GINAStep**: Asthma treatment step based on control assessment and risk factors.
- **ExacerbationSeverity**: Classification as mild, moderate, or severe with criteria.
- **ActionPlanThreshold**: Zone-based self-management thresholds (green, yellow, red).

## Domain Events Published

- **ExacerbationDetected**: Signals an acute worsening requiring clinical response.
- **TreatmentPlanRevised**: Signals a treatment plan modification with justification.
- **DiseaseProgressionDetected**: Signals longitudinal decline in key clinical metrics.
- **SeverityReclassified**: Signals a change in disease severity classification.

## Domain Services

- **DiseaseClassificationService**: Applies disease-specific classification algorithms (GOLD, GINA, WHO functional class).
- **ExacerbationPredictionService**: Identifies patients at elevated exacerbation risk using historical patterns and risk factors.
- **TreatmentOptimizationService**: Recommends treatment adjustments based on outcomes, guidelines, and patient response.

## Context Relationships

The Chronic Disease Management Context is downstream of the Pulmonary Assessment Context (customer-supplier) for baseline assessment data and downstream of the Respiratory Diagnostics Context (partnership) for diagnostic results that inform disease classification.

The context is upstream to the Critical Care Context (conformist) when chronic disease patients experience acute respiratory failure, providing disease history and treatment context. It is also upstream to the Pulmonary Rehabilitation Context (customer-supplier), providing disease severity data and treatment goals that inform rehabilitation program design.

## Business Rules

- Every chronic disease record must have a current severity classification based on the most recent assessment data.
- Treatment plan revisions must document the clinical rationale for each change and reference guideline recommendations where applicable.
- Exacerbation frequency is calculated on a rolling twelve-month basis for COPD management decisions.
- Asthma control must be assessed at every clinical encounter using a validated instrument (ACQ or ACT).
- ILD patients must have FVC and DLCO trending at defined intervals to detect disease progression early.
- Pulmonary hypertension patients must have WHO functional class documented at each visit with hemodynamic reassessment at defined intervals.
- Treatment plans automatically generate TreatmentPlanRevised events when medications are modified, ensuring downstream contexts are notified.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- Global Initiative for Chronic Obstructive Lung Disease (GOLD). Global Strategy for the Diagnosis, Management, and Prevention of COPD, 2024.
- Global Initiative for Asthma (GINA). Global Strategy for Asthma Management and Prevention, 2024.
