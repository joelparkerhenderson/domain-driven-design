# Event-Driven Architecture in Medical Practice

## Overview

Event-driven architecture (EDA) is an architectural pattern in which bounded contexts communicate by producing and consuming domain events rather than through direct synchronous calls. In medical practice, EDA enables loosely coupled integration between patient records, clinical workflows, diagnostics, pharmacy, insurance, and telemedicine contexts while maintaining the audit trails and traceability that healthcare regulations demand.

## Why EDA Matters in Medicine

Medical systems have characteristics that make event-driven architecture particularly valuable:

- **Temporal decoupling**: A lab result can be processed and stored even if the ordering provider's system is momentarily unavailable
- **Audit trail**: Every event is a durable record of what happened and when, supporting HIPAA audit requirements
- **Scalability**: High-volume events (remote monitoring device readings, lab results) can be processed asynchronously without blocking clinical workflows
- **Loose coupling**: The pharmacy context does not need to know the internal structure of the clinical workflow context; it only needs to understand the PrescriptionIssued event
- **Resilience**: If the insurance claims system is down, clinical care continues and claims are generated when the system recovers

## Event Flow Patterns

### Choreography

In choreography, each bounded context independently reacts to events without a central coordinator. This is the dominant pattern in the medical domain:

1. Provider completes encounter -> EncounterCompleted event published
2. Patient Records context updates patient history
3. Insurance & Claims context generates and submits a claim
4. Each context acts independently based on the event

Choreography works well when the workflow is straightforward and each context knows what to do in response to an event.

### Orchestration

In orchestration, a central process (saga) coordinates a multi-step workflow by sending commands to bounded contexts and reacting to their responses:

1. CareTransitionOrchestrator receives PatientTransferred event
2. Sends command to Pharmacy: ReconcileMedications
3. Waits for MedicationsReconciled response
4. Sends command to Insurance: VerifyEligibilityAtNewFacility
5. Waits for EligibilityVerified response
6. Sends command to Patient Records: UpdateCareTeam

Orchestration is appropriate for complex, multi-step workflows with compensation logic.

### Saga Pattern

Sagas manage long-running business transactions that span multiple bounded contexts. In medical practice, sagas handle:

- **Prior authorization saga**: Submit request -> await payer response -> approve/deny -> notify provider -> update scheduling
- **Referral completion saga**: Create referral -> verify insurance network -> schedule specialist -> confirm appointment -> notify referring provider
- **Claim lifecycle saga**: Generate claim -> submit to payer -> track adjudication -> process payment/denial -> manage appeal if denied

Each saga step has a compensating action in case of failure, ensuring the system remains consistent even when individual steps fail.

## Event Infrastructure Considerations

### Event Ordering

Within a single aggregate, events must be processed in order. A PrescriptionIssued event must be processed before a PrescriptionRefilled event for the same prescription. Across aggregates, strict ordering is generally not required.

### Idempotency

Event consumers must handle receiving the same event more than once. Network failures and retries can cause duplicate delivery. Each consumer should check whether it has already processed a given event before applying changes.

### Event Schema Evolution

As the domain model evolves, event schemas change. Medical systems must support backward-compatible schema evolution because:

- Historical events must remain readable for regulatory compliance
- Multiple versions of consumers may be running during deployments
- Archived events may need to be replayed for audit or recovery

### Event Store vs. Message Broker

- **Event store**: Durable, ordered log of events that serves as the source of truth (useful for event-sourced aggregates and audit trails)
- **Message broker**: Publish-subscribe system for distributing events to consumers (useful for real-time notifications and cross-context communication)

Medical systems often use both: an event store for audit and compliance, and a message broker for real-time event distribution.

## Relationships to Other Topics

- [Domain Events](../domain-events/) -- The events that flow through the architecture
- [Bounded Contexts](../bounded-contexts/) -- The producers and consumers of events
- [Context Map](../context-map/) -- Shows which contexts publish and subscribe to which events
- [Integration Patterns](../integration-patterns/) -- EDA is complementary to other integration patterns
- [Regulatory Compliance](../regulatory-compliance/) -- Events provide the audit trail required by HIPAA
- [Repositories](../repositories/) -- Event-sourced repositories store events as the source of truth

## References

- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 8. Addison-Wesley, 2013.
- Kleppmann, Martin. "Designing Data-Intensive Applications." Chapters 11-12. O'Reilly, 2017.
- Richardson, Chris. "Microservices Patterns." Chapter 4: Managing Transactions with Sagas. Manning, 2018.
- Millett, Scott & Tune, Nick. "Patterns, Principles, and Practices of Domain-Driven Design." Chapter 20. Wrox, 2015.
- Hohpe, Gregor & Woolf, Bobby. "Enterprise Integration Patterns." Addison-Wesley, 2003.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 9. O'Reilly, 2021.
