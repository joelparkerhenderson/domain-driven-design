# Pulmonary Rehabilitation Context

## Overview

The Pulmonary Rehabilitation Context coordinates multidisciplinary rehabilitation programs for patients with chronic lung disease within the pulmonolgy domain. It manages exercise training prescriptions, breathing technique instruction, patient education programs, and self-management action plan development. Pulmonary rehabilitation is a cornerstone of comprehensive chronic respiratory disease management, improving exercise capacity, reducing dyspnea, and enhancing quality of life.

As a generic subdomain, the Pulmonary Rehabilitation Context follows well-established, widely standardized protocols defined by ATS/ERS guidelines. The domain model captures program management, outcome measurement, and individualized prescription, but leverages existing frameworks rather than requiring extensive custom modeling.

## Clinical Scope

The Pulmonary Rehabilitation Context covers the following clinical activities:

- **Exercise Training**: Individualized aerobic exercise prescriptions (treadmill, cycle ergometer, walking programs) and resistance training programs. Includes intensity targeting based on cardiopulmonary exercise testing or field testing results, with adjustments for supplemental oxygen requirements and comorbidity limitations.

- **Breathing Techniques**: Instruction in pursed-lip breathing, diaphragmatic breathing, active cycle of breathing technique, and controlled breathing strategies. These techniques help patients manage dyspnea during daily activities and exercise.

- **Patient Education**: Structured education modules covering disease understanding, medication management, inhaler technique, energy conservation, nutrition, anxiety and depression management, and travel planning with respiratory disease.

- **Self-Management Planning**: Development of individualized action plans that empower patients to recognize symptom changes, adjust medications per protocol, and determine when to seek professional care. Includes zone-based plans (green, yellow, red) with specific triggers and responses.

- **Functional Capacity Assessment**: Standardized testing including the six-minute walk test (6MWT), incremental shuttle walk test, and endurance shuttle walk test. These assessments establish baseline capacity, guide exercise prescription, and measure program outcomes.

- **Outcome Measurement**: Pre- and post-program assessment of functional capacity, dyspnea, health-related quality of life (using instruments such as SGRQ and CRQ), and self-efficacy. Application of minimum clinically important difference (MCID) thresholds to determine clinical significance.

## Aggregates

**Rehabilitation Program Aggregate**: The primary aggregate for managing patient enrollment and program delivery. Root entity is RehabilitationProgram, containing ExercisePrescription entities, BreathingTechniqueAssignment value objects, and EducationModule records. Enforces the invariant that every program includes a baseline functional assessment and that exercise prescriptions are within safe parameters.

**Progress Assessment Aggregate**: Manages outcome measurement and progress tracking. Root entity is ProgressAssessment, containing SixMinuteWalkTestResult value objects, DyspneaScore value objects, and QualityOfLifeScore value objects. Enforces the invariant that progress assessments use consistent measurement tools across timepoints for valid comparison.

## Key Entities

- **Rehabilitation Enrollment**: Tracks the patient's program enrollment from referral through completion or withdrawal, including attendance and session history.
- **Exercise Session**: Records individual rehabilitation sessions with exercise modalities, physiological responses, and session modifications.
- **Rehabilitation Therapist**: Represents the physiotherapist or respiratory therapist delivering rehabilitation, with specializations and patient assignments.

## Key Value Objects

- **SixMinuteWalkDistance**: Distance walked with supplemental oxygen use, rest stops, vitals, and dyspnea scores.
- **ExercisePrescription**: Exercise modality, intensity target, duration, and frequency specification.
- **DyspneaScore**: Standardized dyspnea measurement using the Borg CR-10 or modified Borg scale.
- **QualityOfLifeScore**: Validated quality of life assessment with component scores.

## Domain Events Published

- **RehabilitationEnrollmentCompleted**: Signals completed enrollment with baseline assessment results and program plan.
- **FunctionalCapacityAssessed**: Signals completion of a progress assessment with results and baseline comparison.
- **RehabilitationProgramCompleted**: Signals program completion with outcome summary and maintenance plan.

## Domain Services

- **ExercisePrescriptionService**: Generates individualized exercise prescriptions based on functional capacity, disease severity, and comorbidities.
- **RehabilitationOutcomeService**: Evaluates program effectiveness using pre/post comparison and MCID thresholds.
- **SelfManagementPlanService**: Generates zone-based action plans tailored to the patient's disease, severity, and medication regimen.

## Context Relationships

The Pulmonary Rehabilitation Context is downstream of the Chronic Disease Management Context via a customer-supplier relationship. Disease severity data, treatment plans, and functional goals flow from chronic disease management to inform rehabilitation program design and exercise prescription parameters.

The context receives post-ICU patient data from the Critical Care Context through an anti-corruption layer. ICU-specific concepts are translated into rehabilitation-relevant functional status indicators and exercise tolerance estimates.

The context publishes FunctionalCapacityAssessed and RehabilitationProgramCompleted events that the Chronic Disease Management Context may consume to update disease records and adjust treatment plans based on rehabilitation outcomes.

## Business Rules

- Every rehabilitation program must begin with a baseline functional capacity assessment (6MWT or equivalent) before exercise training commences.
- Exercise intensity must be prescribed within documented safety parameters. For patients with exercise-induced desaturation, supplemental oxygen must be prescribed to maintain SpO2 above 88% during training.
- A minimum of 12 supervised sessions is required for a program to be considered complete per ATS/ERS guidelines.
- Progress assessments must use the same measurement instrument and conditions as the baseline to ensure valid comparison. The MCID for the 6MWT in COPD patients is 30 meters.
- Attendance below 50% of scheduled sessions triggers a retention intervention workflow including patient contact and barrier assessment.
- Program outcome reports must include pre/post comparison of at least functional capacity (6MWT), dyspnea (mMRC or Borg), and quality of life (SGRQ or CRQ).
- Self-management action plans must be reviewed with the patient, with demonstrated understanding documented before program completion.
- Maintenance exercise recommendations must be provided at program completion with documented follow-up plan.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- Spruit, M.A., et al. An Official ATS/ERS Statement: Key Concepts and Advances in Pulmonary Rehabilitation. *American Journal of Respiratory and Critical Care Medicine*, 2013.
- Rochester, C.L., et al. An Official ATS/ERS Policy Statement: Enhancing Implementation, Use, and Delivery of Pulmonary Rehabilitation. *American Journal of Respiratory and Critical Care Medicine*, 2015.
