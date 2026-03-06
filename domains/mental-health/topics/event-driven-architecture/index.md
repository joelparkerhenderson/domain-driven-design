# Event-Driven Architecture in the Mental Health Domain

## Overview

Event-driven architecture (EDA) is the communication backbone that connects
bounded contexts in the mental health domain. Rather than having contexts
call each other directly through synchronous APIs, contexts publish domain
events when significant things happen and other contexts subscribe to the
events they care about. This produces loose coupling between contexts while
preserving the causal chain of clinical workflows.

In the mental health domain, EDA is particularly important because clinical
workflows are inherently event-driven: a completed assessment triggers
treatment planning, a crisis episode interrupts scheduled therapy, a
medication change prompts outcome reassessment. The architecture mirrors
the natural flow of clinical care.

## Event Flow Architecture

### Primary Event Chains

The mental health domain has several primary event chains that represent
the core clinical workflow:

**Assessment-to-Treatment Chain**: AssessmentCompleted (Clinical Assessment)
triggers TreatmentPlanCreated (Treatment Planning), which triggers
downstream session scheduling in Therapy Management and medication orders
in Medication Management.

**Crisis Interrupt Chain**: CrisisInitiated (Crisis Intervention) is a
high-priority event that broadcasts to all contexts. Therapy Management
may cancel upcoming sessions. Medication Management may flag urgent
medication review. Treatment Planning may escalate care level.
CrisisResolved triggers resumption of normal workflows.

**Outcome Feedback Chain**: OutcomeMeasured and ClinicallySignificantChange
(Outcomes Measurement) flow back to Treatment Planning, which may trigger
TreatmentPlanRevised if the patient is not responding to treatment.

**Medication Safety Chain**: DrugInteractionDetected (Medication Management)
triggers urgent clinician notification. MedicationDiscontinued may prompt
assessment of withdrawal symptoms through the Clinical Assessment Context.

### Event Priority Levels

Not all events have equal urgency. The mental health domain defines three
priority levels:

- Critical: crisis-related events (CrisisInitiated, InvoluntaryHoldInitiated)
  that require immediate processing and may interrupt other workflows.
- Standard: clinical workflow events (AssessmentCompleted, SessionCompleted,
  MedicationPrescribed) that drive normal care progression.
- Informational: tracking events (OutcomeMeasured, TherapeuticAllianceRated)
  that provide data for analysis but do not trigger urgent action.

## Event Design Patterns

### Event Notification

Most inter-context communication in the mental health domain uses the event
notification pattern. The publishing context emits a lightweight event
containing identifiers and key facts. The subscribing context decides
whether to act and may query the publishing context for additional details
if needed.

Example: AssessmentCompleted carries the patient identifier, diagnostic codes,
and severity ratings. Treatment Planning uses this information to initiate
plan creation without needing the full assessment instrument responses.

### Event-Carried State Transfer

For events consumed by the Outcomes Measurement Context, the event-carried
state transfer pattern is used. Events carry enough clinical data for
Outcomes Measurement to build its longitudinal records without querying
source contexts. This reduces coupling and improves the resilience of
outcome tracking.

### Event Sourcing Considerations

The Crisis Intervention Context benefits from event sourcing, where the
CrisisEpisode aggregate's state is derived from the sequence of events that
occurred during the episode. This provides a complete audit trail of every
decision and action taken during a crisis, which is valuable for quality
review, legal documentation, and clinical supervision.

## Eventual Consistency

Event-driven architecture introduces eventual consistency between bounded
contexts. In the mental health domain, this means:

- After an AssessmentCompleted event is published, there is a brief window
  before the Treatment Planning Context has processed it. During this window,
  the system state is temporarily inconsistent.
- This is clinically acceptable because treatment planning does not happen
  instantaneously after assessment in the real world either. The eventual
  consistency window should be configured to align with clinical workflow
  expectations (seconds to minutes, not hours or days).
- For crisis events, the consistency window must be minimized. Critical
  events are processed with higher priority and lower latency.

## Error Handling and Compensation

When event processing fails, the system must handle the failure without
losing clinical data:

- Failed event processing is retried with exponential backoff.
- Events that repeatedly fail are routed to a dead letter queue for manual
  clinical review.
- Compensating events are published when a previous event's effects need
  to be reversed (e.g., CrisisResolved compensates for CrisisInitiated).
- Idempotent event handlers ensure that reprocessing an event does not
  create duplicate clinical records.

## Security and Compliance

Event streams in the mental health domain carry protected health information
and must comply with HIPAA and 42 CFR Part 2:

- Events are encrypted in transit and at rest.
- Access to event streams is controlled by context-level authorization.
- Substance use disorder-related events receive additional access controls
  per 42 CFR Part 2.
- Event audit logs are retained per regulatory retention requirements.
- Patient consent for information sharing is validated before events cross
  certain context boundaries.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 8 on domain events.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 11 on event-driven architecture.
- Fowler, Martin. "What do you mean by Event-Driven?" martinfowler.com, 2017.
- Richardson, Chris. Microservices Patterns. Manning, 2018. Chapter 3 on event-driven communication.
