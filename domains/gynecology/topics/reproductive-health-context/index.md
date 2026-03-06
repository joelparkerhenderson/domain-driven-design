# Reproductive Health Context

## Overview

The Reproductive Health Context is a bounded context within the gynecology domain that manages fertility assessment, contraception planning, menstrual disorder evaluation and treatment, and hormonal health management. It models the ongoing, longitudinal relationship between a patient and her reproductive health goals.

## Scope and Responsibility

This context owns the reproductive health plan model, managing patient reproductive goals over time. Its responsibilities include:

- Conducting and recording fertility assessments including ovarian reserve testing, hormonal profiling, and partner factor evaluation.
- Developing and maintaining personalized contraception plans based on medical eligibility, patient preference, and efficacy data.
- Evaluating and managing menstrual disorders including abnormal uterine bleeding, amenorrhea, dysmenorrhea, and polycystic ovary syndrome.
- Tracking hormonal profiles and interpreting laboratory results in the context of reproductive health.
- Managing treatment protocols for reproductive health conditions.
- Coordinating with the Clinical Assessment Context for diagnostic input and with the Patient Education Context for patient guidance.

## Ubiquitous Language

Key terms within this context:

- Reproductive Health Plan: A longitudinal record of a patient's reproductive goals, current interventions, and health status.
- Fertility Assessment: An evaluation of reproductive potential incorporating ovarian reserve, ovulatory function, and contributing factors.
- Contraception Plan: A documented strategy for pregnancy prevention, including method selection, initiation, and follow-up.
- Menstrual Disorder: A clinically significant deviation from normal menstrual patterns requiring evaluation and management.
- Ovulatory Cycle: The physiological process of follicular development, ovulation, and luteal phase.
- Hormonal Profile: A set of reproductive hormone measurements used to assess endocrine function.
- Treatment Protocol: A structured intervention plan for managing a reproductive health condition.

## Aggregate: Reproductive Health Plan

The Reproductive Health Plan is the primary aggregate root. It contains:

- FertilityAssessment value objects capturing evaluation results and recommendations.
- ContraceptionPlan value objects recording the current contraceptive strategy.
- MenstrualHistory value objects tracking cycle patterns over time.
- HormonalProfile value objects capturing laboratory measurements.
- TreatmentProtocol entities managing active treatment plans.

Key invariants:

- A contraception plan must not include methods that are medically contraindicated for the patient (WHO MEC Category 4).
- Fertility assessment results have a defined validity period and must be flagged as expired when that period elapses.
- Treatment protocol changes must be documented with clinical rationale.
- A reproductive health plan must always reflect the patient's current stated reproductive goals.

## Domain Events Published

- **ReproductiveHealthPlanUpdated**: Published when the plan is materially changed (goal change, method change, treatment change). Consumed by Clinical Assessment and Patient Education contexts.
- **FertilityAssessmentCompleted**: Published when a fertility evaluation is finalized. Consumed by Patient Education Context.
- **ContraceptionMethodChanged**: Published when a patient transitions to a new contraceptive method. Consumed by Patient Education Context.
- **MenstrualDisorderDiagnosed**: Published when a menstrual disorder is formally diagnosed. Consumed by Patient Education Context.

## Domain Events Consumed

- **DiagnosticImpressionFormed** (from Clinical Assessment): Triggers updates to the reproductive health plan when diagnostic findings are relevant to reproductive health.
- **EducationSessionCompleted** (from Patient Education): Records that the patient has received education relevant to their reproductive health decisions.

## Domain Services

- **ContraceptionEligibilityService**: Evaluates medical eligibility for all contraceptive methods using WHO Medical Eligibility Criteria.
- **FertilityEvaluationService**: Integrates multiple data sources to produce a composite fertility assessment with recommendations.

## Value Objects

- MenstrualCycleSummary, HormonalLevel, ContraceptiveMethod, FertilityScore, OvarianReserve.

## Integration Points

- Downstream from Clinical Assessment Context via DiagnosticImpressionFormed events.
- Upstream to Patient Education Context via plan updates and assessment completion events.
- Shared kernel with Clinical Assessment for patient demographic and basic clinical history value objects.

## Clinical Standards Applied

- WHO Medical Eligibility Criteria for Contraceptive Use (MEC), 5th edition.
- ACOG Practice Bulletins on abnormal uterine bleeding, polycystic ovary syndrome, and infertility evaluation.
- ASRM (American Society for Reproductive Medicine) guidelines on fertility assessment.

## Subdomain Classification

Reproductive Health is a core subdomain. It represents high-value, ongoing patient relationships centered on individualized care that distinguishes the gynecology practice.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- WHO. Medical Eligibility Criteria for Contraceptive Use, 5th edition, 2015.
- ACOG. Practice Bulletin No. 128: Diagnosis of Abnormal Uterine Bleeding in Reproductive-Aged Women, 2012.
