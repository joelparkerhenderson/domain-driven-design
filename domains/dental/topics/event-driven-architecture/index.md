# Event-Driven Architecture - Dental Domain

## Overview

Event-driven architecture (EDA) provides the communication backbone between bounded contexts in the dental domain. Rather than direct synchronous calls between contexts, domain events propagate information about significant occurrences, enabling loose coupling while maintaining data consistency across the system. In the dental domain, events flow naturally along the clinical workflow: examination findings trigger treatment planning, approved plans trigger procedure execution, completed procedures trigger chart updates and billing.

## Event-Driven Principles in Dental Systems

Dental practice workflows are inherently event-driven. A clinical examination produces findings that trigger downstream actions. A patient's approval of a treatment plan authorizes procedure scheduling. A completed restoration triggers both clinical chart updates and insurance claim creation. Modeling these interactions as domain events rather than direct service calls creates a system that mirrors the natural workflow of dental practice.

Events in the dental domain are:
- Immutable records of something that happened, named in past tense.
- Self-contained with all information needed by consumers.
- Published without knowledge of which contexts will consume them.
- Processed asynchronously, supporting eventual consistency between contexts.

## Primary Event Flows

### Examination to Treatment Flow

The examination-to-treatment flow is the most common event chain in the dental domain:

1. ExaminationCompleted event is raised by the Clinical Examination Context.
2. The Treatment Planning Context consumes this event and creates a draft treatment plan.
3. CariesDetected and RadiographInterpreted events provide additional detail for plan items.
4. The Cost Estimation domain service calculates financial estimates.
5. TreatmentPlanApproved event is raised after patient consent.
6. Downstream clinical contexts receive approved items for scheduling and execution.

### Procedure to Billing Flow

The procedure-to-billing flow connects clinical completion with administrative processing:

1. RestorationCompleted (or other procedure completion event) is raised by the clinical context.
2. The Clinical Examination Context updates the dental chart.
3. The Practice Management Context creates a claim with appropriate CDT codes.
4. ClaimSubmitted event records the claim submission.
5. ClaimAdjudicated event records the payment determination.

### Periodontal Monitoring Flow

The periodontal monitoring flow supports ongoing disease management:

1. PeriodontalProbingRecorded event captures new measurements.
2. The Periodontal Care Context updates the longitudinal record and evaluates disease classification.
3. DiseaseClassificationChanged event triggers maintenance interval adjustment.
4. PeriodontalMaintenanceScheduled event configures recall scheduling.

### Orthodontic Progress Flow

The orthodontic progress flow tracks long-duration treatment:

1. OrthodonticCaseStarted event initiates recurring appointment scheduling.
2. OrthodonticProgressRecorded events update the clinical record at each visit.
3. OrthodonticTreatmentCompleted event triggers retention phase scheduling.

## Event Publication Patterns

### Event Store

All domain events are persisted in an event store before publication. This provides an audit trail of all significant occurrences within the dental system, supports event replay for system recovery, and enables temporal queries about the history of any clinical or administrative process.

### Event Bus

Events are distributed through an event bus that supports publish-subscribe patterns. Producing contexts publish events without knowledge of consumers. Consuming contexts subscribe to specific event types relevant to their responsibilities. The event bus handles delivery guarantees and ordering within an aggregate's event stream.

### Event Versioning

As the dental domain model evolves, event schemas may need to change. Event versioning strategies include:
- Adding optional fields to existing events (backward compatible).
- Creating new event types when the semantic meaning changes.
- Maintaining event upcasters that transform old event formats to current versions.

## Eventual Consistency Considerations

The dental domain accepts eventual consistency between bounded contexts because clinical workflows have natural time gaps. A treatment plan is created after examination, not simultaneously. A claim is submitted after procedure completion, not during. The dental domain's tolerance for eventual consistency aligns well with event-driven architecture patterns.

However, certain consistency requirements remain immediate:
- Within an aggregate, consistency is transactional and immediate.
- The dental chart must not show conflicting concurrent modifications to the same tooth.
- Insurance claim amounts must be consistent with the procedures they reference.

## Error Handling and Compensation

When an event-triggered action fails, the dental domain uses compensating events rather than distributed transactions. If a claim submission fails after a procedure completion event, a ClaimSubmissionFailed event triggers a retry workflow rather than rolling back the procedure completion. Clinical events (procedures performed) are never reversed due to administrative processing failures.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 8 on domain events.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 8 on domain events and event-driven architecture.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 11 on event-driven architecture patterns.
- Stopford, Ben. Designing Event-Driven Systems. O'Reilly Media, 2018.
