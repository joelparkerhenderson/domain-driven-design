# Outcomes Tracking Context - Asthma Domain

## Overview

The Outcomes Tracking Context is responsible for measuring, recording, and analyzing clinical outcomes over time to evaluate treatment effectiveness and guide management decisions. This bounded context owns the models for Asthma Control Test (ACT) scoring, exacerbation event recording, lung function trend analysis, quality of life assessment, and healthcare utilization tracking. It is classified as a mixed subdomain (partially generic infrastructure with supporting-level clinical interpretation logic).

Longitudinal outcome measurement is essential for evidence-based asthma management. Without systematic outcomes tracking, clinicians rely on subjective impressions of treatment success, and the gradual decline of lung function or the slow accumulation of exacerbations may go undetected. The Outcomes Tracking Context transforms individual data points into actionable clinical intelligence.

## Key Concepts

**Asthma Control Test (ACT)** is a validated five-question patient-reported outcome measure that quantifies asthma control over the past four weeks. Questions cover activity limitation, shortness of breath frequency, nighttime symptoms, rescue inhaler use, and self-rated asthma control. Each question scores 1 to 5, yielding a total score of 5 to 25. Scores of 20 and above indicate well-controlled asthma; 16 to 19 indicate not well-controlled; 15 and below indicate very poorly controlled. The minimal clinically important difference (MCID) is 3 points.

**Exacerbation Rates** track the frequency and severity of asthma exacerbations over time. An exacerbation is defined as a worsening of symptoms requiring a change in treatment -- typically systemic corticosteroid use, emergency department visit, or hospitalization. Exacerbation rate (events per patient-year) is a primary outcome measure in asthma clinical trials and quality reporting. Severe exacerbations (requiring hospitalization or intensive care) are tracked separately from moderate exacerbations (requiring OCS burst or ED visit).

**Lung Function Trends** analyze the longitudinal trajectory of pulmonary function measurements. FEV1 trend analysis reveals whether lung function is stable, improving, or declining over time. The normal age-related decline in FEV1 is approximately 20-30 mL per year in healthy adults; accelerated decline in asthma patients suggests inadequate disease control or airway remodeling. Peak flow trends reveal variability patterns and seasonal influences.

**Quality of Life** is measured using validated instruments such as the Asthma Quality of Life Questionnaire (AQLQ), which assesses impact across four domains: symptoms, activity limitation, emotional function, and environmental stimuli. Each domain scores 1 to 7, where 7 represents no impairment. The MCID for AQLQ is 0.5 points. Quality of life assessment captures the patient perspective on disease burden that objective clinical measures alone may miss.

## Aggregates

### PatientOutcomeRecord

The central aggregate containing longitudinal outcome data for a patient. Includes ACT score history, lung function trends, exacerbation summaries, and quality of life assessments. Enforces data integrity rules such as valid score ranges and minimum data points for trend calculations.

### ExacerbationEvent

Encapsulates a single exacerbation episode with onset date, severity classification, treatment required, contributing factors, recovery duration, and follow-up actions. Enforces that severity is classified and treatment is documented.

### QualityOfLifeAssessment

Records a quality of life questionnaire administration with domain scores, overall score, instrument identification (AQLQ, mini-AQLQ), and assessment date. Enforces valid score ranges and complete domain scoring.

## Entities

- **LungFunctionTrend:** Longitudinal series of lung function measurements with calculated trajectory, rate of change, and clinical significance flags.
- **ExacerbationEvent:** A discrete exacerbation episode with full documentation of severity, treatment, contributing factors, and resolution.
- **HealthcareUtilizationRecord:** Records healthcare encounters related to asthma (ED visits, hospitalizations, urgent care, unscheduled office visits).

## Value Objects

- **ACTScore:** Total score (5-25), individual question scores, control classification, assessment date.
- **ExacerbationSummary:** Count, rate (per patient-year), severity distribution, analysis period.
- **QualityOfLifeScore:** Overall score (1-7), domain scores, instrument, assessment date.
- **LungFunctionDataPoint:** Measurement type (FEV1 or PEF), value, date, source context (clinic spirometry, home peak flow).
- **TrendAnalysis:** Direction (improving, stable, declining), rate of change, statistical confidence, clinical significance.
- **OutcomePeriod:** Start date, end date, duration for defining analysis windows.

