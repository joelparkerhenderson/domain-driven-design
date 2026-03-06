# Event-Driven Architecture - Otolaryngology Domain

## Overview

Event-driven architecture (EDA) enables communication between bounded contexts through domain events, supporting loose coupling and independent evolution of each ENT subspecialty area. In the otolaryngology domain, EDA connects the six bounded contexts without creating direct dependencies, allowing each context to react to clinically significant occurrences in other contexts while maintaining its own model integrity.

## Why Event-Driven Architecture in Otolaryngology

ENT clinical workflows are inherently event-driven. A completed endoscopy triggers voice evaluation. A failed CPAP trial triggers surgical consultation. A cochlear implant surgery triggers device activation planning. These clinical sequences are natural event chains where one significant occurrence triggers downstream actions. Modeling these as explicit domain events rather than direct method calls preserves the autonomy of each bounded context.

EDA also supports the temporal nature of ENT care. Events create an immutable history of clinical decisions and actions. When reviewing a patient's hearing journey, the sequence of events (HearingEvaluationCompleted, HearingAidFitted, CochlearImplantCandidacyDetermined, SurgeryCompleted, AuralRehabilitationMilestoneReached) tells the clinical story without requiring each context to maintain a unified patient timeline.

## Event Flow Patterns

### Clinical Assessment as Event Source

The Clinical Assessment Context is the primary event source, publishing diagnostic findings that trigger subspecialty workflows:

- EncounterCompleted with laryngeal findings triggers Voice Disorders evaluation workflow.
- EncounterCompleted with sleep complaints triggers Sleep Disorders screening workflow.
- EncounterCompleted with nasal/sinus symptoms triggers Allergy Management evaluation.
- AudiometricScreeningCompleted with abnormal results triggers Hearing Services comprehensive evaluation.
- ImagingStudyReviewed with surgical findings triggers Surgical Management pre-operative planning.

### Treatment Failure Cascades

When conservative treatment fails, events cascade to escalate care:

1. CPAPComplianceAssessed (Sleep Disorders) with non-compliance triggers SleepSurgeryReferred.
2. SleepSurgeryReferred triggers Surgical Management pre-operative evaluation.
3. SurgeryCompleted (Surgical Management) triggers Sleep Disorders post-operative outcome measurement.

Similarly for allergy:
1. Medical therapy failure documented (Allergy Management) triggers SinusitisSurgeryReferred.
2. SinusitisSurgeryReferred triggers Surgical Management pre-operative evaluation.
3. SurgeryCompleted triggers Allergy Management post-surgical medical management.

### Surgical Outcome Distribution

When surgery is completed, outcome events are distributed to relevant subspecialty contexts:

- Phonosurgery completion notifies Voice Disorders for post-surgical rehabilitation.
- Cochlear implant surgery completion notifies Hearing Services for device activation.
- Sinus surgery completion notifies Allergy Management for ongoing medical management.
- Thyroidectomy completion triggers post-operative calcium monitoring and voice assessment.

## Event Design Guidelines

### Event Naming

Events use past-tense, clinically meaningful names:
- Use "SleepStudyCompleted" not "ProcessSleepStudy"
- Use "HearingAidFitted" not "HearingAidUpdate"
- Use "ImmunotherapyReactionRecorded" not "AllergyAlert"

### Event Payload

Each event carries sufficient data for consumers to react without callback:
- Patient identifier for cross-context correlation.
- Clinical summary relevant to potential consumers.
- Timestamps for temporal ordering and audit.
- Source context identifier for provenance tracking.

### Event Ordering and Idempotency

Clinical events must be processed in order within a patient's care pathway. Events include sequence identifiers and consumers implement idempotent processing to handle duplicate delivery. A HearingEvaluationCompleted event processed twice should not create duplicate evaluation records.

## Event Store Considerations

An event store provides an append-only log of all domain events, supporting:

- **Audit trail**: Complete history of clinical decisions and actions across all contexts.
- **Temporal queries**: Ability to reconstruct a patient's state at any point in time.
- **Event replay**: Ability to rebuild read models or populate new bounded contexts from historical events.
- **Compliance**: HIPAA requires audit trails for protected health information access, which the event store naturally provides.

## Eventual Consistency

EDA introduces eventual consistency between bounded contexts. When an EncounterCompleted event is published, the Hearing Services Context may not immediately reflect the new screening results. This is clinically acceptable because ENT subspecialty workflows operate on timescales of days to weeks, not milliseconds. A hearing evaluation scheduled in response to a screening abnormality occurs days later, providing ample time for event propagation.

## Error Handling

### Dead Letter Queues

Events that cannot be processed by a consumer are routed to dead letter queues for investigation. A SurgeryCompleted event that references an unknown patient in Hearing Services indicates a data integrity issue requiring manual resolution.

### Compensating Events

When a domain event is published in error (e.g., a sleep study result is later corrected), a compensating event (SleepStudyResultCorrected) is published rather than modifying the original event. Consumers process the correction according to their own context rules.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 8 on domain events.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapters 8 and 13 on event-driven integration.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 11 on event-driven architecture.
- Stopford, Ben. *Designing Event-Driven Systems*. O'Reilly Media, 2018.
