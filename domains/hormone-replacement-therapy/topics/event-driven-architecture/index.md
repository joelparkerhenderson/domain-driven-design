# Event-Driven Architecture in the Hormone Replacement Therapy Domain

Event-driven architecture (EDA) is the communication paradigm that enables loosely coupled interaction between bounded contexts in the hormone replacement therapy (HRT) domain. Rather than contexts directly calling each other through synchronous interfaces, they communicate by publishing and subscribing to domain events that represent significant occurrences within the treatment lifecycle.

## Purpose

The HRT domain involves a clinical workflow where actions in one context trigger responses in multiple other contexts. When a lab result exceeds a therapeutic threshold, both the Side Effect Management Context and the Treatment Optimization Context need to respond, but in different ways and potentially at different times. Event-driven architecture enables this pattern by allowing the Lab Monitoring Context to publish a ThresholdExceeded event without knowing which contexts will consume it, preserving autonomy while enabling clinical workflow coordination.

## Event Flow Patterns

### Assessment-to-Protocol Flow

The clinical workflow begins with the Clinical Assessment Context publishing an AssessmentCompleted event. The Hormone Protocol Context subscribes to this event to initiate protocol selection. The Side Effect Management Context also subscribes to establish baseline risk profiles for the newly assessed patient. This fan-out pattern demonstrates how a single event can trigger multiple independent downstream processes.

### Protocol-to-Monitoring Flow

When the Hormone Protocol Context publishes a ProtocolPrescribed event, the Lab Monitoring Context subscribes to generate the initial monitoring plan. The Side Effect Management Context subscribes to establish treatment-specific adverse event monitoring. The Outcomes Tracking Context subscribes to record the treatment start point. Each consuming context processes the event according to its own domain logic.

### Monitoring-to-Optimization Flow

The Lab Monitoring Context publishes LabResultRecorded events for every captured result and ThresholdExceeded events when results fall outside therapeutic ranges. The Treatment Optimization Context subscribes to LabResultRecorded for trend analysis data collection and to ThresholdExceeded for urgent optimization consideration. The Side Effect Management Context subscribes to ThresholdExceeded for potential adverse event investigation.

### Optimization Feedback Loop

The Treatment Optimization Context publishes OptimizationRecommended events that the Hormone Protocol Context consumes for prescriber review. If accepted, the Protocol Context publishes a ProtocolModified event, which triggers Lab Monitoring schedule adjustments and Outcomes Tracking time point recording. This creates a continuous improvement loop driven entirely by domain events.

### Outcomes-to-Optimization Loop

The Outcomes Tracking Context publishes OutcomeAssessed events that the Treatment Optimization Context consumes. These events carry efficacy data that informs whether the current protocol is achieving its goals or whether adjustment is warranted. This loop closes the evidence-based treatment cycle.

## Event Categories

### Command Events

Some events carry imperative intent, indicating that an action should be taken. ProtocolPrescribed implies that monitoring should begin. ContraindicationDetected implies that the protocol should be reviewed. These events create expected responses in consuming contexts, though the consuming context retains authority over how it responds.

### Notification Events

Some events are purely informational, notifying interested contexts about changes without implying specific action. LabResultRecorded notifies that a new data point is available. MonitoringScheduleAdjusted notifies that monitoring parameters changed. Consuming contexts determine independently whether these notifications warrant action.

### Signal Events

Some events represent clinical signals that require evaluation by consuming contexts. ThresholdExceeded signals a potential safety concern. RiskLevelChanged signals a shift in patient risk profile. SideEffectReported signals a clinical complication. These events trigger evaluation workflows in consuming contexts.

## Event Payload Design

Events in the HRT domain carry self-contained payloads that include sufficient data for consuming contexts to process without querying back to the producing context. Payloads include the event identifier, timestamp of occurrence, correlation identifier linking related events across contexts, the producing context identifier, and domain-specific data relevant to the event type. Payload schemas are versioned to support consumer evolution.

## Ordering and Consistency

Clinical events often have ordering dependencies. An OptimizationRecommended event is only meaningful after the LabResultRecorded events that informed it. The event infrastructure preserves ordering within a single aggregate stream (all events for a specific MonitoringPlan arrive in order) while allowing parallel processing across different aggregates. Eventual consistency is the accepted model, with bounded contexts designing for the possibility of receiving events in unexpected orders.

## Idempotency

Event consumers are designed for idempotent processing. If a LabResultRecorded event is delivered twice (due to infrastructure retries), the consuming context produces the same result as processing it once. This is achieved through event deduplication based on event identifiers and through consumer-side checks that detect already-processed events.

## Error Handling

When a consuming context fails to process an event, the event is retained for retry. Dead letter handling captures events that repeatedly fail processing for investigation. Clinical urgency determines retry strategies: critical safety events (ContraindicationDetected, ThresholdExceeded with critical severity) receive aggressive retry with alert escalation, while informational events may tolerate longer retry intervals.

## Benefits for the HRT Domain

Event-driven architecture provides temporal decoupling, allowing contexts to process events asynchronously according to their own capacity and priority. It enables extensibility, allowing new contexts to subscribe to existing events without modifying producing contexts. It supports auditability, as the event stream provides a complete record of domain activities. It facilitates scalability, as high-volume contexts (Lab Monitoring) can publish events without being constrained by consumer processing speed.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 8 on domain events.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 11 on event-driven architecture.
- Stopford, Ben. Designing Event-Driven Systems. O'Reilly Media, 2018.
- Brandolini, Alberto. "Introducing EventStorming." Leanpub, 2021.
