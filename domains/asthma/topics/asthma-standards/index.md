# Asthma Standards - Asthma Domain

## Overview

Domain-specific standards inform value object design, published languages, and business rule definitions in Domain-Driven Design. In the asthma management domain, clinical practice guidelines, diagnostic standards, pharmacological classifications, and quality measurement frameworks shape the domain model. These standards provide the authoritative basis for severity classification, treatment algorithms, outcome measures, and data exchange formats.

Understanding and correctly implementing clinical standards is critical because the domain model must reflect current evidence-based practice. Deviations from established standards undermine clinical validity, create regulatory risk, and erode trust among clinical domain experts.

## Global Initiative for Asthma (GINA)

GINA is the primary international clinical guideline for asthma management. Published annually with updates reflecting new evidence, GINA provides the framework for several core domain concepts.

### GINA Severity Classification

GINA classifies asthma severity retrospectively based on the level of treatment required to achieve good symptom control:

- **Intermittent:** Symptoms less than twice per week, no interference with activity, nighttime symptoms less than twice per month, FEV1 greater than 80% predicted.
- **Mild Persistent:** Well controlled on Step 1-2 treatment.
- **Moderate Persistent:** Well controlled on Step 3 treatment.
- **Severe Persistent:** Requires Step 4-5 treatment or remains uncontrolled despite Step 4-5 treatment.

**Domain Impact:** The GINAClassification value object directly encodes these categories. The AsthmaClassificationService applies these criteria.

### GINA Control Assessment

GINA assesses current asthma control based on four criteria evaluated over the past 4 weeks:

1. Daytime symptoms more than twice per week.
2. Any nighttime awakening due to asthma.
3. Reliever inhaler needed more than twice per week (excluding pre-exercise use).
4. Any activity limitation due to asthma.

- **Well-controlled:** None of the above criteria present.
- **Partly controlled:** 1-2 criteria present.
- **Uncontrolled:** 3-4 criteria present.

**Domain Impact:** The ControlLevel value object implements this assessment. The control assessment drives step-up and step-down decisions in the Treatment Management Context.

### GINA Stepwise Therapy

GINA defines five treatment steps with preferred and alternative controller options at each step:

- **Step 1:** As-needed low-dose ICS-formoterol (preferred) or as-needed SABA plus ICS.
- **Step 2:** Daily low-dose ICS (preferred) or LTRA, or as-needed low-dose ICS-formoterol.
- **Step 3:** Low-dose ICS-LABA (preferred) or medium-dose ICS, or low-dose ICS plus LTRA.
- **Step 4:** Medium-dose ICS-LABA (preferred), plus consideration of tiotropium or LTRA.
- **Step 5:** High-dose ICS-LABA, plus add-on therapy (tiotropium, biologic agents), referral for phenotypic assessment. Low-dose OCS only as last resort.

**Domain Impact:** The TherapyStep value object and StepwiseTherapyService encode these step definitions and transition rules.

## National Asthma Education and Prevention Program (NAEPP)

NAEPP Expert Panel Report 4 (EPR-4), published in 2020, is the US national guideline for asthma diagnosis and management.

### EPR-4 Key Recommendations

- Recommends intermittent ICS as alternative to daily ICS in mild persistent asthma.
- Endorses shared decision-making as central to treatment planning.
- Recommends allergen immunotherapy for allergic asthma patients with demonstrated sensitivity.
- Endorses FeNO-guided treatment adjustment for patients aged 5 and older.

**Domain Impact:** EPR-4 recommendations influence the Treatment Management Context business rules, particularly for initial therapy selection and FeNO-guided therapy adjustment.

## ATS/ERS Spirometry Standards

The American Thoracic Society (ATS) and European Respiratory Society (ERS) publish standards for spirometry testing, most recently updated in 2019.

### Acceptability and Reproducibility Criteria

- Maneuver must show satisfactory start (back-extrapolated volume less than 5% of FVC or 100 mL).
- Maneuver must show satisfactory exhalation (plateau in volume-time curve or exhalation time of at least 6 seconds in adults).
- No cough during the first second or other artifacts.
- Reproducibility: At least 2 acceptable maneuvers within 150 mL of each other for FEV1 and FVC.

### Quality Grading System

