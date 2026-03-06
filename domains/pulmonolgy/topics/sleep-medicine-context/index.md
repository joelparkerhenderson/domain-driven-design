# Sleep Medicine Context

## Overview

The Sleep Medicine Context manages the diagnosis and treatment of sleep-related breathing disorders within the pulmonolgy domain. It encompasses polysomnography (PSG), home sleep testing, obstructive sleep apnea (OSA) management, CPAP/BiPAP therapy optimization, and narcolepsy evaluation. Sleep medicine operates as a distinct subspecialty within pulmonology with its own terminology, scoring criteria, and therapy workflows.

As a supporting subdomain, the Sleep Medicine Context provides essential clinical capability but follows standardized protocols defined by the American Academy of Sleep Medicine (AASM). The domain model captures the complexity of sleep study interpretation, therapy titration, and long-term compliance management while recognizing that these workflows are broadly standardized across sleep medicine practices.

## Clinical Scope

The Sleep Medicine Context covers the following clinical activities:

- **Sleep Study Management**: Ordering, scheduling, and managing polysomnography (in-lab attended studies) and home sleep apnea testing (HSAT). Includes study type selection based on clinical indication and pre-authorization requirements.

- **Sleep Study Scoring and Interpretation**: Applying AASM scoring criteria for sleep stages, respiratory events (apneas, hypopneas, respiratory effort-related arousals), periodic limb movements, and cardiac events. Calculating summary indices including AHI, oxygen desaturation index, and arousal index.

- **OSA Diagnosis and Classification**: Classifying OSA severity based on AHI thresholds (mild 5-14, moderate 15-29, severe 30 or greater), symptom burden, and cardiovascular risk assessment.

- **Positive Airway Pressure Therapy**: Prescribing CPAP or BiPAP therapy, managing in-lab titration studies, optimizing auto-titrating device settings, and selecting appropriate mask interfaces.

- **Therapy Compliance Management**: Monitoring CPAP/BiPAP adherence through device data downloads, identifying compliance barriers, implementing targeted interventions, and reporting compliance for insurance requirements.

- **Narcolepsy Evaluation**: Multiple sleep latency testing (MSLT), maintenance of wakefulness testing (MWT), and narcolepsy classification (type 1 with cataplexy, type 2 without cataplexy).

## Aggregates

**Sleep Study Aggregate**: Manages the lifecycle of a sleep study from order through interpretation. Root entity is SleepStudy, containing SleepStageScoring records, RespiratoryEventScoring value objects, and OxygenDesaturation events. Enforces the invariant that scoring follows AASM criteria and that AHI calculation is consistent with scored events.

**Therapy Management Aggregate**: Manages positive airway pressure therapy from prescription through ongoing compliance monitoring. Root entity is TherapyManagement, containing DevicePrescription entities, PressureTitration results, and ComplianceRecord value objects. Enforces the invariant that therapy prescriptions are linked to a diagnostic sleep study.

## Key Entities

- **Sleep Study Order**: Tracks the request lifecycle from clinical indication through scheduling and insurance authorization.
- **Therapy Prescription**: Manages device type, pressure settings, mask type, and prescription validity period.
- **Sleep Patient**: The patient projection within this context, carrying sleep-specific attributes such as Epworth Sleepiness Scale history and compliance patterns.

## Key Value Objects

- **AHIScore**: Total and component AHI values with severity classification.
- **SleepStageDistribution**: Percentage of total sleep time in each sleep stage.
- **OxygenDesaturationIndex**: Desaturation events per hour with threshold specification.
- **CPAPComplianceRecord**: Usage statistics for a defined compliance period.

## Domain Events Published

- **SleepStudyScored**: Signals completed scoring with summary indices and diagnostic classification.
- **OSADiagnosisConfirmed**: Signals confirmed OSA diagnosis with severity and therapy recommendation.
- **TherapyComplianceReported**: Signals periodic compliance data with adherence status.
- **TherapySettingsOptimized**: Signals pressure or device setting adjustments.

## Domain Services

- **SleepStudyInterpretationService**: Applies AASM criteria to produce clinical interpretations from scored data.
- **TherapyTitrationService**: Determines optimal pressure settings from titration data and auto-CPAP downloads.
- **ComplianceInterventionService**: Identifies compliance barriers and recommends targeted interventions.

## Context Relationships

The Sleep Medicine Context has a shared kernel relationship with the Pulmonary Assessment Context. Both contexts share respiratory symptom evaluation instruments, including the Epworth Sleepiness Scale and daytime symptom assessments. Changes to this shared kernel require agreement from both sleep medicine and assessment teams.

The context uses an anti-corruption layer when integrating with external sleep lab data systems. Vendor-specific polysomnography formats, CPAP device data schemas, and scoring output structures are translated into the domain's internal model. This ACL absorbs all vendor variability, enabling the Sleep Medicine Context to remain vendor-independent.

The context may generate events consumed by the Chronic Disease Management Context when sleep-disordered breathing is identified as a comorbidity affecting chronic disease management (such as OSA complicating COPD in overlap syndrome).

## Business Rules

- Sleep study type selection must follow AASM clinical guidelines: in-lab PSG for patients with significant comorbidities, HSAT for uncomplicated suspected OSA.
- AASM scoring rules must be applied consistently. Apneas require at least 90% airflow reduction for 10 or more seconds. Hypopneas require at least 30% airflow reduction with 3% or greater desaturation or arousal.
- AHI severity classification follows standard thresholds: normal (less than 5), mild (5-14), moderate (15-29), severe (30 or greater).
- Therapy compliance is measured using the Medicare standard: at least 4 hours of use on at least 70% of nights during a consecutive 30-day period within the first 90 days.
- Therapy prescriptions must reference a qualifying diagnostic study. Therapy cannot be prescribed without documented diagnostic basis.
- Auto-CPAP data downloads must be reviewed at defined intervals during the initial therapy period to optimize settings and identify mask leak or residual event issues.
- Narcolepsy diagnosis requires MSLT showing mean sleep latency of 8 minutes or less with 2 or more sleep-onset REM periods.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- American Academy of Sleep Medicine. International Classification of Sleep Disorders, 3rd Edition (ICSD-3), 2014.
- Berry, R.B., et al. AASM Manual for the Scoring of Sleep and Associated Events, Version 2.6. American Academy of Sleep Medicine, 2020.
