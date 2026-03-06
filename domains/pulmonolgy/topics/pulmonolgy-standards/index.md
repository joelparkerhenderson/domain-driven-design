# Pulmonolgy Standards

## Overview

Clinical standards and guidelines profoundly shape the domain model in pulmonolgy. Standards from organizations such as the American Thoracic Society (ATS), European Respiratory Society (ERS), Global Initiative for Chronic Obstructive Lung Disease (GOLD), Global Initiative for Asthma (GINA), and American Academy of Sleep Medicine (AASM) define classification systems, diagnostic criteria, treatment algorithms, and measurement protocols that must be faithfully represented in the domain model.

In Domain-Driven Design terms, these standards function as published languages and domain constraints. They inform value object design (defining what measurements mean and how they are classified), aggregate invariants (defining what clinical rules must be enforced), and domain service logic (defining the algorithms for classification and decision support). Understanding and incorporating these standards is essential for a clinically valid domain model.

## Pulmonary Function Testing Standards

### ATS/ERS Spirometry Standardization

The ATS/ERS standards for spirometry define the technical requirements for performing and interpreting spirometric tests. These standards directly inform the PFT Session aggregate and SpirometryResult value object design:

- Acceptability criteria: Maneuvers must have a good start (back-extrapolated volume less than 5% of FVC or 150 mL), no cough in the first second, no early termination, and no variable effort.
- Reproducibility criteria: The two largest FEV1 values must be within 150 mL, and the two largest FVC values must be within 150 mL.
- Session quality grading: Grade A through F based on the number of acceptable maneuvers and reproducibility of results.

These criteria are encoded in the PFTQualityAssuranceService and enforced as invariants in the PFT Session aggregate.

### ATS/ERS Lung Volume Standards

Standards for measuring total lung capacity, residual volume, and functional residual capacity using body plethysmography, nitrogen washout, or helium dilution methods. These inform the LungVolumeResult value object and the VentilatoryDefectClassificationService.

### ATS/ERS DLCO Standards

Standards for measuring diffusing capacity of the lung for carbon monoxide, including correction factors for hemoglobin concentration and altitude. These standards define the DLCOResult value object structure and quality criteria.

## Disease Classification Standards

### GOLD Classification for COPD

The Global Initiative for Chronic Obstructive Lung Disease provides the classification framework for COPD severity and management. The GOLD standard defines:

- Spirometric grading: GOLD 1 (FEV1 at least 80% predicted), GOLD 2 (50-79%), GOLD 3 (30-49%), GOLD 4 (less than 30%).
- Symptom group classification: Groups A through E based on symptom burden (mMRC or CAT) and exacerbation history.
- Pharmacological treatment algorithms: Step-wise pharmacological management based on symptom group and exacerbation risk.

The GOLDStage value object, DiseaseClassificationService, and TreatmentOptimizationService all encode GOLD criteria. Treatment plan aggregates reference GOLD recommendations for medication regimen design.

### GINA Classification for Asthma

The Global Initiative for Asthma provides the treatment step framework for asthma management:

- Asthma control assessment: Categorization as well-controlled, partly controlled, or uncontrolled based on symptom frequency, reliever use, activity limitation, and nocturnal symptoms.
- Treatment steps 1-5: Escalating pharmacological therapy from as-needed SABA to high-dose ICS/LABA with add-on therapies.
- Step-up and step-down criteria for treatment adjustment.

The GINAStep value object and DiseaseClassificationService encode these criteria.

### Berlin Definition for ARDS

The Berlin definition provides the classification framework for acute respiratory distress syndrome:

- Timing: Within one week of a known clinical insult or worsening respiratory symptoms.
- Imaging: Bilateral opacities not fully explained by effusions, lobar collapse, or nodules.
- Oxygenation: Mild (PaO2/FiO2 200-300), moderate (100-200), severe (less than 100) with PEEP at least 5 cmH2O.

The BerlinCriteria value object encodes these classification rules.

### AASM Scoring Criteria for Sleep Medicine

The AASM Manual for the Scoring of Sleep and Associated Events defines:

- Sleep stage scoring rules (N1, N2, N3, REM) based on EEG, EOG, and EMG patterns.
- Respiratory event definitions: Apnea (90% or greater airflow reduction for 10+ seconds), hypopnea (30% or greater reduction with 3% desaturation or arousal).
- AHI severity thresholds: Normal (less than 5), mild (5-14), moderate (15-29), severe (30+).

The AHIScore value object, SleepStageDistribution value object, and SleepStudyInterpretationService encode AASM criteria.

## Procedural Standards

### ATS Bronchoscopy Guidelines

Standards for diagnostic bronchoscopy define pre-procedure assessment requirements, sedation protocols, specimen collection methods, and post-procedure monitoring. These inform the DiagnosticProcedure aggregate and procedural business rules.

### BTS Pleural Procedures Guidelines

British Thoracic Society guidelines for pleural procedures define indications for thoracentesis, fluid analysis interpretation criteria (Light's criteria), and safety protocols. These inform the PleuraFluidAnalysis value object and procedural workflows.

## Rehabilitation Standards

### ATS/ERS Pulmonary Rehabilitation Statement

The ATS/ERS statement on pulmonary rehabilitation defines program components, outcome measurement instruments, and minimum clinically important differences (MCIDs) for key outcomes:

- 6MWT MCID: 30 meters for COPD patients.
- SGRQ MCID: 4-point reduction in total score.
- CRQ MCID: 0.5 points per item.

These MCIDs are encoded in the RehabilitationOutcomeService for determining clinical significance of program outcomes.

## Standards Governance in the Domain Model

Clinical standards are not static. GOLD and GINA release annual updates. AASM revises scoring rules periodically. ATS/ERS publishes updated position statements. The domain model must accommodate standard version tracking to ensure that clinical classifications are interpreted using the correct version of the applicable standard.

Value objects that encode standard-defined classifications carry version references. The DiseaseClassificationService is parameterized by standard version so that historical classifications can be understood in context of the standard version that was current at the time of classification.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- Graham, B.L., et al. Standardization of Spirometry 2019 Update. An Official ATS/ERS Technical Statement. *American Journal of Respiratory and Critical Care Medicine*, 2019.
- Global Initiative for Chronic Obstructive Lung Disease (GOLD). Global Strategy for the Diagnosis, Management, and Prevention of COPD, 2024.
- Global Initiative for Asthma (GINA). Global Strategy for Asthma Management and Prevention, 2024.
- Berry, R.B., et al. AASM Manual for the Scoring of Sleep and Associated Events. American Academy of Sleep Medicine, 2020.
