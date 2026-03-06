# Clinical Assessment Context

## Overview

The Clinical Assessment Context is the primary entry point for cardiac care in the cardiology domain. It encompasses the initial and ongoing evaluation of patients with known or suspected cardiovascular disease. This bounded context models the clinical reasoning process that cardiologists employ when taking a cardiac history, performing a cardiovascular physical examination, interpreting cardiac biomarkers, calculating risk scores, and ordering and interpreting stress tests.

This context operates as the principal upstream context in the cardiology domain, producing assessment data and clinical decisions that flow downstream to Diagnostic Imaging, Interventional Procedures, Heart Failure Management, and Cardiac Rehabilitation contexts.

## Core Concepts

### History and Physical Examination

The cardiac history captures presenting symptoms (chest pain, dyspnea, palpitations, syncope, edema), their characteristics (onset, duration, severity, provocation, relief), and relevant medical history (prior MI, prior PCI/CABG, valve disease, arrhythmia history, heart failure, hypertension, diabetes, hyperlipidemia, smoking, family history of premature CAD).

The cardiovascular physical examination models structured findings: heart sounds (S1, S2, S3, S4, murmurs with grading), jugular venous distension, peripheral edema, lung examination (crackles suggesting congestion), and peripheral pulse assessment.

### Risk Stratification

Risk stratification is a core function that categorizes patients into risk groups to guide diagnostic intensity and therapeutic urgency. The context models multiple validated risk scoring instruments:

- **ASCVD Risk Calculator**: 10-year atherosclerotic cardiovascular disease risk based on age, sex, race, total cholesterol, HDL, systolic blood pressure, blood pressure treatment status, diabetes, and smoking.
- **HEART Score**: Evaluates chest pain patients in acute settings using History, ECG, Age, Risk factors, and Troponin components.
- **TIMI Risk Score**: Predicts 14-day risk of death, MI, or urgent revascularization in unstable angina/NSTEMI patients.
- **GRACE Score**: Estimates in-hospital and 6-month mortality for acute coronary syndrome patients.
- **Framingham Risk Score**: Classic 10-year coronary heart disease risk calculator.

### Cardiac Biomarkers

The context models cardiac biomarker interpretation with clinical decision logic:

- **Troponin** (high-sensitivity cardiac troponin I or T): Serial measurement interpretation with delta values to differentiate acute MI from chronic elevation. Includes sex-specific 99th percentile thresholds.
- **BNP / NT-proBNP**: Heart failure diagnostic and prognostic biomarkers with age-adjusted reference ranges and clinical interpretation tiers.
- **CK-MB**: Supplementary myocardial injury marker used alongside troponin for timing of injury onset.
- **D-dimer**: Pulmonary embolism and aortic dissection screening in acute chest pain evaluation.

### Stress Testing

The context models stress testing protocols and interpretation:

- **Exercise Stress Testing**: Bruce protocol, modified Bruce protocol, and other treadmill protocols with stage progression, hemodynamic response (heart rate, blood pressure), exercise capacity in METs, ECG changes, and symptom assessment.
- **Pharmacological Stress Testing**: Dobutamine and vasodilator (regadenoson, adenosine) protocols for patients unable to exercise adequately.
- **Stress Imaging**: Coordination with the Diagnostic Imaging Context for stress echocardiography and nuclear stress testing. The Clinical Assessment Context manages the stress protocol execution while Diagnostic Imaging manages image acquisition and interpretation.

## Aggregates

- **PatientAssessment**: Root aggregate managing the clinical encounter, findings, vital signs, and assessment status.
- **RiskStratification**: Manages risk score calculations, input parameters, and calculated results.
- **StressTest**: Manages test protocol execution, contraindication checks, stage progression, and results.

## Key Domain Events Published

- PatientAssessmentCompleted
- CriticalBiomarkerDetected
- RiskStratificationUpdated
- StressTestCompleted
- CardiacEmergencyIdentified

## Integration Points

- **Downstream to Diagnostic Imaging**: Publishes imaging referral events with clinical indications.
- **Downstream to Interventional Procedures**: Publishes catheterization referrals with risk profiles.
- **Shared Kernel with Heart Failure Management**: Shares vital signs, biomarker values, and functional classification models.
- **Downstream to Cardiac Rehabilitation**: Publishes risk profiles and exercise capacity data.
- **ACL with EHR**: Translates generic clinical data into cardiology-specific assessment models.
- **ACL with Laboratory Systems**: Translates lab results into typed cardiac biomarker value objects.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- ACC/AHA 2021 Guideline for the Evaluation and Diagnosis of Chest Pain. *Circulation*, 2021.
- Gulati et al. 2021 AHA/ACC Guideline for the Evaluation and Diagnosis of Chest Pain. *JACC*, 2021.
- Thygesen et al. Fourth Universal Definition of Myocardial Infarction. *JACC*, 2018.
