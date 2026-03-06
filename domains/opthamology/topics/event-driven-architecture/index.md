# Event-Driven Architecture in Ophthalmology

## Overview

Event-driven architecture (EDA) provides the communication backbone for the
ophthalmology domain, enabling loosely coupled interactions between bounded contexts.
In this architecture, bounded contexts communicate primarily through domain events
rather than direct synchronous calls. When a clinically significant occurrence happens
in one context (e.g., an examination is completed), a domain event is published and
consumed by interested downstream contexts without the publishing context needing to
know about its consumers.

## Motivation for Event-Driven Design

The ophthalmology domain requires event-driven architecture for several reasons:

- Clinical workflows are inherently asynchronous. An examination may trigger imaging
  orders that are fulfilled hours or days later. Treatment decisions in Retinal Care
  may trigger surgical scheduling in a separate context.
- Bounded context autonomy requires decoupled communication. Each context must be able
  to evolve independently without coordinating releases with other contexts.
- Longitudinal patient care requires event history. Glaucoma management and retinal
  care track patient data over years; an event log provides a natural audit trail of
  clinical activity.
- Multiple consumers may react to the same event. An ExaminationCompleted event may
  simultaneously trigger imaging correlation, glaucoma profile updates, and retinal
  assessment workflows.

## Event Flow Patterns

### Clinical Workflow Events

The primary event flow follows the clinical workflow:

1. ExaminationCompleted events flow from Clinical Examination to Diagnostic Imaging,
   Glaucoma Management, and Retinal Care.
2. ImagingStudyInterpreted events flow from Diagnostic Imaging to Glaucoma Management,
   Retinal Care, and Surgical Management.
3. GlaucomaProgressionDetected events flow from Glaucoma Management to Surgical
   Management when medical therapy is insufficient.
4. RetinalConditionDiagnosed events flow from Retinal Care to Surgical Management
   when surgical intervention is indicated.
5. SurgicalCaseCompleted events flow from Surgical Management to Clinical Examination
   for post-operative follow-up coordination.

### Treatment Monitoring Events

Treatment monitoring creates cyclical event flows:

1. InjectionAdministered events from Retinal Care trigger scheduling of follow-up
   imaging studies in Diagnostic Imaging.
2. ImagingStudyInterpreted events return OCT results to Retinal Care for treatment
   response evaluation.
3. TreatmentResponseEvaluated events may trigger TreatmentRegimenModified events,
   adjusting the injection schedule.

### Progression Analysis Events

Glaucoma progression analysis uses accumulated events:

1. IOPRecorded events accumulate in the Glaucoma Patient Profile.
2. ImagingStudyInterpreted events provide periodic OCT and visual field data.
3. ProgressionAnalysisService aggregates these events to determine progression status.
4. GlaucomaProgressionDetected events trigger treatment plan review.

## Event Design Principles

### Event Naming

Events are named using past tense verbs from the ubiquitous language:
ExaminationCompleted, ImagingStudyInterpreted, SurgicalCaseScheduled. The name
clearly indicates what happened, not what should happen.

### Event Content

Each event carries sufficient data for consumers to act without querying the source
context. An ExaminationCompleted event includes key examination findings, not just
an examination ID that would require a callback.

### Event Ordering

Events within a single aggregate are ordered by occurrence. Cross-aggregate event
ordering is not guaranteed and consumers must handle out-of-order delivery gracefully.

### Event Idempotency

Event consumers are designed for idempotent processing. Receiving the same event
twice produces the same result as receiving it once, preventing duplicate clinical
actions.

### Event Versioning

Events are versioned using a schema version number. When the event schema evolves,
consumers must handle both old and new versions during a transition period. Breaking
schema changes are avoided; new fields are added as optional.

## Error Handling

- Failed event processing is retried with exponential backoff.
- Events that fail after maximum retries are sent to a dead letter queue for manual
  review by clinical informatics staff.
- Critical clinical events (e.g., AbnormalFindingDetected) have alerting mechanisms
  to ensure they are not lost or indefinitely delayed.

## Event Store

The event store maintains a persistent, append-only log of all domain events. This
serves as an audit trail for clinical activity, supports event replay for debugging
and analysis, and enables temporal queries (e.g., "what was this patient's IOP
trend over the past 24 months").

## Consistency Model

The ophthalmology domain uses eventual consistency between bounded contexts. Changes
within a single aggregate are strongly consistent (transactional). Changes across
aggregates and across bounded contexts are eventually consistent, coordinated through
domain events. Clinical workflows are designed to tolerate the brief delay in
cross-context consistency.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 8.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 11.
- Stopford, Ben. *Designing Event-Driven Systems*. O'Reilly Media, 2018.
