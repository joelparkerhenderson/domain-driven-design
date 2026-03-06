# Event-Driven Architecture in Hospital Management

## Overview

Event-driven architecture (EDA) is a design approach where system components communicate by producing and consuming events rather than through direct synchronous calls. In hospital management, EDA enables bounded contexts to remain loosely coupled while reacting to clinical, administrative, and operational occurrences in real time.

## Relevance to Hospital Management

Hospital operations are inherently event-driven. Patients arrive, are triaged, get admitted, receive orders, get results, and are discharged. Each occurrence triggers downstream actions across multiple departments. EDA mirrors this reality in software:

- Admitting a patient notifies billing, nursing, pharmacy, and dietary
- A critical lab result alerts the attending physician immediately
- A bed becoming available triggers the next patient assignment from the waitlist
- A completed encounter initiates charge capture and claim generation

## Key Patterns

### Event Notification

The simplest EDA pattern. An event signals that something happened, carrying minimal data. Consumers query the source system for details if needed.

- **Use case**: PatientRegistered event with just the MRN — consumers call back to Patient Context for full demographics
- **Tradeoff**: Reduces event payload size but introduces coupling to the source API

### Event-Carried State Transfer

Events carry all data consumers need, eliminating the need for callbacks. Each consumer maintains its own local projection of the data.

- **Use case**: PatientUpdated event carries full demographics — Clinical, Billing, and Scheduling contexts update their local projections
- **Tradeoff**: Larger event payloads, eventual consistency, but full decoupling between contexts

### Event Sourcing

The aggregate's state is derived entirely from a sequence of events rather than being stored as a current snapshot. The event log is the source of truth.

- **Use case**: Patient allergy history — every AllergyRecorded, AllergyRemoved, and AllergyUpdated event is stored, enabling full audit trail
- **Use case**: Encounter history — every action taken during an encounter is recorded as an event, supporting regulatory audit requirements
- **Tradeoff**: Complex to implement, excellent for audit trails and temporal queries

### CQRS (Command Query Responsibility Segregation)

Separates the write model (commands that produce events) from the read model (optimized projections for queries). Events flow from the write side to build read-optimized views.

- **Use case**: Clinical dashboard — the write model processes orders and results as aggregates; the read model projects a flat, denormalized view optimized for the ED tracking board
- **Tradeoff**: Increased complexity, eventual consistency in read models, but dramatically better read performance

## Choreography vs. Orchestration

### Choreography

Each bounded context reacts independently to events without a central coordinator. The workflow emerges from individual context reactions.

- **Use case**: Patient admission — PatientAdmitted event is published; Billing opens an account, Nursing receives a notification, Pharmacy reviews medication orders, Dietary sends a menu — all independently
- **Strength**: Loose coupling, no single point of failure
- **Weakness**: Difficult to monitor the overall workflow; hard to detect when a step fails silently

### Orchestration

A central process (saga/process manager) coordinates the workflow by issuing commands and waiting for responses.

- **Use case**: Discharge process — the Discharge Saga coordinates: verify all results reviewed → reconcile medications → generate discharge instructions → confirm follow-up appointment → close encounter → generate final bill
- **Strength**: Explicit workflow visibility, easier error handling
- **Weakness**: Central coordinator is a coupling point

### Choosing Between Them

- Use **choreography** for loosely coupled, notification-style workflows (patient registration, appointment reminders)
- Use **orchestration** for multi-step processes where order matters and failures must be handled explicitly (discharge, surgery scheduling, claim submission)

## Saga Pattern

Sagas manage long-running processes that span multiple aggregates or bounded contexts, with compensating actions for failures.

### Admission Saga

1. Create inpatient encounter → success
2. Assign bed → success
3. Notify nursing → success
4. Open billing account → **failure**
5. Compensate: release bed, cancel encounter, notify nursing of cancellation

### Discharge Saga

1. Verify results reviewed → success
2. Reconcile medications → success
3. Generate discharge instructions → success
4. Schedule follow-up → success
5. Close encounter → success
6. Generate final bill → success

If step 2 fails (unreconciled medications), the saga pauses and alerts the provider rather than compensating all previous steps.

## Infrastructure Considerations

### Message Brokers

Hospital EDA typically uses a message broker for reliable event delivery:

- **Guaranteed delivery**: Events must not be lost (a lost lab result could harm a patient)
- **Ordering**: Events within an aggregate must be processed in order
- **Durability**: Events must survive system restarts
- **Replay**: The ability to replay events for debugging or rebuilding projections

### Idempotency

Consumers must handle duplicate events gracefully. Network issues may cause the same event to be delivered multiple times. Each consumer should use event IDs to detect and ignore duplicates.

## Relationships to Other Topics

- [Domain Events](../domain-events/) — The domain objects that flow through the architecture
- [Integration Patterns](../integration-patterns/) — EDA is the foundation for inter-context integration
- [Bounded Contexts](../bounded-contexts/) — EDA enables loose coupling between contexts
- [Aggregates](../aggregates/) — Aggregates produce events; event sourcing stores them
- [Regulatory Compliance](../regulatory-compliance/) — Event logs support HIPAA audit requirements

## References

- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 8: Domain Events. Addison-Wesley, 2013.
- Richardson, Chris. "Microservices Patterns." Chapter 4: Managing Transactions with Sagas. Manning, 2018.
- Hohpe, Gregor & Woolf, Bobby. "Enterprise Integration Patterns." Addison-Wesley, 2003.
- Fowler, Martin. "Event Sourcing." martinfowler.com, 2005.
- Fowler, Martin. "CQRS." martinfowler.com, 2011.
- Young, Greg. "CQRS and Event Sourcing." CQRS.nu.
