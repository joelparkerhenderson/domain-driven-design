# Event-Driven Architecture in the Orthopaedic Domain

## Overview

Event-driven architecture (EDA) is a design approach where bounded contexts communicate
by publishing and subscribing to domain events. In the orthopaedic domain, EDA enables
the six bounded contexts to coordinate clinical workflows without direct dependencies,
supporting the natural flow of orthopaedic care from assessment through treatment
to rehabilitation.

## Purpose

Orthopaedic care is inherently sequential and event-driven: a diagnosis triggers
surgical planning, surgery triggers rehabilitation, healing milestones trigger
weight-bearing changes. Modeling these transitions as domain events rather than direct
method calls preserves the autonomy of each bounded context. The Clinical Assessment
Context does not need to know how Surgical Planning constructs an operative plan; it
only needs to publish the fact that an assessment was completed.

## Event Flow Through the Orthopaedic Pathway

### Assessment to Treatment

When the Clinical Assessment Context completes a patient evaluation, it publishes
a PatientAssessed event. This event is consumed by the appropriate treatment context
based on the diagnosis:

- Fracture diagnosis: consumed by Fracture Management Context.
- Joint disease diagnosis: consumed by Surgical Planning Context.
- Athletic injury diagnosis: consumed by Sports Medicine Context.

### Planning to Execution

When the Surgical Planning Context approves an operative plan, it publishes a
SurgicalPlanApproved event. This event triggers preparation in:

- Joint Replacement Context (for arthroplasty procedures).
- Fracture Management Context (for operative fracture fixation).

### Surgery to Rehabilitation

When a surgical context completes a procedure, it publishes a completion event:

- ArthroplastyCompleted from Joint Replacement.
- FractureFixated from Fracture Management.
- ArthroscopyCompleted from Sports Medicine.

The Rehabilitation Context subscribes to all three events and initiates the
appropriate post-operative protocol based on the event type and content.

### Healing to Progression

When the Fracture Management Context confirms union (FractureUnionConfirmed), the
Rehabilitation Context uses this event to advance weight-bearing status.

## Event Design Standards

### Event Structure

Each domain event in the orthopaedic domain includes:

- Event identifier: A unique identifier for this specific event instance.
- Event type: The fully qualified event name (e.g., ArthroplastyCompleted).
- Timestamp: When the event occurred in the domain.
- Aggregate identifier: The identity of the aggregate that produced the event.
- Payload: The domain data relevant to consumers.
- Version: The schema version of the event.

### Event Naming Conventions

- Past tense: events describe something that has already happened.
- Domain language: event names use the ubiquitous language of the publishing context.
- Specificity: events are specific enough to convey meaning without ambiguity.

### Event Payload Guidelines

- Include all data consumers need to react without querying the source context.
- Do not include data that is only relevant to the publishing context.
- Use value objects in the event payload, not entity references.
- Version the payload schema to support independent evolution.

## Eventual Consistency

Event-driven communication between orthopaedic bounded contexts introduces eventual
consistency. When an ArthroplastyCompleted event is published, the Rehabilitation
Context may not immediately have the rehabilitation episode created. This is acceptable
because clinical workflows naturally involve time gaps between surgery and the first
therapy session.

## Error Handling

- Failed event processing should be retried with idempotent handlers.
- Dead-letter queues capture events that cannot be processed after retries.
- Compensating events can reverse the effects of events processed in error.
- Clinical safety events (e.g., implant recall notifications) should use guaranteed
  delivery mechanisms.

## Benefits for Orthopaedics

- Each context can be developed, deployed, and scaled independently.
- New contexts can be added by subscribing to existing events.
- The clinical workflow is explicitly modeled in the event flow.
- Audit trails are built from the event history.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 14.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapters 8, 13.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 11.
