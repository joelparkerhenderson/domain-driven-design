# Clinical Assessment Context

## Overview

The Clinical Assessment Context is responsible for the structured evaluation of a patient's respiratory status. It encompasses respiratory examination workflows, history taking, symptom scoring using standardized instruments, and risk stratification for treatment planning. This bounded context is classified as a core subdomain because it encodes the specialized clinical reasoning that drives all downstream decisions in the respirology domain.

The Clinical Assessment Context is the entry point for most patient interactions in the respirology system. Assessment results flow downstream to the Respiratory Diagnostics Context (informing test selection), the Chronic Disease Management Context (updating care plans), and the Pulmonary Rehabilitation Context (guiding program design).

## Ubiquitous Language

Key terms within this context include: respiratory examination, auscultation, percussion, inspection, palpation, history taking, chief complaint, symptom score, mMRC dyspnea grade, CAT score, Borg dyspnea scale, risk stratification, clinical encounter, vital signs, smoking history, occupational exposure, and respiratory review of systems.

## Aggregate: Respiratory Assessment

The Respiratory Assessment is the aggregate root of this context. It represents a single clinical encounter during which a patient's respiratory status is evaluated.

**Components**:
- Assessment ID (unique identifier)
- Patient ID (reference to external patient identity)
- Encounter date and time
- Assessing clinician ID
- Chief complaint and history of present illness
- Respiratory history (smoking history, occupational exposures, prior diagnoses)
- Physical examination findings (inspection, palpation, percussion, auscultation findings)
- Vital signs (respiratory rate, heart rate, blood pressure, SpO2, temperature)
- Symptom scores (mMRC, CAT, Borg as applicable)
- Risk stratification result
- Assessment status (in-progress, completed, amended)

**Invariants**:
- A completed assessment must include at least one symptom score and vital signs.
- Risk stratification must be consistent with recorded symptom scores and examination findings.
- An assessment cannot be completed without auscultation findings recorded for all lung fields.
- An amended assessment must record the reason for amendment and the amending clinician.

## Entities

**Assessment Encounter**: Tracks the metadata of the clinical encounter including location, duration, encounter type (initial, follow-up, urgent), and associated clinicians. The encounter entity provides the temporal and administrative context for the assessment.

**Symptom History Entry**: Represents a discrete entry in the patient's symptom timeline, recording the symptom type, onset date, duration, severity, frequency, alleviating and aggravating factors, and associated triggers. Multiple entries build the longitudinal symptom picture.

## Value Objects

**SymptomScore**: Encapsulates a standardized score with the instrument type (mMRC, CAT, Borg), the numeric value, and the assessment timestamp. Self-validates against the instrument's valid range.

**AuscultationFinding**: Captures a single finding including lung zone, sound type (normal vesicular, wheeze, crackle, rhonchi, stridor, diminished, absent), phase (inspiratory, expiratory, both), and laterality (unilateral, bilateral).

**VitalSigns**: Captures respiratory rate, heart rate, systolic and diastolic blood pressure, SpO2 percentage, and temperature. Validates physiological ranges.

**SmokingHistory**: Captures smoking status, pack-years, age at initiation, and cessation date if applicable.

**RiskLevel**: Represents the computed risk stratum (low, moderate, high, very high) with the contributing factors and calculation date.

## Domain Events

**AssessmentCompleted**: Published when a clinician finalizes a respiratory assessment. Carries the assessment summary including patient ID, key scores, examination findings summary, and risk level. Consumed by Respiratory Diagnostics, Chronic Disease Management, and Pulmonary Rehabilitation contexts.

**RiskStratificationUpdated**: Published when a patient's risk level changes from its previous value. Carries the patient ID, old and new risk levels, and the driving factors.

**SymptomDeteriorationIdentified**: Published when scoring reveals clinically significant worsening compared to prior assessments. Carries the patient ID, affected scores, baseline versus current values, and clinical urgency classification.

## Domain Services

**RiskStratificationService**: Computes risk level by integrating symptom scores, vital sign trends, exacerbation history, comorbidity burden, and current examination findings. Implements risk stratification algorithms derived from clinical guidelines.

**SymptomTrendAnalysisService**: Analyzes sequential symptom scores to detect trends in symptom burden over time, identifying patients whose conditions are deteriorating, improving, or remaining stable.

## Integration Points

- **Upstream**: Receives patient demographic and admission data from the hospital ADT system through an anti-corruption layer.
- **Downstream**: Publishes assessment events consumed by Respiratory Diagnostics, Chronic Disease Management, and Pulmonary Rehabilitation contexts.
- **External**: Receives historical clinical data from electronic health record systems through an anti-corruption layer.

## Clinical Workflow

1. A clinician initiates a new Respiratory Assessment for a patient encounter.
2. The clinician records the chief complaint and respiratory history.
3. Physical examination findings are documented, including auscultation of all lung fields.
4. Vital signs are recorded.
5. Standardized symptom instruments are administered and scores recorded.
6. The risk stratification service computes the patient's risk level.
7. The clinician reviews and finalizes the assessment, triggering the AssessmentCompleted event.

## References

- Evans, E. (2003). *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media.
- Bickley, L. S. (2020). *Bates' Guide to Physical Examination and History Taking*. Wolters Kluwer.
- Global Initiative for Chronic Obstructive Lung Disease (GOLD). (2024). *Global Strategy for COPD*.
