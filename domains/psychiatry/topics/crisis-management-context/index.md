# Crisis Management Context

## Overview

The Crisis Management Context is a supporting bounded context in the psychiatry domain responsible for managing psychiatric emergencies. This context owns the workflows for suicide risk assessment, involuntary hold initiation and management, safety planning, de-escalation interventions, and crisis disposition. It maintains a shared kernel relationship with the Diagnostic Assessment and Medication Management contexts to ensure real-time access to critical clinical information during emergencies.

## Responsibility

This context answers the urgent clinical question: "Is this patient in immediate danger, and what intervention is required right now?" It encompasses all activities from crisis identification through acute intervention, stabilization, and disposition. The context operates under compressed timeframes with heightened clinical and legal stakes. Unlike other contexts that operate on clinical schedules, crisis management is event-driven and time-critical.

## Ubiquitous Language

Within this context, the following terms have specific meanings:

- **Crisis Event**: An acute psychiatric situation in which a patient presents an immediate risk of harm to self or others, requiring urgent intervention. This is not a general difficulty or stressor; it is a clinical emergency.
- **Suicide Risk Assessment**: A structured evaluation using validated instruments (Columbia Protocol) to determine the likelihood, imminence, and severity of suicidal behavior.
- **Involuntary Hold**: A legal mechanism for temporary psychiatric detention of an individual who, due to a mental disorder, poses a danger to self or others or is gravely disabled. Specific statutes vary by jurisdiction (5150, Baker Act, Section 12).
- **Safety Plan**: A prioritized, written document created collaboratively with the patient listing coping strategies, contacts, and means restriction steps for use during suicidal crises.
- **De-escalation**: Specific verbal and environmental techniques used to reduce acute agitation and prevent escalation to violence, following a defined protocol sequence.
- **Crisis Disposition**: The clinical decision about next steps after acute crisis stabilization (voluntary admission, involuntary admission, discharge with safety plan, transfer).

## Aggregates

### Crisis Event (Root Aggregate)

The central aggregate representing a single psychiatric emergency from onset to resolution.

Components:
- Activation timestamp and activating circumstances
- Presenting situation description and immediate risk factors
- Risk assessment results (C-SSRS scores, agitation assessment)
- Current diagnoses (via shared kernel with Diagnostic Assessment)
- Current medications (via shared kernel with Medication Management)
- Interventions performed (verbal de-escalation, medication administration, restraint, seclusion)
- Intervention timestamps and outcomes
- Disposition decision and justification
- Resolution timestamp
- Follow-up plan with responsible clinician and timeline
- Crisis event status (active, stabilizing, resolved, follow-up pending)

Invariants:
- A crisis event cannot be resolved without a documented disposition.
- If suicide risk is assessed as imminent, the disposition must be either involuntary hold or documented clinical justification for an alternative action.
- All restraint or seclusion episodes must be documented with start time, end time, and clinical justification.
- Follow-up must be scheduled within 24 hours of crisis resolution for all patients discharged with a safety plan.

### Safety Plan (Root Aggregate)

A patient's documented crisis coping plan, maintained as a living document.

Components:
- Warning signs that a crisis may be developing
- Internal coping strategies the patient can use independently
- People and social settings that provide distraction
- Family members or friends to contact for help
- Mental health professionals and agencies to contact
- Emergency services contact information
- Steps for means restriction (removing access to lethal means)
- Reasons for living identified by the patient
- Plan creation date, last review date, responsible clinician

Invariants:
- A safety plan must include at least one professional or emergency contact.
- Means restriction steps must be documented if the patient has identified access to lethal means.
- The safety plan must be reviewed and updated at each crisis event and at minimum every three months for high-risk patients.

## Value Objects

- **Risk Level**: Risk category (imminent, acute, chronic, low), assessment basis, time horizon.
- **C-SSRS Score**: Ideation severity, ideation intensity, behavior type, lethality assessment.
- **Involuntary Hold Type**: Hold statute, jurisdiction, maximum duration, required criteria.
- **De-escalation Intervention**: Intervention type (verbal, environmental, pharmacological), description, outcome.
- **Crisis Disposition**: Disposition type, receiving facility (if transfer), follow-up requirements.
- **Agitation Level**: Assessment scale score, behavioral descriptors, intervention threshold.

## Domain Events

- **Crisis Activated**: Signals that a psychiatric emergency has been identified and response initiated.
- **Risk Assessment Completed**: Signals completion of a structured suicide risk assessment with results.
- **Involuntary Hold Initiated**: Signals that a legal hold has been placed, triggers state reporting.
- **De-escalation Attempted**: Signals that a de-escalation intervention was performed with its outcome.
- **Crisis Resolved**: Signals that the acute crisis has been stabilized with a documented disposition.
- **Safety Plan Created**: Signals creation of a new safety plan for a patient.
- **Safety Plan Updated**: Signals modification of an existing safety plan.

## Domain Services

- **Suicide Risk Stratification Service**: Integrates multiple data sources to produce a comprehensive risk level.
- **De-escalation Protocol Selection Service**: Recommends appropriate interventions based on agitation level, diagnosis, and history.

## Clinical Workflow

1. Crisis is identified through patient presentation, clinician observation, or external notification.
2. Crisis event is activated with immediate documentation of presenting circumstances.
3. Shared kernel is accessed to retrieve current diagnoses and active medications.
4. Structured suicide risk assessment is conducted (C-SSRS or institutional protocol).
5. Risk level is determined and appropriate interventions are initiated.
6. De-escalation techniques are applied per protocol (verbal first, then environmental, then pharmacological as needed).
7. If imminent risk is determined, involuntary hold procedures are initiated per jurisdiction-specific statutes.
8. Ongoing monitoring continues until the patient is stabilized.
9. Disposition decision is made (admission, discharge, transfer) with clinical justification.
10. Safety plan is created or updated collaboratively with the patient.
11. Follow-up plan is established and communicated to the patient's treatment team.
12. Crisis event is resolved and domain events are published.

## Legal Considerations

This context has the highest legal sensitivity in the psychiatry domain. Involuntary holds, restraint, and seclusion are governed by federal regulations (CMS Conditions of Participation) and state mental health codes that vary by jurisdiction. The domain model must accommodate jurisdictional variation in hold statutes, documentation requirements, patient rights notification timelines, and judicial review processes. These legal requirements are modeled as configurable business rules, not hardcoded logic.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- Posner, Kelly, et al. "The Columbia-Suicide Severity Rating Scale." *American Journal of Psychiatry*, 2011.
- Richmond, Janet S., et al. "Verbal De-escalation of the Agitated Patient." *Western Journal of Emergency Medicine*, 2012.
- Simon, Robert I. *Assessing and Managing Suicide Risk*. American Psychiatric Publishing, 2004.
