# Domain Events in Cardiology

## Overview

Domain events represent significant occurrences within the cardiology domain that are meaningful to domain experts. They capture the fact that something clinically important happened at a specific point in time. Domain events enable loose coupling between bounded contexts by allowing one context to react to changes in another without direct dependencies. In cardiology, domain events model the natural flow of clinical information as patients move through assessment, diagnosis, treatment, and recovery.

## Domain Event Characteristics

Cardiology domain events share these characteristics:

- **Past tense naming**: Events describe something that has already occurred. "PatientAssessmentCompleted" rather than "CompletePatientAssessment."
- **Immutability**: Once published, an event's data cannot be changed. It represents a historical fact.
- **Self-contained**: Events carry sufficient data for consumers to act without querying the publishing context.
- **Temporal ordering**: Events carry timestamps enabling consumers to reconstruct the sequence of clinical occurrences.
- **Clinical significance**: Events represent occurrences that cardiologists, nurses, or technologists would recognize as meaningful.

## Key Domain Events by Bounded Context

### Clinical Assessment Context (Publisher)

**PatientAssessmentCompleted**: Published when a clinical assessment encounter is finalized. Carries patient identifier, assessment findings summary, risk scores calculated, and clinical recommendations. Consumed by Diagnostic Imaging (to anticipate imaging orders), Heart Failure Management (to update clinical status), and Cardiac Rehabilitation (to update risk profiles).

**CriticalBiomarkerDetected**: Published when a cardiac biomarker result exceeds critical thresholds (e.g., troponin elevation suggesting acute MI). Carries patient identifier, biomarker type, value, and clinical urgency level. Consumed by Interventional Procedures (to prepare for potential emergent catheterization) and Heart Failure Management (to assess for acute decompensation).

**RiskStratificationUpdated**: Published when a patient's cardiovascular risk classification changes. Carries patient identifier, previous risk level, new risk level, and the scoring instrument used. Consumed by all downstream contexts to adjust care intensity.

**StressTestCompleted**: Published when a stress test is finalized with results. Carries patient identifier, test protocol, exercise duration, peak hemodynamic data, ECG findings, imaging results (if applicable), and overall interpretation. Consumed by Diagnostic Imaging (if further imaging indicated) and Interventional Procedures (if ischemia detected).

### Diagnostic Imaging Context (Publisher)

**ImagingStudyCompleted**: Published when an imaging study interpretation is finalized. Carries study accession number, patient identifier, modality, key measurements (ejection fraction, valve gradients, chamber dimensions), and critical findings. Consumed by Clinical Assessment, Heart Failure Management, and Interventional Procedures.

**CriticalImagingFindingDetected**: Published when imaging reveals an urgent finding (e.g., severe aortic stenosis, cardiac tamponade, aortic dissection). Carries patient identifier, finding description, severity, and recommended urgent action. Consumed by Clinical Assessment and Interventional Procedures for immediate clinical response.

**EjectionFractionChanged**: Published when a serial echocardiogram shows a significant change in LVEF. Carries patient identifier, previous LVEF, new LVEF, and delta. Consumed by Heart Failure Management for phenotype reclassification and GDMT adjustment.

### Interventional Procedures Context (Publisher)

**ProcedureCompleted**: Published when a cardiac catheterization or intervention is completed. Carries procedure identifier, patient identifier, procedure type, findings summary, interventions performed, devices implanted, and complications. Consumed by Clinical Assessment, Heart Failure Management, and Cardiac Rehabilitation.

**StentImplanted**: Published when a coronary stent is placed. Carries patient identifier, lesion location, stent specifications, and antiplatelet therapy requirements. Consumed by Clinical Assessment for medication management and Cardiac Rehabilitation for activity restrictions.

**ValveReplaced**: Published when a valve replacement (TAVR or surgical) is completed. Carries patient identifier, valve position, prosthesis type and size, and post-procedural gradient. Consumed by Diagnostic Imaging for follow-up scheduling and Cardiac Rehabilitation for recovery planning.

### Electrophysiology Context (Publisher)

**ArrhythmiaDetected**: Published when a new or recurrent arrhythmia is identified. Carries patient identifier, arrhythmia type, detection method, hemodynamic impact, and recommended management.

**DeviceImplanted**: Published when a pacemaker, ICD, or CRT device is implanted. Carries patient identifier, device type, model, indication, and initial programming. Consumed by Heart Failure Management (for CRT patients) and Cardiac Rehabilitation (for exercise parameter guidance).

**AblationCompleted**: Published when an ablation procedure achieves its endpoint. Carries patient identifier, target arrhythmia, ablation strategy, and acute success determination.

### Heart Failure Management Context (Publisher)

**HeartFailureClassificationChanged**: Published when a patient's HF phenotype, NYHA class, or ACC/AHA stage changes. Consumed by Clinical Assessment and Cardiac Rehabilitation.

**GDMTOptimizationAchieved**: Published when a patient reaches target doses on all four pillars of GDMT. Consumed by Clinical Assessment for care plan documentation.

**MechanicalSupportEscalation**: Published when a patient is referred for VAD or transplant evaluation. Consumed by Interventional Procedures for surgical planning.

### Cardiac Rehabilitation Context (Publisher)

**RehabilitationPhaseCompleted**: Published when a patient completes a rehabilitation phase. Carries patient identifier, completed phase, outcomes achieved, and next phase recommendation.

**ExerciseCapacityImproved**: Published when a patient demonstrates measurable improvement in functional capacity. Consumed by Heart Failure Management for prognostic assessment.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 8 on domain events.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 8 on domain events.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 10 on event-driven architecture.
- ACC/AHA 2021 Chest Pain Guideline for clinical event-driven pathways.