- **Grade A:** 3 or more acceptable maneuvers; FEV1 and FVC within 150 mL.
- **Grade B:** 3 or more acceptable maneuvers; FEV1 or FVC within 200 mL.
- **Grade C:** 2 or more acceptable maneuvers; FEV1 or FVC within 200 mL.
- **Grade D:** 2 or more acceptable maneuvers; FEV1 or FVC within 250 mL.
- **Grade E:** 1 acceptable maneuver.
- **Grade F:** 0 acceptable maneuvers.

**Domain Impact:** The SpirometrySession aggregate enforces these quality criteria. The QualityGrade value object encodes the grading system. The session cannot be finalized without quality grading.

## GLI-2012 Reference Equations

The Global Lung Function Initiative (GLI) 2012 reference equations provide predicted spirometry values and lower limits of normal for diverse populations. Predicted values are calculated from age, sex, height, and ethnic group.

**Domain Impact:** The PredictedValues value object uses GLI-2012 equations. The FEV1 and FVC value objects calculate percent predicted using these reference values.

## ATS FeNO Clinical Practice Guidelines

The ATS published guidelines for FeNO interpretation in 2011, with subsequent updates:

- **Low FeNO (<25 ppb in adults):** Eosinophilic inflammation less likely; ICS response less likely.
- **Intermediate FeNO (25-50 ppb):** Results should be interpreted cautiously in clinical context.
- **High FeNO (>50 ppb):** Eosinophilic inflammation likely; good ICS response expected.

**Domain Impact:** The FeNOReading value object encodes these thresholds and interpretation categories.

## Asthma Control Test (ACT) Validation

The ACT was validated by Nathan et al. (2004) as a patient-reported outcome measure:

- Five questions covering the past 4 weeks.
- Score range: 5 (poorly controlled) to 25 (completely controlled).
- Cut-point: Score of 19 or below indicates not well-controlled asthma.
- MCID: 3 points.
- Sensitivity: 71.3% and specificity: 70.8% for identifying uncontrolled asthma at the 19-point cut-off.

**Domain Impact:** The ACTScore value object enforces valid score ranges and provides control classification based on validated cut-points.

## Asthma Quality of Life Questionnaire (AQLQ)

The AQLQ, developed by Juniper et al., measures disease-specific quality of life:

- 32 items across 4 domains (symptoms, activity limitation, emotional function, environmental stimuli).
- 7-point scale (1 = maximum impairment, 7 = no impairment).
- MCID: 0.5 points per domain and overall.
- Mini-AQLQ: 15-item shortened version with comparable validity.

**Domain Impact:** The QualityOfLifeScore value object encodes domain scores, overall scores, and MCID calculations.

## Pharmacological Classification Standards

### RxNorm

RxNorm provides normalized names and codes for clinical drugs. Used in the MedicationIdentifier value object for unambiguous drug identification.

### National Drug Code (NDC)

NDC identifies specific drug products (manufacturer, product, package). Used in prescription management for pharmacy system integration.

### SNOMED CT

SNOMED CT provides clinical terminology codes for conditions, procedures, and findings. Used in anti-corruption layers for translating between clinical systems.

## Air Quality Standards

### EPA Air Quality Index (AQI)

The EPA AQI provides a standardized 0-500 scale for six criteria pollutants. AQI categories (Good, Moderate, Unhealthy for Sensitive Groups, Unhealthy, Very Unhealthy, Hazardous) inform trigger monitoring alert thresholds.

**Domain Impact:** The AirQualityReading value object encodes AQI values and category classifications.

## References

- Global Initiative for Asthma (GINA). Global Strategy for Asthma Management and Prevention, 2023.
- National Asthma Education and Prevention Program (NAEPP). Expert Panel Report 4, 2020.
- Graham et al. ATS/ERS Standardization of Spirometry 2019 Update. AJRCCM, 2019.
- Quanjer et al. GLI-2012 Multi-Ethnic Reference Values for Spirometry. ERJ, 2012.
- Dweik et al. ATS Clinical Practice Guideline: Interpretation of FeNO Levels. AJRCCM, 2011.
- Nathan et al. Development of the Asthma Control Test. JACI, 2004.
- Juniper et al. Asthma Quality of Life Questionnaire. ERJ, 1999.
- Evans, Eric. Domain-Driven Design. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
