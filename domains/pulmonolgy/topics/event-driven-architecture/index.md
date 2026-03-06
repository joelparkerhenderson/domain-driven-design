# Event-Driven Architecture in the Pulmonolgy Domain

## Overview

Event-driven architecture (EDA) enables asynchronous communication between bounded contexts through domain events. In the pulmonolgy domain, EDA decouples the six bounded contexts so that each can operate independently while still coordinating clinical workflows that span assessment, diagnostics, chronic management, critical care, sleep medicine, and rehabilitation.

The pulmonolgy domain is inherently event-driven in its clinical reality. A completed pulmonary assessment triggers diagnostic workup. A confirmed diagnosis initiates treatment planning. An exacerbation detection activates clinical response protocols. A successful extubation begins rehabilitation planning. Event-driven architecture models these natural clinical triggers faithfully, ensuring the software architecture mirrors the domain's actual workflow dynamics.

## Event Flow Patterns

### Assessment-to-Diagnostics Flow

When the Pulmonary Assessment Context publishes an AssessmentCompleted event, the Respiratory Diagnostics Context subscribes and evaluates whether diagnostic procedures are indicated based on the assessment findings. This flow is asynchronous: the assessment context does not wait for the diagnostics context to respond. The diagnostics context independently applies its clinical indication rules to determine appropriate follow-up.

### Diagnostics-to-Management Flow

When the Respiratory Diagnostics Context publishes a DiagnosticResultAvailable event, the Chronic Disease Management Context subscribes and incorporates the results into disease classification and treatment planning. A confirmed ILD diagnosis, for example, triggers initial treatment plan creation with disease-specific monitoring schedules.

### Exacerbation Cascade

When the Chronic Disease Management Context publishes an ExacerbationDetected event, multiple contexts may respond:
- The Critical Care Context evaluates whether ICU-level intervention is needed for severe exacerbations.
- The Pulmonary Rehabilitation Context adjusts active program parameters or pauses rehabilitation activities during acute episodes.
- The Chronic Disease Management Context itself may trigger treatment plan revision workflows.

### ICU-to-Rehabilitation Flow

When the Critical Care Context publishes an ExtubationCompleted event, the Pulmonary Rehabilitation Context subscribes and initiates post-ICU rehabilitation planning. The event carries sufficient data for the rehabilitation context to begin functional status assessment and exercise program design through its anti-corruption layer translation.

### Sleep Medicine Compliance Loop

The Sleep Medicine Context publishes TherapyComplianceReported events periodically. The Chronic Disease Management Context may subscribe to these events for patients where OSA is a comorbidity affecting chronic disease management. Suboptimal CPAP compliance in a patient with COPD-OSA overlap syndrome may inform treatment plan adjustments in the chronic disease management context.

## Event Publication and Subscription

### Publication Principles

Events are published by the bounded context where the significant occurrence takes place. The publishing context is the authority on the event's content and does not need to know which contexts subscribe. This ensures loose coupling: the Pulmonary Assessment Context publishes AssessmentCompleted without any dependency on the Respiratory Diagnostics Context.

Each published event is immutable and carries a unique event identifier, a timestamp, and sufficient payload data for subscribers to react without querying back to the publishing context. This self-contained design reduces inter-context coupling and supports eventual consistency.

### Subscription Principles

Subscribing contexts register interest in specific event types and process them independently. Each subscriber decides how to interpret and react to an event based on its own domain logic. The Chronic Disease Management Context processes a DiagnosticResultAvailable event differently than the Pulmonary Assessment Context would (if it subscribed), because each context applies its own business rules.

Subscribers are designed for idempotent processing, meaning they can safely handle duplicate event delivery without producing incorrect results. This is essential in distributed healthcare systems where network reliability cannot be guaranteed.

## Eventual Consistency

Event-driven architecture in the pulmonolgy domain embraces eventual consistency across bounded context boundaries. When an AssessmentCompleted event is published, the downstream diagnostics context will eventually process it and update its own state, but there is no guarantee of instantaneous consistency.

This eventual consistency model aligns well with clinical reality. In practice, diagnostic workup does not begin instantaneously when an assessment is completed. There is always a natural temporal gap between clinical activities in different care areas. The event-driven architecture models this natural asynchrony rather than imposing artificial synchronous coupling.

However, within a single bounded context, strong consistency is maintained through aggregate design. A VentilationSession aggregate enforces immediate consistency between ventilator settings and physiological response documentation.

## Event Ordering and Temporal Integrity

Clinical events in pulmonolgy have natural temporal ordering that must be preserved. An ExacerbationDetected event must be processed before the corresponding ExacerbationResolved event. Events carry timestamps and sequence identifiers that enable subscribers to process them in the correct clinical order.

For contexts that require strict ordering (such as the Critical Care Context processing ventilator setting change events), partitioned event streams keyed by patient identifier ensure that events for a given patient are processed in sequence.

## Error Handling and Compensation

When event processing fails, the system must handle the failure gracefully without compromising clinical safety. Failed events are routed to dead-letter queues for investigation and reprocessing. Critical clinical events (such as severe exacerbation notifications) include escalation mechanisms that ensure they are not lost.

Compensating events are published when a previous event's effects need to be reversed. For example, if a DiagnosticResultAvailable event is later found to contain an error, a DiagnosticResultCorrected event is published to notify subscribers of the correction.

## Monitoring and Observability

Event flow monitoring tracks publication rates, subscription lag, processing latency, and failure rates across all bounded contexts. This observability is essential for ensuring that clinical workflows are progressing as expected and that no critical events are stuck in processing queues.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 8 on domain events as a modeling concept.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 8 on domain event design and event-driven architecture patterns.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 11 on event-driven architecture and its relationship to bounded context communication.
