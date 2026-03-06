# Event-Driven Architecture - Asthma Domain

## Overview

Event-driven architecture (EDA) is the communication paradigm that enables bounded contexts in the asthma management domain to interact while maintaining loose coupling and autonomy. Rather than direct synchronous calls between contexts, bounded contexts communicate by publishing and subscribing to domain events. This approach mirrors the clinical reality where asthma management involves multiple specialists and systems reacting to changes in patient status.

In the asthma domain, EDA connects the six bounded contexts -- Clinical Assessment, Treatment Management, Trigger Monitoring, Action Planning, Medication Management, and Outcomes Tracking -- through a network of domain events that propagate clinical state changes, treatment decisions, environmental alerts, and outcome measurements.

## Event-Driven Principles

**Temporal Decoupling.** The publisher of an event does not need the consumer to be available at the time of publication. When the Clinical Assessment Context publishes an AssessmentFinalized event, it does not wait for the Treatment Management Context to process it. This decoupling is critical in healthcare systems where different subsystems may have different availability requirements.

**Eventual Consistency.** Cross-context data consistency is achieved eventually through event processing, not immediately through distributed transactions. When a treatment plan step changes, the medication prescriptions and action plan instructions will be updated after the event is processed, not within the same transaction. The acceptable delay depends on clinical urgency -- medication safety rules may require near-real-time processing, while outcome trend updates can tolerate minutes or hours of delay.

**Event Sourcing Consideration.** The asthma domain benefits from event sourcing for audit-critical contexts. Clinical Assessment and Treatment Management may use event sourcing to maintain a complete, immutable history of all clinical decisions and their rationale. This supports regulatory compliance, clinical audit, and malpractice defense requirements.

## Event Categories

### Clinical State Change Events

These events represent changes in a patient's clinical status and drive clinical workflow reactions.

- **AssessmentFinalized:** Core clinical event that triggers treatment review and action plan evaluation.
- **SeverityReclassified:** Triggers comprehensive treatment plan and action plan revision.
- **ControlLevelChanged:** Signals that the patient's asthma control has shifted, potentially requiring intervention.
- **SpirometrySessionCompleted:** Provides objective lung function data for trending and decision-making.

### Treatment Decision Events

These events represent therapeutic decisions that must be reflected across medication, action planning, and outcome tracking.

- **TreatmentPlanSteppedUp:** Escalation requires new prescriptions and updated action plan instructions.
- **TreatmentPlanSteppedDown:** De-escalation requires prescription discontinuation and action plan revision.
- **BiologicTherapyInitiated:** Specialty therapy requires specialty pharmacy coordination and response tracking.
- **BiologicResponseAssessed:** Response evaluation may trigger therapy continuation, switch, or discontinuation.

### Environmental Alert Events

These events represent environmental conditions that may affect patient health and require proactive responses.

- **HighRiskExposureDetected:** Real-time alert that current environmental conditions pose elevated risk for a specific patient.
- **TriggerProfileUpdated:** Changes in known triggers affect avoidance recommendations and treatment considerations.

### Medication and Adherence Events

These events represent medication-related occurrences that inform clinical decision-making.

- **AdherenceAlertRaised:** Non-adherence patterns that must be considered before treatment escalation.
- **PrescriptionFilled:** Pharmacy dispensing confirmation for adherence tracking.
- **ExcessiveRescueUseDetected:** Elevated rescue inhaler use signaling inadequate control.

### Outcome Measurement Events

These events represent outcome data points and trend analyses that close the clinical feedback loop.

- **ACTScoreRecorded:** Patient-reported control assessment driving treatment review.
- **ExacerbationRecorded:** Clinically significant event requiring root cause analysis and treatment response.
- **OutcomeTrendAlertRaised:** Longitudinal pattern detection triggering proactive clinical review.

## Event Flow Patterns

### Assessment-to-Treatment Flow

1. Clinical Assessment publishes AssessmentFinalized.
2. Treatment Management consumes the event and evaluates step change criteria.
3. If step change is warranted, Treatment Management publishes TreatmentPlanSteppedUp or TreatmentPlanSteppedDown.
4. Medication Management consumes the treatment change event and adjusts prescriptions.
5. Action Planning consumes the treatment change event and updates medication instructions.

### Adherence Feedback Loop

1. Medication Management detects declining adherence and publishes AdherenceAlertRaised.
2. Treatment Management consumes the alert and flags that poor control may be adherence-related rather than therapy-inadequacy.
3. Outcomes Tracking consumes the alert and records the adherence data point for correlation analysis.
4. Action Planning may consume the alert to trigger patient outreach or education.

### Environmental Alert Flow

1. Trigger Monitoring detects high-risk conditions and publishes HighRiskExposureDetected.
2. Action Planning consumes the alert and notifies the patient of precautionary measures.
3. Outcomes Tracking records the exposure event for future trigger-exacerbation correlation analysis.

### Outcome Feedback Flow

1. Outcomes Tracking detects declining trend and publishes OutcomeTrendAlertRaised.
2. Treatment Management consumes the alert and initiates proactive treatment plan review.
3. Clinical Assessment may schedule an assessment based on the outcome alert.

## Event Delivery Guarantees

**At-Least-Once Delivery.** Events are guaranteed to be delivered at least once to each subscriber. Consumers must be idempotent -- processing the same event twice must produce the same result as processing it once. This is achieved through event ID deduplication.

**Ordered Delivery Within Context.** Events from a single bounded context for a single patient are delivered in order. AssessmentFinalized always precedes SeverityReclassified for the same assessment session. Cross-context ordering is not guaranteed.

**Dead Letter Handling.** Events that cannot be processed after retry attempts are routed to a dead letter queue for manual investigation. Clinical events in the dead letter queue trigger operational alerts due to patient safety implications.

## Event Schema Evolution

As the asthma domain model evolves, event schemas must change without breaking consumers. The domain follows these conventions:

- New optional fields can be added to events without version increment.
- Removing fields or changing field semantics requires a new event version.
- Consumers must tolerate unknown fields (forward compatibility).
- Event versioning uses semantic versioning (major.minor.patch).

## Monitoring and Observability

- Event processing latency is monitored with alerts for delays exceeding clinical SLA thresholds.
- Event flow dashboards visualize the rate and distribution of events across bounded contexts.
- Failed event processing is tracked and alerted with priority based on clinical impact.
- End-to-end event chain tracing (correlation IDs) enables debugging of multi-context clinical workflows.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 8 on domain events and event-driven architecture.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 11 on event-driven architecture.
- Stopford, Ben. Designing Event-Driven Systems. O'Reilly Media, 2018.
