# Therapy Management Context

## Overview

The Therapy Management Context governs the delivery of therapeutic services
in mental health care. It manages the operational aspects of therapy: session
scheduling, progress note documentation, therapeutic modality implementation,
and tracking of the therapeutic relationship. This bounded context is
classified as a supporting subdomain because it executes well-established
clinical protocols rather than requiring novel domain modeling, yet it is
essential to translating treatment plans into actual patient care.

## Purpose and Scope

This context answers the operational question: how are therapeutic sessions
delivered, documented, and tracked? It encompasses:

- Session scheduling that aligns with treatment plan specifications for
  modality and frequency.
- Session conduct tracking including attendance, duration, and cancellation.
- Progress note documentation using structured clinical formats (SOAP notes,
  DAP notes, or modality-specific templates).
- Therapeutic modality implementation for evidence-based treatments including
  Cognitive Behavioral Therapy (CBT), Dialectical Behavior Therapy (DBT),
  Eye Movement Desensitization and Reprocessing (EMDR), psychodynamic therapy,
  motivational interviewing, and group therapy.
- Therapeutic alliance monitoring using validated measures of the clinician-
  patient relationship.
- No-show and cancellation pattern tracking.

## Ubiquitous Language

Within this context, the following terms have precise meanings:

- Session: a scheduled, time-bounded therapeutic encounter between a patient
  and a mental health provider.
- Progress Note: a clinical document recording what occurred during a session,
  including patient presentation, interventions applied, patient response,
  and the plan for the next session.
- Modality: the specific therapeutic approach used during sessions, as
  specified by the treatment plan.
- Therapeutic Alliance: the quality of the collaborative relationship between
  clinician and patient, measured through validated instruments.
- No-Show: a scheduled session the patient did not attend without advance
  cancellation.

## Aggregate: Session

The Session aggregate is the root of this context. It encapsulates:

- SessionId (unique identity)
- Patient reference (by identity)
- Clinician reference (by identity)
- Treatment plan reference (TreatmentPlanId)
- Scheduled date, time, and duration
- Modality used
- Session status (scheduled, in-progress, completed, cancelled, no-show)
- Progress note (ProgressNote entity)
- Therapeutic interventions applied (TherapeuticIntervention value objects)
- Patient response summary
- Therapeutic alliance rating (if assessed)

Invariants enforced by the aggregate:

- A session must reference an active treatment plan.
- The modality used must be one of the modalities specified in the treatment
  plan.
- A progress note must be completed within the configurable documentation
  window (typically 24-72 hours after the session).
- A completed session must have a signed progress note.
- Session duration must fall within acceptable clinical ranges for the
  modality.

## Domain Events Published

- SessionCompleted: signals that a session has been conducted and documented.
  Consumed by Outcomes Measurement for tracking treatment dose and engagement.
- SessionCancelled: signals a cancelled session with reason and initiator.
  Consumed by Outcomes Measurement for engagement tracking.
- TherapeuticAllianceRated: signals that the therapeutic relationship quality
  has been assessed. Consumed by Outcomes Measurement for alliance monitoring.

## Domain Services

- SessionSchedulingService: coordinates scheduling based on treatment plan
  requirements, provider availability, and patient preferences.
- ProgressTrackingService: analyzes patterns across multiple sessions to
  identify engagement trends and intervention effectiveness.

## Integration Points

- Upstream: consumes TreatmentPlanCreated and TreatmentPlanRevised events
  from the Treatment Planning Context to receive modality directives and
  session frequency requirements.
- Downstream: publishes session events to the Outcomes Measurement Context.
- Lateral: coordinates with the Crisis Intervention Context when a session
  reveals acute risk, potentially triggering a CrisisInitiated event.
- External: may integrate with scheduling systems and telehealth platforms
  through anti-corruption layers.

## Modality-Specific Considerations

Different therapeutic modalities impose different session structures:

- CBT: structured sessions with agenda setting, cognitive restructuring
  exercises, behavioral experiments, and homework assignment.
- DBT: combines individual therapy sessions with skills group sessions;
  the session aggregate must track both formats.
- EMDR: sessions follow the 8-phase protocol with specific interventions
  (bilateral stimulation, cognitive interweave) tracked per phase.
- Group Therapy: sessions involve multiple patients and may have co-
  facilitators; the session aggregate references multiple patient identities.

## Regulatory Considerations

- Progress notes must comply with state-specific documentation requirements.
- Psychotherapy notes receive heightened protection under HIPAA (45 CFR
  164.501) and generally cannot be disclosed without specific patient
  authorization.
- Session records for substance use disorder treatment receive additional
  protections under 42 CFR Part 2.
- Telehealth sessions must comply with state telehealth practice acts and
  cross-state licensure requirements.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- Beck, Judith S. Cognitive Behavior Therapy: Basics and Beyond. Guilford Press, 2020.
- Linehan, Marsha M. DBT Skills Training Manual. Guilford Press, 2014.
- Shapiro, Francine. Eye Movement Desensitization and Reprocessing (EMDR) Therapy. Guilford Press, 2017.
