# Event-Driven Architecture in Finance

## Overview

Event-driven architecture (EDA) is a design approach where bounded contexts communicate through domain events rather than direct synchronous calls. Events represent facts about things that have happened in the domain. Producers publish events without knowledge of consumers, and consumers react to events without knowledge of producers. This decoupling enables independent evolution, scalability, and resilience.

## Relevance to Finance

Financial systems are naturally event-driven. A payment is authorized, a trade is executed, a loan defaults, a suspicious transaction is detected -- each of these facts ripples across multiple bounded contexts, triggering downstream processing. EDA aligns with the fundamental nature of financial workflows and enables the high throughput, auditability, and regulatory traceability that financial institutions require.

## Key Concepts

### Event Types

- **Domain events**: Business-meaningful occurrences (PaymentSettled, TradeExecuted, LoanDisbursed)
- **Integration events**: Events specifically designed for cross-context communication, potentially with different schemas than internal domain events
- **Notification events**: Lightweight events that signal something happened, requiring the consumer to query for details
- **Event-carried state transfer**: Events that carry enough data for the consumer to act without querying the source

### Event Sourcing

Event sourcing stores the state of an aggregate as a sequence of domain events rather than current state. The current state is reconstructed by replaying events. This is particularly valuable in finance because:

- **Complete audit trail**: Every state change is recorded as an immutable event
- **Temporal queries**: Reconstruct the state of any aggregate at any point in time (critical for regulatory investigations)
- **Debugging and forensics**: Replay events to understand exactly what happened and when
- **Regulatory compliance**: Regulators can trace the complete history of any financial entity

### CQRS (Command Query Responsibility Segregation)

CQRS separates write operations (commands) from read operations (queries). In finance:

- **Write side**: Processes commands through aggregates, enforcing business rules and producing events
- **Read side**: Maintains optimized read models (projections) built from events for reporting, dashboards, and queries
- **Example**: The trading write model validates orders and executes trades; the read model provides real-time portfolio views and P&L calculations

### Choreography vs. Orchestration

**Choreography**: Each context reacts to events independently without a central coordinator.

- PaymentSettled -> Accounts & Ledger posts journal entries (independently)
- PaymentSettled -> Risk & Compliance updates transaction monitoring (independently)
- Best for loosely coupled, independently evolving contexts

**Orchestration**: A central process (saga or process manager) coordinates multi-step workflows.

- Loan origination saga: Application -> Credit Check -> Underwriting -> Approval -> Disbursement
- Trade settlement saga: Execution -> Allocation -> Clearing -> Settlement
- Best for complex, multi-step workflows that require coordination and compensation

### Sagas and Process Managers

Long-running business processes that span multiple aggregates or bounded contexts:

- **Payment processing saga**: Initiate -> Screen for fraud -> Check sanctions -> Validate funds -> Authorize -> Submit to clearing -> Settle
- **Loan origination saga**: Apply -> Verify identity -> Pull credit -> Underwrite -> Approve -> Close -> Disburse
- **Trade lifecycle saga**: Submit order -> Execute -> Allocate -> Clear -> Settle

Sagas handle compensation (rollback) when a step fails. If sanctions screening rejects a payment after authorization, the saga reverses the authorization hold.

## Finance-Specific Considerations

### Ordering Guarantees

Financial events often require strict ordering:

- Interest accrual events must be processed before end-of-day balance calculations
- Trade execution events must maintain sequence for regulatory audit trails
- Payment events must be processed in order to maintain correct running balances

### Exactly-Once Processing

Financial operations must not be duplicated:

- A payment must not be settled twice
- A journal entry must not be posted twice
- Idempotent event handlers and deduplication mechanisms are essential

### Event Versioning

As the domain model evolves, event schemas change:

- New fields may be added to events
- Field semantics may change
- Consumers must handle multiple event versions gracefully
- Schema registries help manage event version compatibility

## Relationships to Other Topics

- [Domain Events](../domain-events/) -- The building blocks of event-driven architecture
- [Aggregates](../aggregates/) -- Aggregates produce events when state changes
- [Integration Patterns](../integration-patterns/) -- EDA is the primary inter-context integration approach
- [Context Map](../context-map/) -- Event flows define the relationships on the context map
- [Domain Services](../domain-services/) -- Sagas and process managers are often implemented as domain services

## References

- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 8: Domain Events. Addison-Wesley, 2013.
- Vernon, Vaughn. "Domain-Driven Design Distilled." Chapter 6: Tactical Design with Domain Events. Addison-Wesley, 2016.
- Young, Greg. "CQRS and Event Sourcing." CQRS Documents, 2010.
- Kleppmann, Martin. "Designing Data-Intensive Applications." Chapter 11: Stream Processing. O'Reilly, 2017.
- Richardson, Chris. "Microservices Patterns." Chapter 4: Managing Transactions with Sagas. Manning, 2018.
- Fowler, Martin. "Event Sourcing." https://martinfowler.com/eaaDev/EventSourcing.html
