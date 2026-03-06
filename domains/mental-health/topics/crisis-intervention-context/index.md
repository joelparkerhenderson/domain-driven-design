# Crisis Intervention Context

## Overview

The Crisis Intervention Context addresses acute psychiatric emergencies. It
owns the workflows for risk assessment, safety planning, crisis hotline
response, and coordination with emergency services. This bounded context is
classified as a supporting subdomain because crisis protocols follow
established best practices, but its operational requirements are uniquely
demanding: speed of response, high availability, and strict protocol adherence
are paramount when patient safety is at immediate risk.

## Purpose and Scope

This context answers the urgent clinical question: is this patient at
immediate risk, and what actions must be taken right now? It encompasses:

- Structured risk assessment using validated instruments and clinical
  judgment to determine the level of immediate danger.
- Safety plan creation following evidence-based frameworks that provide
  patients with a personalized set of coping strategies and resources.
- Crisis hotline protocols for telephone and text-based crisis contacts.
- Emergency department coordination when a patient requires higher-level
  crisis care.
- Involuntary commitment evaluation when a patient meets legal criteria
  for danger to self or others.
- Post-crisis follow-up to ensure continuity of care after a crisis episode
  resolves.

## Ubiquitous Language

Within this context, the following terms have precise meanings:

- Crisis Episode: a discrete event during which a patient experiences an
  acute psychiatric emergency requiring immediate intervention.
- Risk Assessment: a systematic evaluation of a patient's potential for
  harm to self or others, considering risk factors, protective factors,
  and warning signs.
- Safety Plan: a prioritized list of coping strategies and resources a
  patient can use during a crisis, following the Stanley-Brown model.
- Risk Level: a structured determination of danger (low, moderate, high,
  imminent) based on assessed factors.
- Disposition: the clinical decision about the patient's placement after
  crisis stabilization (e.g., return to outpatient, partial hospitalization,
  emergency department, inpatient admission).
- Means Restriction: the process of reducing a patient's access to
  methods of self-harm.

## Aggregate: CrisisEpisode

The CrisisEpisode aggregate is the root of this context. It encapsulates:

- CrisisEpisodeId (unique identity)
- Patient reference (by identity)
- Initiating event (self-referral, clinician-identified, hotline call,
  third-party report)
- Responding clinician reference
- Risk assessment (RiskLevel, RiskFactor, ProtectiveFactor value objects)
- Safety plan reference (SafetyPlan entity, if created or updated)
- Interventions performed
- Disposition decision with clinical justification
- Follow-up plan (type, timeline, responsible clinician)
- Status (active, stabilized, resolved, transferred)
- Timeline of events during the episode

Invariants enforced by the aggregate:

- Every crisis episode must have a documented risk assessment.
- Episodes assessed as high or imminent risk must have a safety plan.
- Disposition must be documented and clinically justified before an episode
  can be resolved.
- Follow-up must be scheduled before resolution.
- Means restriction must be assessed for all episodes involving suicidal
  ideation.

## Domain Events Published

- CrisisInitiated: a high-priority event signaling that a patient has
  entered a crisis state. All other contexts must be prepared to respond.
  Carries patient identifier, crisis type, initial risk level, and
  responding clinician.
- SafetyPlanCreated: signals creation or major revision of a safety plan.
  Consumed by the Treatment Planning Context for plan integration.
- CrisisResolved: signals that the crisis episode has reached a stable
  disposition. Carries resolution type and follow-up plan. Triggers
  downstream workflow resumption.
- InvoluntaryHoldInitiated: signals that the patient meets criteria for
  involuntary psychiatric hold. Triggers legal documentation workflows.

## Domain Services

- RiskCalculationService: computes structured risk levels by weighing risk
  factors, protective factors, and clinical presentation.
- SafetyPlanningService: guides safety plan creation and validates
  completeness against evidence-based requirements.

## Integration Points

- Lateral: publishes high-priority events to all other bounded contexts.
  CrisisInitiated may cause Therapy Management to cancel upcoming sessions,
  Medication Management to flag urgent medication review, and Treatment
  Planning to escalate care level.
- External: integrates with the 988 Suicide & Crisis Lifeline, emergency
  departments, and inpatient psychiatric facilities through anti-corruption
  layers.
- Upstream: consumes patient clinical data from other contexts to inform
  risk assessment (current medications, recent assessment scores, treatment
  history).

## Clinical Standards

This context implements:

- Columbia Suicide Severity Rating Scale (C-SSRS) for suicidal ideation
  and behavior assessment (Posner et al., 2011).
- Stanley-Brown Safety Planning Intervention model (Stanley & Brown, 2012).
- Joint Commission National Patient Safety Goals for behavioral health.
- Suicide Prevention Resource Center best practices.
- Zero Suicide framework principles (Covington et al., 2011).

## Regulatory Considerations

- Involuntary commitment laws vary by state and must be modeled according
  to the applicable jurisdiction's statutes.
- Crisis documentation must meet legal evidentiary standards.
- Duty to warn (Tarasoff obligations) may apply when a patient poses a
  threat to identifiable third parties.
- Emergency holds have specific time limits and judicial review requirements
  that vary by jurisdiction.
- EMTALA (Emergency Medical Treatment and Labor Act) applies when crisis
  patients present to emergency departments.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- Stanley, Barbara and Brown, Gregory K. "Safety Planning Intervention: A Brief Intervention to Mitigate Suicide Risk." Cognitive and Behavioral Practice, 2012.
- Posner, K. et al. "The Columbia-Suicide Severity Rating Scale." American Journal of Psychiatry, 2011.
