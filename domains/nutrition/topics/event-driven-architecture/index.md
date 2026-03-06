# Event-Driven Architecture in the Nutrition Domain

## Overview

Event-driven architecture (EDA) is a design pattern in which bounded contexts communicate through domain events rather than direct synchronous calls. In the nutrition domain, EDA enables the six bounded contexts to remain loosely coupled while sharing information about significant occurrences. When a nutritional assessment is completed, the Assessment context publishes an event; the Clinical Nutrition, Sports Nutrition, and Outcomes Tracking contexts each react to that event independently, without the Assessment context needing to know who is listening or what they do with the information.

## Why Event-Driven Architecture for Nutrition

The nutrition domain benefits from EDA for several reasons. First, nutritional workflows are inherently asynchronous: an assessment triggers downstream processes that may take hours or days to complete. Second, the same event may be relevant to multiple contexts: a biomarker result matters to Clinical Nutrition for diagnosis, to Sports Nutrition for energy availability assessment, and to Outcomes Tracking for trend analysis. Third, contexts evolve independently: new consumers can subscribe to existing events without modifying the producing context.

## Event Flow Patterns

### Assessment-Triggered Flows

When the Nutritional Assessment context publishes AssessmentCompleted, three independent flows begin. Clinical Nutrition evaluates the assessment data for potential nutrition diagnoses. Sports Nutrition updates energy availability calculations if the subject is an athlete. Outcomes Tracking records the assessment data points in the subject's outcome timeline. These flows execute concurrently and independently.

### Prescription Cascade

When Clinical Nutrition publishes DietPrescriptionIssued, Meal Planning receives the event and adjusts or constrains the patient's meal plan to comply with the prescription. Once Meal Planning modifies the plan and publishes MealPlanModified, Dietary Compliance updates its adherence reference. This cascade flows through three contexts sequentially, with each context reacting to the event from the previous one.

### Compliance Feedback Loop

Dietary Compliance publishes ComplianceScoreCalculated at the end of each reporting period. Outcomes Tracking records the score as a trend data point. If the score is persistently low, Clinical Nutrition may adjust the therapy approach. If the score is high, Outcomes Tracking may correlate it with positive health trends. This creates a feedback loop where compliance data informs both outcome analysis and intervention adjustment.

### Risk Detection

When Sports Nutrition detects low energy availability and publishes REDSRiskDetected, Clinical Nutrition receives the event and may initiate a clinical evaluation. Outcomes Tracking flags the risk event in the athlete's timeline. This pattern demonstrates how EDA enables rapid cross-context response to safety-critical situations.

## Event Design Standards

### Event Structure

All domain events in the nutrition domain follow a standard structure: a unique event identifier, the event type name, a timestamp of occurrence, the aggregate root identifier from which the event originated, a correlation identifier for tracing related events across contexts, and the event payload containing domain-specific data.

### Event Naming

Events are named in past tense to reflect that the action has already occurred: AssessmentCompleted, DietPrescriptionIssued, ComplianceScoreCalculated. This convention makes it clear that consumers are reacting to facts, not requests.

### Event Payload

Events carry sufficient data for consumers to react without querying the producing context. The AssessmentCompleted event includes a summary of key findings, not just an assessment identifier. This reduces coupling by eliminating the need for consumers to call back into the producing context for additional data.

### Idempotency

Event consumers in the nutrition domain are designed to be idempotent: processing the same event twice produces the same result as processing it once. This is essential because event delivery mechanisms may deliver events more than once during failure recovery.

## Eventual Consistency

The nutrition domain embraces eventual consistency between bounded contexts. When an assessment is completed, the Clinical Nutrition context may not immediately reflect the new data. This is acceptable because nutrition workflows operate on clinical timescales (hours to days) where brief delays between contexts are not clinically significant. However, within each aggregate, strong consistency is maintained: a NutritionalAssessment is always internally consistent.

## Event Storage and Replay

Domain events in the nutrition domain may be stored as an event log, enabling event replay for debugging, auditing, and rebuilding read models. This is particularly valuable in the clinical context where audit trails of nutritional decisions are required for regulatory compliance.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 8 on domain events.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 11 on event-driven architecture.
- Stopford, Ben. Designing Event-Driven Systems. O'Reilly Media, 2018.