## Domain Events Published

- **ACTScoreRecorded:** Published when a patient completes an ACT questionnaire. Consumed by Treatment Management (declining scores may trigger treatment review) and Action Planning (may prompt zone threshold reassessment).
- **ExacerbationRecorded:** Published when a clinician documents an exacerbation event. Consumed by Treatment Management (evaluates step-up need) and Trigger Monitoring (correlates with environmental data).
- **OutcomeTrendAlertRaised:** Published when longitudinal analysis detects a concerning trend (declining FEV1, increasing exacerbation frequency, worsening ACT scores). Consumed by Treatment Management (triggers proactive treatment plan review).
- **QualityOfLifeDeclineDetected:** Published when AQLQ scores decline by more than the MCID. Consumed by Treatment Management (evaluates treatment adequacy from patient perspective).

## Domain Events Consumed

- **SpirometrySessionCompleted (from Clinical Assessment):** Provides new lung function data points for trend analysis.
- **AssessmentFinalized (from Clinical Assessment):** Provides control level assessment for longitudinal tracking.
- **PrescriptionFilled (from Medication Management):** Tracks medication fill patterns for adherence correlation.
- **AdherenceAlertRaised (from Medication Management):** Records adherence alerts for correlation with outcome changes.
- **ActionPlanActivated (from Action Planning):** Records action plan changes for effectiveness analysis.
- **BiologicTherapyInitiated (from Treatment Management):** Marks initiation date for biologic response tracking.
- **TreatmentPlanSteppedUp/Down (from Treatment Management):** Records treatment changes for outcome attribution.

## Domain Services

- **OutcomeTrendAnalysisService:** Calculates lung function trajectories, detects statistically significant trends, and assesses clinical significance of outcome changes over defined periods.
- **TreatmentEffectivenessService:** Evaluates whether treatment plan changes have achieved their intended outcome goals by comparing pre-intervention and post-intervention outcome measures.
- **OutcomeReportGenerationService:** Compiles comprehensive outcome reports including ACT trends, exacerbation summaries, lung function trends, adherence rates, and quality of life scores for clinical review and population health analysis.

## Integration Points

- **Shared Kernel with Clinical Assessment:** Lung function value objects (FEV1, FVC, PEF) are shared for consistent representation.
- **Downstream from Medication Management:** Receives adherence and prescription fill data. Customer-supplier relationship.
- **Published Language from Trigger Monitoring:** Receives environmental exposure data in standardized format.
- **Upstream to Treatment Management:** Publishes outcome trend alerts that trigger treatment review. Customer-supplier relationship.

## Population Health Analytics

Beyond individual patient outcomes, this context supports population-level analysis:

- **Quality Metrics:** HEDIS measures for asthma (medication management for people with asthma, asthma medication ratio).
- **Cohort Analysis:** Outcome comparisons across patient cohorts defined by severity, treatment step, phenotype, or demographic factors.
- **Intervention Effectiveness:** Pre-post analysis of clinical program interventions on population outcome measures.
- **Benchmarking:** Comparison of practice-level outcome metrics against regional and national benchmarks.

## Business Rules

1. ACT scores must be between 5 and 25 (sum of five questions, each scored 1-5).
2. Trend calculations require a minimum of three data points over a minimum 3-month span.
3. Exacerbation severity must be classified as mild, moderate, or severe using standardized criteria.
4. ACT score changes of 3 or more points are clinically significant (MCID).
5. AQLQ score changes of 0.5 or more points are clinically significant (MCID).
6. FEV1 decline exceeding 40 mL per year triggers a clinical significance alert.
7. Two or more exacerbations in 12 months classifies a patient as high exacerbation risk.
8. Outcome reports must include the measurement period and data completeness assessment.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- Nathan et al. Development of the Asthma Control Test. Journal of Allergy and Clinical Immunology, 2004.
- Juniper et al. Asthma Quality of Life Questionnaire (AQLQ). European Respiratory Journal, 1999.
- Global Initiative for Asthma (GINA). Global Strategy for Asthma Management and Prevention, 2023.
