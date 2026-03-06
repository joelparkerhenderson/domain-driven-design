# Pulmonary Rehabilitation Context

## Overview

The Pulmonary Rehabilitation Context coordinates structured rehabilitation programs for patients with chronic respiratory disease. It encompasses exercise program design, breathing retraining techniques, patient education curricula, outcome measurement using standardized functional tests, and discharge planning. This bounded context is classified as a supporting subdomain because rehabilitation programs follow evidence-based structures defined by ATS/ERS guidelines, though their effective delivery significantly impacts patient outcomes and quality of life.

This context consumes data from multiple upstream contexts: care plan information from Chronic Disease Management, assessment data from Clinical Assessment, diagnostic results from Respiratory Diagnostics, and ventilation weaning status from Ventilatory Support.

## Ubiquitous Language

Key terms within this context include: rehabilitation program, exercise prescription, aerobic training, resistance training, upper-limb training, lower-limb training, interval training, continuous training, breathing retraining, pursed-lip breathing, diaphragmatic breathing, active cycle of breathing technique, patient education, self-management education, six-minute walk test (6MWT), incremental shuttle walk test (ISWT), endurance shuttle walk test (ESWT), cardiopulmonary exercise test (CPET), Borg scale, functional capacity, exercise intensity, target heart rate, minimal clinically important difference (MCID), session attendance, program completion, discharge planning, maintenance program, and community referral.

## Aggregate: Rehabilitation Program

The Rehabilitation Program is the aggregate root, representing the full course of pulmonary rehabilitation for a patient.

**Components**:
- Program ID (unique identifier)
- Patient ID (reference)
- Referring clinician ID
- Primary diagnosis and disease severity
- Baseline functional assessment (6MWT result, symptom scores, quality-of-life scores)
- Exercise prescription (aerobic modalities, resistance exercises, intensity targets, duration, frequency)
- Breathing retraining plan (techniques prescribed, practice schedule)
- Education modules (assigned topics, completion tracking)
- Session schedule (planned dates, attendance record)
- Progress assessments (mid-program and end-program evaluations)
- Program status (enrolled, active, completed, withdrawn)
- Enrollment date, expected completion date
- Discharge plan (when program nears completion)

**Invariants**:
- Exercise intensity must not exceed the patient's assessed functional capacity limits.
- The program must include a minimum number of supervised sessions per established guidelines (typically 12-24 sessions over 6-12 weeks).
- Baseline functional assessment must be completed before exercise prescription is finalized.
- Education modules must include core topics: disease understanding, medication management, breathing techniques, energy conservation, and psychological support.
- A completed program must have end-of-program outcome assessment.

## Entities

**Exercise Prescription**: Defines the individualized exercise program with components for aerobic training (modality, intensity, duration, frequency, progression rules), resistance training (exercises, sets, repetitions, resistance level), and flexibility training. Intensity is specified as target heart rate range, target Borg dyspnea score, or percentage of peak workload from exercise testing.

**Education Module Assignment**: Represents the assignment of a specific education topic to a patient, tracking module content, delivery method (group session, individual, written material, video), completion date, comprehension assessment, and any follow-up needs. Core modules cover disease education, medication technique, breathing strategies, energy conservation, nutrition, psychosocial support, and advance care planning.

**Discharge Plan**: Documents the transition plan from supervised rehabilitation to independent maintenance. Includes recommended maintenance exercise program, community exercise options, follow-up assessment schedule, self-management goals, and referrals to community resources.

**Rehabilitation Session Record**: Tracks an individual rehabilitation session including the date, exercises performed, vital signs before and during exercise, Borg scores, oxygen supplementation if used, symptoms during exercise, and clinician observations.

## Value Objects

**SixMinuteWalkResult**: Captures the 6MWT distance (meters), pre- and post-walk SpO2 and heart rate, pre- and post-walk Borg dyspnea and fatigue scores, number of rest stops, and supplemental oxygen use. The minimal clinically important difference (MCID) for 6MWT distance in COPD is approximately 30 meters.

**ExerciseIntensity**: Specifies the target intensity for an exercise modality as a target heart rate range, target Borg score range (typically 3-5 for moderate intensity), percentage of peak VO2, or percentage of peak work rate.

**FunctionalCapacityLevel**: Classifies the patient's exercise capacity based on 6MWT distance, symptom burden, and oxygen requirements during exercise. Used to determine the appropriate starting intensity for exercise prescription.

**QualityOfLifeScore**: Captures scores from respiratory-specific quality-of-life instruments such as the St. George's Respiratory Questionnaire (SGRQ) or the Chronic Respiratory Disease Questionnaire (CRQ). Tracks total and domain scores for comparison across time points.

**ProgramOutcome**: Captures the change in key outcome measures from baseline to program end, including 6MWT distance change, symptom score changes, quality-of-life score changes, and whether changes meet or exceed the MCID.

## Domain Events

**RehabilitationProgramStarted**: Published when a patient begins the program. Consumed by the Chronic Disease Management Context to update the care plan.

**FunctionalAssessmentCompleted**: Published when a formal assessment (6MWT, shuttle walk test) is completed. Carries the assessment results for tracking by other contexts.

**ExercisePrescriptionUpdated**: Published when the exercise prescription is progressed or modified based on patient response.

**RehabilitationMilestoneAchieved**: Published when a patient reaches a defined goal such as a target 6MWT distance, completion of all education modules, or demonstrated self-management competency.

**PatientDischarged**: Published when a patient completes or withdraws from the program. Carries the final outcome assessment and discharge plan details. Consumed by the Chronic Disease Management Context.

## Domain Services

**ExercisePrescriptionService**: Generates an individualized exercise prescription based on functional assessment results, disease severity, comorbidities, and rehabilitation goals. Applies exercise training principles (specificity, overload, progression) within safe limits for respiratory patients.

**OutcomeMeasurementService**: Calculates changes in outcome measures between assessment time points, determines whether changes meet the MCID, and produces outcome summary reports.

**DischargeReadinessService**: Evaluates whether the patient has met criteria for program discharge including attendance thresholds, functional improvement, self-management knowledge demonstration, and completion of the maintenance exercise plan.

**ProgressionService**: Determines when and how to progress exercise intensity based on patient response, Borg scores during exercise, vital sign trends, and symptom patterns.

## Integration Points

- **Upstream**: Consumes CarePlanUpdated events from Chronic Disease Management, AssessmentCompleted events from Clinical Assessment, DiagnosticResultAvailable from Respiratory Diagnostics, and WeaningCompleted from Ventilatory Support.
- **Downstream**: Publishes PatientDischarged and milestone events consumed by Chronic Disease Management.

## References

- Evans, E. (2003). *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media.
- Spruit, M. A., et al. (2013). *An Official ATS/ERS Statement: Key Concepts and Advances in Pulmonary Rehabilitation*. AJRCCM.
- Holland, A. E., et al. (2014). *An Official ERS/ATS Technical Standard: Field Walking Tests in Chronic Respiratory Disease*. European Respiratory Journal.
