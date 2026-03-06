# Event-Driven Architecture in the Gerontology Domain

## Overview

Event-driven architecture (EDA) is the communication pattern used between bounded contexts in the gerontology domain. Rather than bounded contexts calling each other directly through synchronous interfaces, they communicate by publishing and subscribing to domain events. This approach achieves loose coupling between contexts while ensuring that clinically important changes propagate to all relevant areas of the system.

In geriatric care, events naturally model the clinical workflow: an assessment is completed, a cognitive decline is detected, a medication is changed, an advance directive is recorded. Each of these occurrences has consequences across multiple areas of care. Event-driven architecture captures these cause-and-effect relationships without creating direct dependencies between the contexts involved.

## Why EDA for Gerontology

Geriatric care is inherently event-driven. Clinical workflows proceed through a series of significant occurrences: a patient is screened, a risk is identified, an intervention is planned, a transition occurs, a preference is documented. Each occurrence triggers responses from different clinical teams and care processes. EDA mirrors this natural flow of clinical information.

EDA also supports the temporal decoupling necessary in geriatric care. When a comprehensive geriatric assessment is completed, the resulting care plan development, cognitive screening, medication review, and functional assessment do not all happen simultaneously. They are triggered asynchronously, proceeding at their own pace. EDA naturally accommodates this asynchronous workflow without requiring orchestration.

## Event Categories

### Assessment Events

Events originating from the Geriatric Assessment Context include AssessmentCompleted, FrailtyStatusChanged, FallRiskIdentified, and NutritionalDeficiencyDetected. These events carry assessment results that trigger downstream clinical processes. AssessmentCompleted is the most consequential event, as it initiates activity in nearly every other bounded context.

### Coordination Events

Events originating from the Care Coordination Context include CarePlanCreated, CareTransitionInitiated, CareTransitionCompleted, CaregiverBurdenAssessed, and CareGoalAchieved. These events signal changes in the overall care approach that other contexts must account for in their own planning.

### Clinical Change Events

Events originating from clinical contexts include CognitiveDeclineDetected, DementiaStageProgressed, FunctionalDeclineDetected, MedicationRegimenChanged, PolypharmacyReviewCompleted, and AdverseDrugEventReported. These events represent clinically significant changes that may trigger reassessment, care plan modification, or goals-of-care discussions.

### Planning Events

Events originating from the End of Life Planning Context include AdvanceDirectiveRecorded, GoalsOfCareConversationCompleted, HospiceReferralInitiated, and CodeStatusChanged. These events are unique because they constrain the behavior of all other contexts, establishing boundaries on acceptable treatment decisions.

## Event Flow Patterns

### Fan-Out Pattern

The AssessmentCompleted event demonstrates a fan-out pattern where a single event is consumed by multiple contexts. When a CGA is completed, Care Coordination initiates care planning, Cognitive Health may begin detailed screening, Functional Independence establishes baselines, and Medication Management initiates polypharmacy review. Each consumer processes the event independently.

### Chain Pattern

Some events trigger chains of responses. A CognitiveDeclineDetected event may lead to a MemoryCarePlanEstablished event, which in turn triggers a CarePlanUpdated event in Care Coordination. Each event in the chain is published by a different context, creating a cascading workflow without direct context-to-context coupling.

### Constraining Pattern

AdvanceDirectiveRecorded and GoalsOfCareConversationCompleted events constrain rather than trigger. When these events are received, contexts adjust their behavior to ensure compliance with the patient's documented preferences. A comfort-measures-only code status constrains the types of interventions the Medication Management and Care Coordination contexts will recommend.

## Event Design Principles

Events in the gerontology domain carry enough data for consumers to act without querying the publishing context. An AssessmentCompleted event includes summary findings, not just an assessment identifier. This reduces coupling and improves resilience when contexts are temporarily unavailable.

Events are immutable records of things that happened. They are never modified after publication. If an assessment result is corrected, a new AssessmentCorrected event is published rather than modifying the original AssessmentCompleted event.

Events use the ubiquitous language of the publishing context. The Cognitive Health Context publishes CognitiveDeclineDetected using cognitive health terminology. Consuming contexts translate event data into their own ubiquitous language through their anti-corruption layers.

## Eventual Consistency

EDA in the gerontology domain embraces eventual consistency between bounded contexts. When a medication change occurs, the care plan may not immediately reflect the change. Eventual consistency is acceptable because clinical workflows already account for propagation delays. Vernon (2013) describes this as a natural fit for domains where immediate consistency across all contexts is neither achievable nor necessary.

However, eventual consistency requires compensating actions when things go wrong. If a medication is added but the Beers screening identifies it as potentially inappropriate, the Medication Management Context publishes a PolypharmacyReviewCompleted event with recommendations, and the prescribing clinician takes corrective action.

## Infrastructure Independence

The domain model defines events in terms of their business meaning, not their transport mechanism. Whether events are delivered through a message queue, event stream, or in-process event bus is an infrastructure decision that does not affect the domain model. This separation ensures that the business logic remains pure and testable without infrastructure dependencies.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media.
