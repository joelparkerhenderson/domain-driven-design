# Event-Driven Architecture in the Rheumatology Domain

## Overview

Event-driven architecture enables communication between bounded contexts through domain events, allowing each context to operate autonomously while reacting to significant occurrences in other contexts. In the rheumatology domain, event-driven architecture is essential because clinical workflows span multiple contexts: an assessment triggers disease activity evaluation, which may trigger therapy adjustment, which requires safety monitoring, and all of these produce data for outcome tracking. Events decouple these contexts temporally and structurally, allowing each to evolve independently.

## Event Flow Patterns

### Assessment-Driven Flow

The most common event flow begins in the Clinical Assessment Context. When a clinician completes a patient evaluation, the AssessmentCompleted event is published. This event carries the patient identifier, assessment date, disease activity scores, joint counts, and key laboratory results.

The Autoimmune Disease Context subscribes to AssessmentCompleted to compare the new disease activity score against the treatment target. If the score indicates a flare, the DiseaseFlareDetected event is published. If the score indicates remission, the RemissionAchieved event is published. If the score indicates inadequate response to therapy after a defined period, the TreatmentEscalated event may follow.

The Outcomes Tracking Context also subscribes to AssessmentCompleted to record the assessment as a longitudinal data point. It independently calculates disease activity trajectories and generates TreatToTargetReviewDue events when scheduled reassessment intervals elapse.

### Therapy-Driven Flow

When the Autoimmune Disease Context determines that therapy escalation to a biologic agent is needed, the TreatmentEscalated event reaches the Biologic Therapy Context. The Biologic Therapy Context initiates its pre-treatment screening workflow. Upon successful screening and therapy initiation, the TherapyStarted event is published.

The Outcomes Tracking Context subscribes to TherapyStarted to mark the therapy initiation as a landmark in the patient timeline. Subsequent AssessmentCompleted events are evaluated in the context of the new therapy to determine treatment response.

If an infusion reaction occurs, the InfusionReactionOccurred event is published. The Outcomes Tracking Context records this adverse event. The Biologic Therapy Context uses this event internally to adjust future infusion protocols.

### Pain-Driven Flow

The DiseaseFlareDetected event triggers a review in the Pain Management Context, which may create or update a PainManagementPlan. The PainPlanCreated event notifies the Outcomes Tracking Context that a new pain intervention baseline has been established. TherapyProgressReported events from physical and occupational therapy providers flow back to update the pain management plan and contribute to outcome tracking.

## Event Design

### Event Structure

Events in the rheumatology domain follow a consistent structure. Each event includes an event identifier, event type, timestamp, source context, patient identifier, and a payload containing context-specific data. Events use past tense naming (AssessmentCompleted, DiseaseFlareDetected, TherapyStarted) to indicate they represent facts that have already occurred.

### Event Granularity

Events are designed at a granularity that matches clinical significance. An AssessmentCompleted event carries the full assessment summary rather than individual sub-events for each joint examined. This granularity reduces event volume while providing consumers with sufficient information to react without additional queries.

However, some events are fine-grained when clinical significance demands it. An InfusionReactionOccurred event is published immediately when a reaction is detected because it may require urgent clinical response, rather than waiting for an InfusionSessionCompleted summary event.

### Event Enrichment

Events carry enough data for consumers to react without synchronously querying the source context. The AssessmentCompleted event includes the DAS28 score, interpretation category, and component values so that the Autoimmune Disease Context can immediately evaluate against the treatment target. This enrichment pattern reduces inter-context coupling and supports asynchronous processing.

## Choreography vs. Orchestration

The rheumatology domain primarily uses event choreography, where each context independently decides how to respond to events, rather than a central orchestrator directing the workflow. This approach aligns with the clinical reality that different specialists (rheumatologists, pharmacists, therapists) independently react to clinical events according to their professional judgment and protocols.

The assessment-disease-therapy flow is choreographed: the Clinical Assessment Context does not know or care what downstream contexts do with its assessment events. The Autoimmune Disease Context reacts based on its own rules. The Biologic Therapy Context responds to escalation decisions independently.

Choreography supports context autonomy and reduces the risk that a single point of failure disrupts the entire workflow.

## Event Store and Audit

Given the clinical nature of the domain, all events are stored in an append-only event store that serves as an audit trail. This event store supports regulatory compliance requirements (HIPAA audit logging), clinical review of the sequence of events leading to a clinical decision, and potential event replay for system recovery or analytics.

The event store is distinct from the aggregate state stores in each context. It captures the complete history of inter-context communication, providing a temporal record of how clinical decisions unfolded.

## Error Handling and Compensation

When an event consumer fails to process an event, the system uses retry with exponential backoff. If processing continues to fail, the event is placed in a dead letter queue for manual review. In clinical systems, silent event loss is unacceptable because it could mean a missed flare detection or an unrecorded adverse event.

Compensating events are used when a prior event's effects need to be reversed. For example, if a TherapyStarted event was published but the pre-treatment screening is subsequently found to be invalid, a TherapyCancelled compensating event is published.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 8.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 10.
- Hohpe, Gregor, and Bobby Woolf. Enterprise Integration Patterns. Addison-Wesley, 2003.
- Brandolini, Alberto. "Introducing EventStorming." Leanpub, 2021.
