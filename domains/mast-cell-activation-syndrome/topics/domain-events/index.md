# Domain Events for Mast Cell Activation Syndrome

## Overview

Domain events represent significant occurrences within the domain that other parts of the system need to know about. In the MCAS domain, domain events are the primary mechanism for communication between bounded contexts. When something meaningful happens in one context, it publishes an event that other contexts can subscribe to and react upon.

Domain events enable loose coupling between bounded contexts. The publishing context does not need to know who subscribes to its events or what they do with the information. This decoupling allows each context to evolve independently while maintaining system-wide coherence.

## Diagnostic Assessment Events

### DiagnosticWorkupInitiated

Published when a new diagnostic workup begins for a patient. This event signals to other contexts that a patient is under evaluation. The Trigger Management Context may begin preliminary trigger screening, and the Symptom Tracking Context may initiate enhanced symptom monitoring to capture data relevant to the diagnostic evaluation.

Payload: workup identifier, patient identifier, ordering clinician identifier, date initiated, suspected condition.

### MediatorLevelRecorded

Published when a laboratory result for a mast cell mediator is received and recorded. This event carries the mediator type, the measured value, the baseline comparison, and whether the elevation meets consensus criteria thresholds.

Payload: workup identifier, patient identifier, mediator type, measured value, baseline value, elevation status, collection timestamp.

### DiagnosisConfirmed

Published when a diagnostic workup concludes with a confirmed MCAS diagnosis. This is a pivotal event that triggers actions across multiple contexts. The Trigger Management Context creates an initial trigger profile. The Medication Protocol Context initiates a treatment regimen. The Specialist Coordination Context begins assembling a care team.

Payload: workup identifier, patient identifier, diagnosis date, criteria met, diagnosing clinician identifier.

### DiagnosisRuledOut

Published when evaluation determines the patient does not meet MCAS diagnostic criteria. This event may trigger cleanup actions in contexts that began preliminary work during the evaluation period.

Payload: workup identifier, patient identifier, conclusion date, alternative considerations.

## Trigger Management Events

### TriggerIdentified

Published when a new trigger is confirmed for a patient. The Medication Protocol Context may adjust treatment to provide prophylaxis for the identified trigger type. The Outcomes Measurement Context records the trigger identification as a milestone in the management timeline.

Payload: trigger entry identifier, patient identifier, trigger substance or condition, trigger classification, confidence level, identification date.

### AvoidancePlanUpdated

Published when a patient's avoidance plan is modified to address newly identified triggers or to refine existing strategies. The Specialist Coordination Context updates the shared care plan to reflect the new avoidance recommendations.

Payload: avoidance plan identifier, patient identifier, changes made, effective date.

## Medication Protocol Events

### MedicationStarted

Published when a new medication is added to a patient's regimen. The Symptom Tracking Context may prompt enhanced monitoring to assess treatment response. The Outcomes Measurement Context begins tracking the medication's effectiveness.

Payload: medication entry identifier, patient identifier, medication name, dosage, frequency, prescribing clinician identifier, start date.

### MedicationTitrated

Published when a medication dosage is adjusted as part of a titration plan. The Outcomes Measurement Context correlates the dosage change with subsequent symptom changes.

Payload: medication entry identifier, patient identifier, previous dosage, new dosage, titration step number, adjustment date.

### MedicationDiscontinued

Published when a medication is removed from the regimen due to ineffectiveness, adverse reaction, or clinical decision. The Outcomes Measurement Context records the discontinuation reason and timing.

Payload: medication entry identifier, patient identifier, medication name, discontinuation reason, end date.

## Symptom Tracking Events

### SymptomRecorded

Published when a new symptom entry is logged. The Trigger Management Context evaluates the symptom against recent exposures to identify potential trigger correlations. Published frequently, so downstream consumers must handle high event volumes.

Payload: symptom entry identifier, patient identifier, organ system, symptom description, severity score, timestamp.

### FlareDocumented

Published when an acute exacerbation episode is recorded. This event carries richer information than individual symptom entries and triggers analysis in both the Trigger Management and Outcomes Measurement contexts.

Payload: flare record identifier, patient identifier, onset time, peak severity, affected organ systems, suspected triggers, interventions applied.

### FlareResolved

Published when a documented flare episode ends. The Outcomes Measurement Context uses the flare duration and resolution pattern to assess treatment effectiveness.

Payload: flare record identifier, patient identifier, resolution time, total duration, resolution method.

## Specialist Coordination Events

### ReferralRequested

Published when a referral to a specialist is initiated. The target context depends on the specialty involved.

Payload: referral identifier, patient identifier, referring provider identifier, target specialty, clinical reason, urgency level.

### CarePlanUpdated

Published when the shared care plan is modified. All clinical contexts receive this event to stay informed of changes to the coordinated management strategy.

Payload: care plan identifier, patient identifier, changes summary, updating provider identifier, update date.

## Outcomes Measurement Events

### OutcomesAssessmentCompleted

Published when a comprehensive outcomes assessment is finalized. The Specialist Coordination Context uses this event to inform the care team of the patient's current status.

Payload: assessment identifier, patient identifier, symptom burden score, quality of life score, functional status rating, assessment date.

## Event Design Principles

All MCAS domain events are immutable records of facts that occurred. They carry sufficient data for consumers to act without querying back to the source. Event schemas are versioned to support evolution without breaking existing consumers. Events are published asynchronously to avoid coupling the publishing context's transaction to downstream processing.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 8 on domain events.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 8 on domain events.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 6 on domain events and integration.
