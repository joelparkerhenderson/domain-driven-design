# Clinical Assessment Context

## Overview

The Clinical Assessment Context is a bounded context within the gynecology domain that encompasses gynecological examination, symptom evaluation, clinical history taking, and diagnostic workup. It is the entry point for most patient interactions and produces the diagnostic impressions that drive downstream clinical decisions across other bounded contexts.

## Scope and Responsibility

This context owns the clinical encounter model from patient intake through diagnostic conclusion. Its responsibilities include:

- Recording the presenting complaint and relevant history.
- Conducting and documenting the gynecological examination, including inspection, palpation, and specimen collection.
- Evaluating symptoms systematically using structured assessment tools.
- Ordering and tracking diagnostic tests (laboratory, imaging, pathology).
- Formulating differential diagnoses and producing diagnostic impressions.
- Generating referrals to other bounded contexts when clinical findings warrant specialized care.

## Ubiquitous Language

Key terms within this context:

- Clinical Encounter: A single patient visit involving history, examination, and assessment.
- Presenting Complaint: The primary reason the patient seeks care, expressed in clinical terms.
- Symptom Evaluation: Structured assessment of symptoms including onset, duration, severity, and associated factors.
- Physical Findings: Objective observations documented during the gynecological examination.
- Diagnostic Workup: The ordered set of tests and procedures performed to establish a diagnosis.
- Diagnostic Impression: The clinician's concluded assessment of the patient's condition.
- Referral Recommendation: A formal suggestion to transfer care to another context or specialist.

## Aggregate: Clinical Encounter

The Clinical Encounter is the primary aggregate root. It contains:

- SymptomEvaluation value objects capturing each symptom with structured attributes.
- PhysicalFindings value objects recording examination observations.
- DiagnosticWorkup entity tracking ordered tests and their results.
- DiagnosticImpression value object capturing the final clinical assessment.

Key invariants:

- A diagnostic impression cannot be finalized until at least one evaluation (symptom or physical) is complete.
- A diagnostic workup must be linked to the presenting complaint or examination findings that prompted it.
- Referrals can only be generated after a diagnostic impression is formed.

## Domain Events Published

- **DiagnosticImpressionFormed**: Published when the clinician finalizes their diagnostic assessment. Consumed by Reproductive Health, Surgical Services, Screening Programs, and Patient Education contexts.
- **ClinicalReferralCreated**: Published when the encounter results in a referral. Consumed by the target context (Surgical Services, Prenatal Care, or Screening Programs).
- **FollowUpEncounterScheduled**: Published when a follow-up visit is required. Consumed internally and by Patient Education.

## Domain Events Consumed

- **AbnormalResultDetected** (from Screening Programs): Triggers creation of a new clinical encounter for follow-up assessment.
- **SurgeryCompleted** (from Surgical Services): Triggers scheduling of a postoperative follow-up encounter.
- **HighRiskIdentified** (from Prenatal Care): Triggers scheduling of a specialist consultation encounter.

## Domain Services

- **DifferentialDiagnosisService**: Applies clinical reasoning algorithms to generate ranked differential diagnoses from symptom evaluations and examination findings.
- **ReferralRoutingService**: Determines the appropriate downstream context and urgency level for clinical referrals.

## Value Objects

- BloodPressure, BodyMassIndex, PainScore, SymptomDescription, DiagnosisCode, PhysicalFindings.

## Integration Points

- Upstream to Reproductive Health Context via DiagnosticImpressionFormed events.
- Upstream to Surgical Services Context via ClinicalReferralCreated events.
- Downstream from Screening Programs Context via AbnormalResultDetected events.
- Downstream from Surgical Services Context via SurgeryCompleted events.
- Downstream from Prenatal Care Context via HighRiskIdentified events.

## Clinical Standards Applied

- ACOG guidelines for gynecological examination best practices.
- ICD-10 coding standards for diagnostic classification.
- Clinical decision support algorithms based on evidence-based gynecological assessment protocols.

## Subdomain Classification

Clinical Assessment is a core subdomain. It represents the foundational clinical activity of the gynecology practice and requires deep domain expertise in both the modeling and the clinical reasoning it encodes.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- American College of Obstetricians and Gynecologists. Guidelines for Women's Health Care, 4th edition.
