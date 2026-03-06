# Context Map in the Mental Health Domain

## Overview

A context map is a visual and textual representation of the relationships and
interactions between bounded contexts in a system. It makes explicit how
contexts depend on one another, which context has upstream authority, and what
integration patterns govern their communication. In the mental health domain,
the context map reveals the clinical workflow dependencies that connect
assessment, treatment planning, therapy, crisis response, medication, and
outcomes measurement.

## Context Relationships

### Clinical Assessment Context --> Treatment Planning Context

Relationship type: Customer-Supplier

The Clinical Assessment Context is the upstream supplier. When a diagnostic
assessment is completed, it publishes the diagnostic results including DSM-5
codes, severity ratings, and clinical formulations. The Treatment Planning
Context is the downstream customer that consumes these results to construct
individualized treatment plans. The Treatment Planning team can request
specific data formats from Clinical Assessment, but Clinical Assessment
controls the timing and structure of its outputs.

### Treatment Planning Context --> Therapy Management Context

Relationship type: Customer-Supplier

Treatment Planning is upstream. Once a treatment plan is approved, it publishes
the selected therapeutic modalities, session frequency, treatment goals, and
care level. Therapy Management is downstream and organizes its session
scheduling, note templates, and modality-specific protocols based on the
treatment plan directives.

### Treatment Planning Context --> Medication Management Context

Relationship type: Customer-Supplier

Treatment Planning is upstream. When the treatment plan includes pharmacotherapy,
it publishes medication recommendations, target symptoms, and prescribing
authority assignments. Medication Management is downstream and manages the
actual prescribing, titration, and monitoring workflows.

### Crisis Intervention Context <--> All Other Contexts

Relationship type: Partnership with priority override

Crisis Intervention operates as a special context that can preempt any other
context's workflow. When a patient enters a crisis state, Crisis Intervention
publishes high-priority domain events that other contexts must respect. For
example, an active suicide risk assessment may pause ongoing therapy sessions,
trigger emergency medication review, and escalate the treatment plan. This is
a partnership relationship because Crisis Intervention also depends on
information from other contexts (current medications, recent assessment
results, treatment history).

### Outcomes Measurement Context <-- All Other Contexts

Relationship type: Published Language subscriber

Outcomes Measurement is a downstream context that subscribes to events from
all five other contexts. It does not dictate upstream behavior but depends on
a published language of clinical events (assessment completed, session
conducted, medication changed, crisis resolved) to build its longitudinal
outcome records. This is a conformist relationship; Outcomes Measurement
conforms to the event schemas defined by upstream contexts.

### External System Integrations

The context map also documents relationships with systems outside the domain:

- EHR Systems: Clinical Assessment and Treatment Planning interact with
  external electronic health records through anti-corruption layers.
- Pharmacy Systems: Medication Management integrates with pharmacy benefit
  managers and drug databases through an open host service.
- Insurance/Payer Systems: Treatment Planning and Outcomes Measurement
  exchange information with insurance systems for authorization and
  quality reporting through anti-corruption layers.
- Crisis Hotlines: Crisis Intervention integrates with external crisis
  hotline services (e.g., 988 Suicide & Crisis Lifeline) through a
  published language interface.

## Integration Patterns Used

- Customer-Supplier: The primary pattern between assessment, planning, and
  downstream execution contexts.
- Published Language: Used for event-based communication, especially for
  Outcomes Measurement subscriptions.
- Anti-Corruption Layer: Used at all external system boundaries to prevent
  foreign models from contaminating internal contexts.
- Partnership: Used between Crisis Intervention and other contexts where
  bidirectional coordination is required.
- Conformist: Outcomes Measurement conforms to upstream event schemas.

## Conflict Resolution

When context boundaries create friction, the context map identifies where
translation is needed. Common translation points in the mental health domain:

- "Diagnosis" in Clinical Assessment is a structured DSM-5 code with
  specifiers; in Treatment Planning it becomes a set of problem statements
  that drive goal selection.
- "Session" in Therapy Management is a scheduled encounter; in Outcomes
  Measurement it is a data collection opportunity.
- "Medication" in Medication Management is a detailed prescription with
  pharmacokinetics; in Crisis Intervention it is a safety-relevant factor
  on the risk assessment checklist.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 14 on context maps.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 3 on context maps.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 8 on context mapping patterns.
- Brandolini, Alberto. "Introducing EventStorming." Leanpub, 2021. EventStorming as a technique for discovering context boundaries.
