# Event-Driven Architecture

Event-driven architecture in the heart health metrics domain provides the communication backbone for asynchronous, loosely coupled interaction between bounded contexts. It enables cardiac monitoring data, analysis findings, risk assessments, and alerts to flow through the system without requiring tight coupling between producing and consuming contexts.

## Overview

Event-driven architecture (EDA) is a natural fit for cardiac health monitoring because the domain is inherently event-driven: hearts beat, blood pressure changes, rhythms shift, thresholds are exceeded, and risk profiles evolve. Each of these occurrences generates domain events that multiple downstream contexts may need to process independently and at their own pace.

In the heart health metrics domain, EDA enables the Data Collection Context to publish measurement events without knowing which analysis algorithms will consume them. The Cardiac Analysis Context can publish arrhythmia detection events without knowing which monitoring rules or reporting dashboards will react. This decoupling supports independent deployment, scaling, and evolution of each bounded context.

## Key Concepts

**Domain Event Publishing.** Each bounded context publishes domain events when significant state changes occur. The Data Collection Context publishes "Heart Rate Reading Acquired" and "ECG Recording Completed" events. The Cardiac Analysis Context publishes "Arrhythmia Detected" and "HRV Analysis Completed" events. The Risk Assessment Context publishes "Risk Score Updated" events.

**Event Subscriptions.** Bounded contexts subscribe to events from other contexts based on their needs. The Monitoring and Alerts Context subscribes to cardiac anomaly events. The Reporting Context subscribes to analysis completion and risk score events. The Clinical Integration Context subscribes to events that require EHR synchronization.

**Event Ordering and Idempotency.** Cardiac events must be processed in temporal order within a patient's context. An arrhythmia detection event must not be processed before the underlying ECG recording event. Event consumers must be idempotent to handle duplicate deliveries without producing incorrect clinical results.

**Event Sourcing Considerations.** The cardiac analysis domain benefits from event sourcing, where the state of cardiac aggregates is reconstructed from their event history. This provides a complete audit trail of how cardiac findings evolved, supports retrospective reanalysis with updated algorithms, and enables temporal queries (e.g., "what was the patient's risk score before the medication change?").

**Eventual Consistency.** The heart health metrics system embraces eventual consistency between bounded contexts. When a new blood pressure reading is recorded, the risk assessment does not update instantaneously. The system is designed so that clinical decisions are made with awareness of data currency, and critical real-time paths (alert evaluation) use synchronous processing where necessary.

**Event Schema Evolution.** As the cardiac monitoring system evolves, event schemas change. New analysis capabilities produce new event types. Updated risk models add fields to risk score events. The architecture supports schema evolution through versioning and backward-compatible additions.

## Domain Examples

A continuous cardiac monitor detects a transition from normal sinus rhythm to atrial fibrillation. The event flow proceeds as follows: the Data Collection Context publishes a stream of heart rate readings showing increasing irregularity; the Cardiac Analysis Context, processing these readings, publishes an "Arrhythmia Detected" event with type "atrial fibrillation"; the Monitoring and Alerts Context receives the event and triggers a critical alert; the Clinical Integration Context receives the event and posts a FHIR Flag to the hospital EHR; the Reporting Context records the event on the patient's cardiac timeline.

Each of these downstream reactions happens independently and asynchronously. If the Reporting Context is temporarily unavailable, the alert still fires. If the Clinical Integration Context encounters an EHR outage, it queues the event for retry without affecting monitoring.

## Relationships to Other Topics

- [Domain Events](../domain-events/index.md) - Domain events are the fundamental building blocks of EDA.
- [Bounded Contexts](../bounded-contexts/index.md) - EDA enables loose coupling between contexts.
- [Integration Patterns](../integration-patterns/index.md) - EDA is one of several integration approaches.
- [Monitoring & Alerts Context](../monitoring-alerts-context/index.md) - Critical consumer of cardiac events.
- [Context Map](../context-map/index.md) - Event flows are documented on the context map.
- [Clinical Integration Context](../clinical-integration-context/index.md) - Translates domain events to FHIR resources.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 8 on domain events and Chapter 13 on integration.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly, 2021. Chapter 11 on event-driven architecture.
- Stopford, Ben. *Designing Event-Driven Systems*. O'Reilly, 2018.
- Brandolini, Alberto. "Introducing EventStorming." Leanpub, 2021.
- Kleppmann, Martin. *Designing Data-Intensive Applications*. O'Reilly, 2017. Chapters on stream processing.
