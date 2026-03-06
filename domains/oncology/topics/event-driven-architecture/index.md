# Event-Driven Architecture in Oncology

## Overview

Event-driven architecture (EDA) provides the communication backbone between bounded contexts in the oncology domain. Rather than contexts directly calling each other, they communicate through domain events that represent significant clinical occurrences. This architectural pattern enables loose coupling between oncology contexts while maintaining the coordinated workflows essential for cancer care. EDA naturally fits oncology because clinical workflows are inherently event-driven: a diagnosis triggers treatment planning, treatment completion triggers survivorship care, and toxicity triggers dose modification.

## Why Event-Driven Architecture for Oncology

Oncology care is a sequence of clinical events that trigger downstream actions across organizational boundaries. When a pathologist finalizes a diagnosis, that event triggers tumor board review, which triggers treatment planning, which triggers treatment orders across multiple modality departments. These workflows cross team boundaries, happen on different timescales, and involve different specialists.

Direct synchronous communication between these contexts would create tight coupling, making each context dependent on the availability and response time of every other context it interacts with. Event-driven architecture decouples these interactions. The Diagnosis and Staging Context publishes a DiagnosisConfirmed event without knowing or caring which other contexts will respond. The Treatment Planning Context subscribes to that event and processes it on its own timeline.

## Event Flow Patterns in Oncology

### The Diagnostic Cascade

The diagnostic cascade begins with a ScreeningResultReceived event that may trigger a diagnostic workup. When the workup completes, a PathologyReportFinalized event is published. The Diagnosis and Staging Context processes this event to confirm the diagnosis and publish a DiagnosisConfirmed event. The Treatment Planning Context subscribes to DiagnosisConfirmed and initiates the tumor board case preparation workflow.

### The Treatment Cascade

When the tumor board reaches a recommendation, a TumorBoardRecommendationIssued event is published. The Treatment Planning Context finalizes the treatment plan and publishes TreatmentPlanApproved. Each treatment modality context subscribes to this event and filters for plans that include its modality. The Chemotherapy Management Context creates orders for systemic therapy. The Radiation Therapy Context initiates the simulation workflow. The Surgical Oncology Context begins preoperative planning.

### The Feedback Loop

Treatment outcomes generate events that may circle back to modify the treatment plan. A ToxicityRecorded event from the Chemotherapy Management Context may trigger a dose modification within that context, but severe toxicity may also trigger a TreatmentPlanRevised event in the Treatment Planning Context. A MarginAssessmentReported event from the Surgical Oncology Context may flow through the Diagnosis and Staging Context (updating staging) and back to the Treatment Planning Context (revising the treatment plan).

### The Survivorship Transition

Treatment completion events from each modality context (TreatmentCourseCompleted, RadiationCourseCompleted, SurgicalProcedureCompleted) flow to the Survivorship Care Context, which assembles the comprehensive treatment summary and generates the survivorship care plan. Later, a RecurrenceSuspected event from survivorship surveillance may restart the entire diagnostic and treatment cascade.

## Event Design Principles

### Event Naming

Events are named in past tense to indicate completed actions: DiagnosisConfirmed, not ConfirmDiagnosis. Event names use the ubiquitous language of the publishing context. Event names should be specific enough to convey meaning without reading the payload: ToxicityRecorded is more informative than DataUpdated.

### Event Payload

Each event carries sufficient context for consumers to act without querying the publishing context. A DiagnosisConfirmed event includes the cancer type, TNM stage, and key molecular markers, not just a diagnosis ID. This self-contained design reduces inter-context coupling and improves resilience when a context is temporarily unavailable.

### Event Versioning

As the oncology domain model evolves, event schemas will change. Event versioning strategies must accommodate this evolution without breaking existing consumers. Adding new optional fields to events is backward-compatible. Changing the meaning of existing fields requires a new event version with explicit migration support.

## Delivery Guarantees

Different oncology events require different delivery guarantees based on their clinical significance.

At-least-once delivery is required for safety-critical events such as ToxicityRecorded (Grade 3 or higher), CumulativeDoseLimitApproaching, and PositiveMarginIdentified. These events may trigger clinical actions that affect patient safety, and missing them could have serious consequences.

At-most-once delivery may be acceptable for informational events such as SurveillanceTestDue or SurvivorshipCarePlanGenerated, where duplication would cause confusion but a missed event can be recovered through other channels.

Exactly-once delivery, while desirable, is difficult to achieve in distributed systems. For oncology events that require exactly-once semantics, idempotent event handlers combined with at-least-once delivery provide a practical solution.

## Event Ordering

Within a single aggregate's event stream, events must be ordered. A ChemotherapyCycleStarted event must precede the corresponding ChemotherapyCycleCompleted event. Across different aggregates and contexts, strict global ordering is generally not required. The event infrastructure must support per-aggregate ordering while allowing concurrent processing of events from different aggregates.

## Event Storage and Audit

Oncology regulations require comprehensive audit trails. Domain events provide a natural audit log because they represent an immutable record of everything that happened in the system. Event sourcing, where the current state of an aggregate is derived from its event history, is particularly well-suited to oncology because the clinical history itself is medically significant.

## Error Handling

When an event consumer fails to process an event, the error handling strategy must account for clinical implications. A failed processing attempt for a ToxicityRecorded event should trigger alerts to clinical staff, not just a retry queue. Dead letter queues for unprocessable events must be monitored with clinical awareness of the potential impact.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 8: Domain Events.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 10: Domain Events.
- Stopford, Ben. Designing Event-Driven Systems. O'Reilly Media, 2018.
