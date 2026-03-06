# Event-Driven Architecture: Attention-Deficit Domain

Event-driven architecture enables asynchronous, loosely coupled communication between the bounded contexts of the ADHD management domain. Domain events propagate significant occurrences across context boundaries, allowing each context to react to changes in other parts of the system without creating direct dependencies.

## Purpose

ADHD management is inherently reactive. A diagnostic assessment triggers treatment planning. A medication adjustment triggers intensified monitoring. A significant behavioral change triggers treatment plan review. An accommodation effectiveness report triggers outcome measurement. These causal chains span multiple bounded contexts, and event-driven architecture makes them explicit, traceable, and maintainable without creating tight coupling between the clinical, educational, therapeutic, and pharmacological systems.

## Event Flow Patterns

### Assessment-to-Treatment Flow

When the Clinical Assessment Context completes a comprehensive evaluation, the AssessmentCompleted event is published. The Treatment Management Context subscribes to this event and initiates treatment plan creation based on the diagnostic impression, severity level, and comorbidity profile carried in the event payload. The Medication Management Context also subscribes, initiating medication evaluation when the diagnosis warrants pharmacological treatment. This pattern ensures that treatment planning begins automatically upon assessment completion without the assessment context needing to know about treatment or medication systems.

### Medication-to-Monitoring Flow

When the Medication Management Context adjusts a dosage, the DosageAdjusted event is published. The Behavioral Monitoring Context subscribes to this event and intensifies symptom tracking for the period following the adjustment, enabling clinicians to correlate medication changes with behavioral responses. The Outcomes Tracking Context also subscribes, recording the medication change as a factor in its longitudinal outcome analysis. This pattern ensures that behavioral data collection adapts automatically to medication changes.

### Monitoring-to-Treatment Flow

When the Behavioral Monitoring Context detects a significant symptom change, the SignificantSymptomChangeDetected event is published. The Treatment Management Context subscribes and triggers a treatment plan review, evaluating whether the current intervention approach needs modification. This pattern creates a feedback loop where behavioral data drives treatment adaptation without the monitoring context making treatment decisions.

### Accommodation-to-Outcomes Flow

When the Educational Accommodation Context completes a plan review, the AccommodationReviewCompleted event is published. The Outcomes Tracking Context subscribes and incorporates the review outcome into its longitudinal effectiveness analysis. This pattern ensures that educational intervention effectiveness is captured in the overall treatment outcome picture.

## Event Categories

### Command-Triggering Events

Events that trigger specific actions in subscribing contexts:

- AssessmentCompleted triggers TreatmentPlanCreation command in Treatment Management.
- MedicationTrialInitiated triggers MonitoringIntensification command in Behavioral Monitoring.
- SignificantSymptomChangeDetected triggers TreatmentPlanReview command in Treatment Management.

### Informational Events

Events that provide data for analysis without triggering immediate action:

- RatingScaleAdministered provides trend data to Behavioral Monitoring and Outcomes Tracking.
- SideEffectReported provides pharmacovigilance data to Outcomes Tracking.
- AccommodationEffectivenessReported provides school-based outcome data to Outcomes Tracking.

### Lifecycle Events

Events marking significant state transitions:

- TreatmentEpisodeConcluded marks the end of a treatment period, triggering final outcome evaluation.
- MedicationDiscontinued marks the end of a medication trial, triggering alternative treatment consideration.
- AccommodationPlanCreated marks the establishment of educational supports, triggering monitoring setup.

## Event Design Considerations

### Event Payload Design

Each event carries sufficient context for consumers to act without querying the source context. For example, the AssessmentCompleted event includes the patient identifier, diagnostic impression summary, ADHD presentation type, severity level, comorbidity list, and assessment date. This self-contained payload eliminates the need for synchronous callbacks to the Clinical Assessment Context.

### Event Ordering and Idempotency

ADHD management events must be processed in the correct order (a DosageAdjusted event must be processed after the corresponding MedicationTrialInitiated event) and handlers must be idempotent (processing the same event twice must not create duplicate records or trigger duplicate actions).

### Event Versioning

As the ADHD management domain model evolves, event schemas change. Event versioning ensures that older events remain processable and that consumers can handle both old and new event formats during transition periods. This is important as ADHD clinical guidelines are updated and new assessment instruments or treatment modalities are incorporated.

### Event Store and Audit Trail

All domain events are persisted in an event store, providing a complete audit trail of significant occurrences across the ADHD management domain. This audit trail is essential for clinical review, regulatory compliance, and retrospective outcome analysis. The event store enables reconstruction of the state of any aggregate at any point in time.

## Privacy and Security Considerations

ADHD management events carry protected health information (PHI) subject to HIPAA regulations and educational records subject to FERPA protections. Event-driven architecture must ensure that event payloads are encrypted in transit and at rest, access to event streams is controlled by role-based authorization, and event data is retained and disposed of in accordance with applicable regulations.

## References

- Evans, E. (2003). *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media.
