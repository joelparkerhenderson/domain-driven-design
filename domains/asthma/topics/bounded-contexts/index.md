# Bounded Contexts - Asthma Domain

## Overview

Bounded contexts are logical boundaries within which a specific domain model is defined and applicable. In the asthma management domain, six bounded contexts partition the system along clinical, pharmacological, environmental, and patient-centered lines. Each bounded context maintains its own ubiquitous language, its own model of shared concepts, and its own consistency rules.

The choice of bounded context boundaries reflects how asthma care is organized in practice: different teams, different clinical guidelines, different data sources, and different update cycles govern each area of concern.

## Clinical Assessment Context

**Purpose:** Own the diagnostic and monitoring workflow for asthma patients.

**Key Concepts:** Spirometry test sessions, peak flow measurements, FeNO readings, bronchial challenge tests, GINA severity classification, asthma control level assessment.

**Aggregate Roots:** PatientAssessment, SpirometrySession, PeakFlowRecord.

**Responsibilities:**
- Record and validate pulmonary function test results (FEV1, FVC, FEV1/FVC ratio, PEF).
- Perform FeNO testing and interpret eosinophilic inflammation markers.
- Classify asthma severity using GINA criteria (intermittent, mild persistent, moderate persistent, severe persistent).
- Assess current asthma control level (well-controlled, partly controlled, uncontrolled).
- Detect bronchial hyperresponsiveness through methacholine or exercise challenge.

**Boundary Rationale:** Diagnostic assessment follows strict clinical protocols and regulatory requirements for test calibration and interpretation. Separating assessment from treatment ensures that diagnostic objectivity is maintained.

## Treatment Management Context

**Purpose:** Manage therapeutic interventions and treatment planning for asthma patients.

**Key Concepts:** Stepwise therapy plans, treatment steps (1-5), biologic agent eligibility, allergen immunotherapy protocols, inhaler device selection, inhaler technique assessment.

**Aggregate Roots:** TreatmentPlan, BiologicTherapy, ImmunotherapyProtocol.

**Responsibilities:**
- Maintain stepwise therapy plans aligned with GINA steps 1 through 5.
- Evaluate step-up and step-down criteria based on assessment results.
- Manage biologic agent selection based on phenotype and endotype classification.
- Track allergen immunotherapy schedules and dose escalation protocols.
- Assess and score inhaler technique proficiency.

**Boundary Rationale:** Treatment decisions require specialized pharmacological knowledge and evolve as new therapies gain approval. This context must adapt rapidly to updated clinical guidelines and new drug approvals.

## Trigger Monitoring Context

**Purpose:** Track and analyze environmental, allergen, and occupational factors that provoke asthma symptoms.

**Key Concepts:** Allergen exposure records, air quality index readings, pollen counts, occupational exposure assessments, environmental factor logs, seasonal patterns.

**Aggregate Roots:** TriggerProfile, EnvironmentalExposure, OccupationalAssessment.

**Responsibilities:**
- Record patient-specific allergen sensitivities and exposure events.
- Integrate air quality index (AQI) data from external monitoring services.
- Track indoor environmental factors (dust mites, mold, pet dander, tobacco smoke).
- Assess occupational exposures (chemical sensitizers, industrial irritants).
- Correlate trigger exposure patterns with exacerbation events.

**Boundary Rationale:** Trigger monitoring involves external data sources (air quality APIs, pollen databases) and environmental health expertise distinct from clinical medicine. This context operates on different time scales (continuous monitoring vs. periodic clinical visits).

## Action Planning Context

**Purpose:** Create and manage personalized asthma action plans that guide patient self-management.

**Key Concepts:** Asthma action plans, zone system (green/yellow/red), zone thresholds, self-management instructions, emergency protocols, shared decision-making records.

**Aggregate Roots:** AsthmaActionPlan, EmergencyProtocol.

**Responsibilities:**
- Define personalized zone thresholds based on patient best PEF and symptom criteria.
- Specify green zone (well-controlled) daily management instructions.
- Define yellow zone (caution) symptom recognition and step-up actions.
- Establish red zone (emergency) medication dosing and emergency contact procedures.
- Record shared decision-making discussions and patient preferences.

**Boundary Rationale:** Action plans translate clinical concepts into patient-understandable instructions. They bridge the professional clinical model and the patient self-management model, requiring distinct design that prioritizes clarity and actionability.

## Medication Management Context

**Purpose:** Handle medication prescriptions, schedules, adherence monitoring, and pharmacy integration.

**Key Concepts:** Controller medication prescriptions, rescue inhaler prescriptions, medication schedules, adherence records, refill tracking, inhaler device inventory.

**Aggregate Roots:** MedicationRegimen, Prescription, AdherenceRecord.

**Responsibilities:**
- Manage active medication prescriptions and dosing schedules.
- Track rescue inhaler usage frequency as an indicator of asthma control.
- Monitor medication adherence through refill data and electronic monitoring.
- Alert on non-adherence patterns or excessive rescue inhaler use.
- Coordinate with pharmacy systems for prescription fulfillment.

**Boundary Rationale:** Medication management follows pharmaceutical regulations and pharmacy workflows that differ from clinical assessment and treatment planning. This context interacts with external pharmacy and insurance systems.

## Outcomes Tracking Context

**Purpose:** Measure, record, and analyze clinical outcomes over time to evaluate treatment effectiveness.

**Key Concepts:** ACT scores, exacerbation records, lung function trends, quality of life assessments (AQLQ), healthcare utilization metrics, outcome reports.

**Aggregate Roots:** PatientOutcomeRecord, ExacerbationEvent, QualityOfLifeAssessment.

**Responsibilities:**
- Record periodic Asthma Control Test (ACT) scores and track trends.
- Log exacerbation events with severity, treatment required, and contributing factors.
- Maintain longitudinal lung function data (FEV1 trends, PEF trends).
- Administer and score quality of life questionnaires (AQLQ, mini-AQLQ).
- Generate outcome reports for clinical review and population health analysis.

**Boundary Rationale:** Outcomes tracking requires longitudinal data aggregation and statistical analysis capabilities that differ from transactional clinical workflows. This context serves both individual patient management and population-level quality improvement.

## Cross-Context Communication

Bounded contexts communicate through well-defined interfaces:

- Clinical Assessment publishes assessment completion events consumed by Treatment Management and Action Planning.
- Treatment Management publishes treatment plan changes consumed by Medication Management and Action Planning.
- Medication Management publishes adherence alerts consumed by Outcomes Tracking and Treatment Management.
- Trigger Monitoring publishes high-risk exposure alerts consumed by Action Planning.
- Outcomes Tracking publishes outcome trend alerts consumed by Treatment Management.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 14 on maintaining model integrity.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 2 on bounded contexts.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 7 on defining bounded context boundaries.
- Global Initiative for Asthma (GINA). Global Strategy for Asthma Management and Prevention, 2023.
