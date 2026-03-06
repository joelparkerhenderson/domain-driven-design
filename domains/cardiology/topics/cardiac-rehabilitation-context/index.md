# Cardiac Rehabilitation Context

## Overview

The Cardiac Rehabilitation Context supports structured, medically supervised programs that combine exercise training, cardiovascular risk factor modification, patient education, and psychosocial support for patients recovering from cardiac events or procedures. This bounded context models the multi-phase rehabilitation process from inpatient mobilization through outpatient supervised exercise and long-term maintenance, with a focus on measurable outcomes and secondary prevention.

This context is the most downstream context in the cardiology domain, consuming data from Clinical Assessment, Diagnostic Imaging, Heart Failure Management, and Interventional Procedures to design and monitor individualized rehabilitation programs.

## Core Concepts

### Rehabilitation Phases

The context models the standard three-phase cardiac rehabilitation framework:

**Phase I (Inpatient)**: Early mobilization during hospitalization following a cardiac event or procedure. Models activity progression from bed rest through ambulation, discharge readiness assessment, and patient education on activity guidelines. Typical duration is 3-7 days.

**Phase II (Outpatient Supervised)**: The core supervised exercise program typically consisting of 36 sessions over 12 weeks. Models exercise prescription, ECG-monitored sessions, progressive exercise intensity advancement, and risk factor modification counseling. This is the phase most heavily modeled in the domain, with detailed session tracking and outcomes measurement.

**Phase III (Maintenance)**: Long-term self-directed exercise and risk factor management with periodic reassessment. Models independent exercise programs, home exercise logging, annual reassessment visits, and community-based program referrals.

### Exercise Programs

Exercise prescription is the central clinical function of cardiac rehabilitation:

- **Functional Capacity Assessment**: Baseline exercise testing (6-minute walk test, symptom-limited graded exercise test) to determine MET capacity, peak heart rate, and hemodynamic response. Models assessment results as value objects with normative comparisons.
- **Exercise Prescription Parameters**: Mode (treadmill walking, stationary cycling, arm ergometry, recumbent stepping), intensity (target heart rate range using Karvonen formula, RPE scale, talk test), duration (minutes per session), and frequency (sessions per week). Beta-blocker adjustment: heart rate targets are modified for patients on beta-blockers using RPE-based methods.
- **Exercise Progression**: Systematic advancement of exercise intensity and duration based on patient tolerance, symptom response, and hemodynamic stability. Models progression criteria and advancement rules.
- **Safety Monitoring**: ECG telemetry monitoring during sessions, blood pressure and heart rate response tracking, symptom assessment (chest pain, dyspnea, dizziness, claudication), and session termination criteria. Models safety thresholds as configurable value objects.

### Risk Factor Modification

The context models individualized risk factor targets and tracking:

- **Lipid Management**: LDL-cholesterol targets (<70 mg/dL for very high risk, <100 mg/dL for high risk), statin therapy adherence tracking, and lipid panel trending.
- **Blood Pressure Management**: Target blood pressure (<130/80 mmHg per ACC/AHA guidelines), medication adherence, and serial measurement tracking.
- **Glycemic Control**: HbA1c targets for diabetic patients, diabetes self-management education referral, and glucose monitoring during exercise.
- **Tobacco Cessation**: Smoking status tracking, cessation intervention documentation (counseling, pharmacotherapy referral), and abstinence verification.
- **Weight Management**: BMI and waist circumference tracking, dietary counseling referrals, caloric intake goals, and weight trend monitoring.
- **Physical Activity**: Daily step counts, weekly active minutes, and adherence to prescribed home exercise between supervised sessions.

### Lifestyle Counseling

- **Nutritional Counseling**: Heart-healthy dietary patterns (Mediterranean, DASH), sodium restriction guidance, and dietitian referral tracking.
- **Psychosocial Support**: Depression screening (PHQ-9), anxiety screening (GAD-7), stress management education, and mental health referral tracking. Psychosocial distress is common post-cardiac event and impacts rehabilitation adherence and outcomes.
- **Return to Work and Activity**: Functional capacity-based activity clearance, occupational demands assessment, driving restrictions post-procedure, and sexual activity counseling.
- **Patient Education**: Disease-specific education modules (CAD, heart failure, arrhythmia, valve disease), medication education, and symptom recognition training.

### Outcomes Tracking

The context models standardized rehabilitation outcomes:

- **Functional Outcomes**: Change in MET capacity from baseline to program completion, 6-minute walk distance improvement, and functional classification change.
- **Risk Factor Outcomes**: Percentage of patients achieving LDL target, blood pressure target, HbA1c target, and smoking cessation.
- **Behavioral Outcomes**: Program completion rate (sessions attended vs. prescribed), home exercise adherence, and dietary goal adherence.
- **Clinical Outcomes**: 30-day and 1-year readmission rates, subsequent cardiac events, and mortality (tracked but typically reported by registries).
- **Patient-Reported Outcomes**: Quality of life scores (SF-36, Kansas City Cardiomyopathy Questionnaire for HF patients), depression scores, and patient satisfaction.

## Aggregates

- **RehabilitationProgram**: Root aggregate containing exercise prescription, risk factor goals, session schedule, and phase designation.
- **RehabilitationSession**: Root aggregate containing exercise performance data, vital signs, symptoms, and session outcome.

## Key Domain Events Published

- RehabilitationPhaseCompleted
- ExerciseCapacityImproved
- RiskFactorGoalAchieved
- RehabilitationProgramCompleted
- AdverseEventDuringSession

## Integration Points

- **Upstream from Clinical Assessment**: Receives risk profiles and exercise capacity data.
- **Upstream from Interventional Procedures**: Receives procedure completion data and activity restrictions.
- **Upstream from Heart Failure Management**: Receives GDMT status and exercise clearance decisions.
- **Upstream from Diagnostic Imaging**: Receives functional capacity data from stress imaging.
- **ACL with Insurance Systems**: Translates authorization requirements for session coverage.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- Thomas et al. 2019 ACC/AHA Guideline on the Primary Prevention of Cardiovascular Disease. *JACC*, 2019.
- AACVPR Guidelines for Cardiac Rehabilitation Programs, 6th Edition. Human Kinetics, 2020.
- ACSM Guidelines for Exercise Testing and Prescription, 11th Edition. Wolters Kluwer, 2021.
