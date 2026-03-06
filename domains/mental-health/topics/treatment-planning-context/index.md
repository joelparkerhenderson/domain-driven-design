# Treatment Planning Context

## Overview

The Treatment Planning Context is responsible for creating, managing, and
revising individualized treatment plans that translate diagnostic findings
into actionable clinical strategies. This bounded context owns the treatment
plan lifecycle from initial draft through discharge, including goal setting,
modality selection, care level determination, and scheduled plan reviews.
It is classified as a core subdomain because individualized, evidence-based
treatment planning is the foundation of effective mental health care.

## Purpose and Scope

This context answers the clinical question: given the patient's diagnoses and
circumstances, what is the best course of treatment? It encompasses:

- Treatment plan creation that links diagnoses to specific, measurable goals.
- Collaborative goal setting between clinicians and patients, following SMART
  criteria (Specific, Measurable, Achievable, Relevant, Time-bound).
- Modality selection based on diagnosis, severity, patient preference, and
  evidence-based practice guidelines.
- Care level determination using structured criteria that match symptom
  severity and functional impairment to the appropriate intensity of services.
- Scheduled plan reviews that assess progress toward goals and adjust the
  plan as clinical understanding deepens.
- Plan transitions including revision, care escalation, care de-escalation,
  and discharge planning.

## Ubiquitous Language

Within this context, the following terms have precise meanings:

- Treatment Plan: a comprehensive document specifying diagnoses, goals,
  modalities, care level, responsible providers, and review dates.
- Treatment Goal: a specific, measurable objective that defines what
  successful treatment will accomplish for the patient.
- Modality: a type of therapeutic intervention selected for the patient
  (e.g., CBT, DBT, EMDR, psychodynamic therapy, group therapy).
- Care Level: the intensity of services (outpatient, intensive outpatient,
  partial hospitalization, residential, inpatient).
- Plan Review: a scheduled clinical evaluation of treatment plan effectiveness
  and appropriateness.

## Aggregate: TreatmentPlan

The TreatmentPlan aggregate is the root of this context. It encapsulates:

- TreatmentPlanId (unique identity)
- Patient reference (by identity)
- Linked diagnostic codes from the Clinical Assessment Context
- Treatment goals (TreatmentGoal entities)
- Selected modalities (ModalitySelection value objects)
- Care level (CareLevel value object)
- Responsible clinician assignments
- Review schedule
- Status (draft, active, under-review, revised, discharged)
- Version history

Invariants enforced by the aggregate:

- At least one active treatment goal is required.
- Every goal must link to at least one diagnosis.
- The care level must be clinically justified with documented criteria.
- A review date must be set and cannot exceed regulatory maximums.
- Only one treatment plan per patient may be active at a time.

## Domain Events Published

- TreatmentPlanCreated: signals that a new plan has been approved and
  activated. Consumed by Therapy Management and Medication Management
  to initiate downstream workflows.
- TreatmentPlanRevised: signals that an existing plan has been updated.
  Downstream contexts adjust their workflows accordingly.
- CareEscalated: signals an increase in care level intensity. May trigger
  facility transfer coordination and insurance authorization.
- GoalAchieved: signals that a treatment goal has been marked as achieved.
  Consumed by Outcomes Measurement for effectiveness tracking.

## Domain Services

- ModalityRecommendationService: recommends therapeutic modalities based on
  diagnoses, severity, and evidence-based guidelines.
- CareLevelDeterminationService: determines the appropriate care level using
  structured criteria.

## Integration Points

- Upstream: consumes AssessmentCompleted events from the Clinical Assessment
  Context to obtain diagnostic information for plan creation.
- Downstream: publishes treatment directives to the Therapy Management Context
  (modalities, frequency) and the Medication Management Context
  (pharmacotherapy recommendations) as a customer-supplier relationship.
- External: interfaces with insurance/payer systems for prior authorization
  through an anti-corruption layer. Communicates with utilization review
  systems for care level justification.

## Clinical Standards

This context implements:

- APA Practice Guidelines for treatment of specific disorders.
- NICE Clinical Guidelines for evidence-based modality selection.
- LOCUS (Level of Care Utilization System) for care level determination.
- Joint Commission standards for treatment plan documentation.
- Recovery-oriented care principles emphasizing patient participation in
  goal setting.

## Regulatory Considerations

- Treatment plans must be reviewed at intervals specified by state mental
  health regulations (typically 90 days for outpatient, 30 days for
  inpatient).
- Informed consent for treatment modalities must be documented.
- Treatment plans for minors require guardian involvement per state law.
- Plans involving restrictive interventions must comply with CMS Conditions
  of Participation.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- American Psychiatric Association. Practice Guidelines. APA Publishing, various years.
- American Association of Community Psychiatrists. LOCUS. AACP, 2010.
