# Domain Services in the Gynecology Domain

## Overview

A domain service is a stateless operation that encapsulates domain logic which does not naturally belong to any single entity or value object. Domain services coordinate actions across multiple aggregates or perform computations that require information from several domain objects. In the gynecology domain, domain services handle clinical decision support, risk calculations, and cross-aggregate coordination.

## Purpose

Some gynecological business logic spans multiple aggregates or bounded contexts. Calculating a patient's overall risk profile may require data from the prenatal record, clinical history, and screening results. Determining the appropriate screening interval requires knowledge of the patient's age, prior results, and risk factors. These operations belong in domain services rather than being forced into a single entity or aggregate.

## Domain Services by Bounded Context

### Clinical Assessment Context

**DifferentialDiagnosisService**
- Accepts symptom evaluations, physical findings, and diagnostic workup results.
- Produces a ranked list of candidate diagnoses based on clinical decision rules.
- Applies evidence-based diagnostic algorithms specific to gynecological conditions.
- Does not own any state; it operates on data provided by the Clinical Encounter aggregate.

**ReferralRoutingService**
- Determines the appropriate referral destination based on diagnostic impressions.
- Evaluates referral criteria for surgical consultation, reproductive health specialist referral, prenatal care initiation, or screening program enrollment.
- Produces referral recommendations with urgency classifications.

### Reproductive Health Context

**ContraceptionEligibilityService**
- Evaluates a patient's medical eligibility for contraceptive methods based on WHO Medical Eligibility Criteria (MEC).
- Accepts patient medical history, current medications, and contraindications.
- Returns a categorized list of contraceptive methods with eligibility classifications (Category 1 through 4).

**FertilityEvaluationService**
- Integrates ovarian reserve data, hormonal profiles, and clinical history to produce a composite fertility assessment.
- Applies age-adjusted interpretation algorithms to laboratory values.
- Produces structured fertility assessment reports with recommended next steps.

### Surgical Services Context

**SurgicalRiskAssessmentService**
- Calculates perioperative risk scores based on patient comorbidities, surgical procedure complexity, and anesthesia classification.
- Applies validated risk assessment tools adapted for gynecological procedures.
- Produces risk stratification outputs that inform surgical planning and consent discussions.

**ProcedureSelectionService**
- Evaluates clinical indications, patient anatomy, surgical history, and patient preferences to recommend surgical approaches.
- Compares minimally invasive versus open surgical options based on evidence-based criteria.
- Produces procedure recommendation reports for shared decision-making.

### Prenatal Care Context

**RiskStratificationService**
- Evaluates maternal risk factors including age, medical history, obstetric history, current pregnancy findings, and social determinants.
- Applies validated risk scoring systems to classify pregnancy as low-risk, moderate-risk, or high-risk.
- Recalculates risk level when new clinical data becomes available.
- Triggers HighRiskIdentified domain events when risk level escalates.

**GestationalAgeDateService**
- Calculates gestational age from last menstrual period or ultrasound dating.
- Reconciles conflicting dating information using clinical decision rules.
- Determines estimated due date and current trimester.

### Screening Programs Context

**ScreeningScheduleService**
- Determines appropriate screening intervals based on patient age, risk factors, prior results, and clinical guideline recommendations.
- Applies ACOG, USPSTF, and ACS screening guidelines.
- Produces personalized screening schedules with due dates and test types.

**ResultInterpretationService**
- Applies clinical classification systems to raw screening results.
- Classifies Pap smear results using the Bethesda System.
- Determines appropriate follow-up pathways for abnormal results based on ASCCP management guidelines.

### Patient Education Context

**ContentMatchingService**
- Matches clinical events and patient characteristics to appropriate educational content.
- Considers health literacy level, language preference, and clinical context when selecting content.
- Produces content recommendations ranked by relevance and appropriateness.

**SharedDecisionSupportService**
- Structures the shared decision-making process by presenting treatment options with evidence-based outcome data.
- Formats clinical information at the patient's assessed literacy level.
- Documents the decision-making process and the patient's stated preferences.

## Domain Service Design Principles

1. Domain services are stateless. They do not maintain any internal state between invocations.
2. Domain services operate on domain objects (entities, value objects, aggregates) passed as parameters.
3. Domain services are named using verbs or verb phrases that describe the operation (CalculateRisk, DetermineEligibility, RouteReferral).
4. Domain services belong to the domain layer, not the application or infrastructure layer.
5. Domain services encapsulate complex business rules that span multiple domain objects.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 5 on domain services.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 7 on domain service design.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 6 on domain services.
- WHO. Medical Eligibility Criteria for Contraceptive Use, 5th edition, 2015.
