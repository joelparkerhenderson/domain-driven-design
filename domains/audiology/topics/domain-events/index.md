# Domain Events in the Audiology Domain

## Overview

Domain events represent significant occurrences within the audiology domain that the system needs to recognize and respond to. A domain event captures the fact that something important happened at a specific point in time. In the audiology domain, domain events drive workflows across bounded contexts, trigger downstream processes, and maintain an auditable record of clinical activities.

## Domain Event Characteristics

Domain events in the audiology domain are immutable records of past occurrences. They are named in past tense to reflect that the event has already happened. Each event carries a timestamp, the identity of the aggregate that produced it, and the relevant data needed by consumers. Events are published by one bounded context and may be consumed by one or more other bounded contexts.

## Hearing Assessment Context Events

### EvaluationCompleted

Published when an audiometric evaluation is finalized by the audiologist. This event carries the evaluation identifier, patient identifier, audiogram data, hearing loss classification, and clinical recommendations. This is the most consequential event in the audiology domain because it triggers activities in multiple downstream contexts: Device Management begins device selection, Rehabilitation assesses intervention needs, and Clinical Documentation generates the evaluation report.

### HearingLossDetected

Published when a diagnostic evaluation reveals a hearing loss that meets clinical significance criteria. This event carries the patient identifier, hearing loss type, degree, and configuration. It triggers referral workflows, counseling protocols, and potential device candidacy evaluation.

### ThresholdChangeDetected

Published when a follow-up evaluation reveals a significant change in hearing thresholds compared to previous evaluations. This event carries the patient identifier, previous and current thresholds, and the magnitude of change. It triggers device reprogramming considerations in Device Management and rehabilitation plan adjustments in the Rehabilitation Context.

### SpeechRecognitionEvaluationCompleted

Published when speech recognition testing is finalized. This event carries the patient identifier, speech recognition threshold, word recognition scores, and the presentation levels used. It informs device fitting decisions and rehabilitation planning.

## Device Management Context Events

### DeviceFittingCompleted

Published when a hearing device fitting session is finalized. This event carries the fitting identifier, device identifier, patient identifier, prescribed targets, actual settings, and verification measurements. It triggers documentation generation in the Clinical Documentation Context and informs the Rehabilitation Context about the patient's current amplification.

### DeviceVerificationFailed

Published when real ear measurement verification reveals that device output does not meet prescriptive targets within acceptable tolerances. This event triggers a refitting workflow within the Device Management Context and may generate an alert for the supervising audiologist.

### DeviceMaintenanceRequired

Published when a device reaches a maintenance milestone or when the patient reports a device malfunction. This event triggers scheduling workflows and updates the device's maintenance record.

### DeviceReturned

Published when a patient returns a hearing device within the trial period. This event triggers documentation updates, inventory management, and potential re-evaluation workflows.

## Rehabilitation Context Events

### RehabilitationPlanCreated

Published when a new rehabilitation plan is established for a patient. This event carries the plan identifier, patient identifier, goals, and planned interventions. It triggers documentation generation and may inform scheduling systems.

### OutcomeMeasurementRecorded

Published when a standardized outcome measure is administered and scored. This event carries the measurement identifier, instrument used, scores, and comparison to previous measurements. It informs clinical decision-making about plan effectiveness and may trigger plan modifications.

### RehabilitationGoalAchieved

Published when a patient meets a rehabilitation goal. This event triggers plan updates, documentation, and potential graduation from the rehabilitation program.

## Vestibular Services Context Events

### VestibularEvaluationCompleted

Published when a vestibular assessment is finalized. This event carries the evaluation identifier, patient identifier, test results, and vestibular diagnosis. It triggers rehabilitation program design if vestibular rehabilitation is indicated.

### FallRiskIdentified

Published when a vestibular evaluation or screening identifies elevated fall risk. This event carries the patient identifier and risk assessment details. It triggers safety counseling, referrals, and documentation.

## Pediatric Audiology Context Events

### NewbornScreeningReferred

Published when a newborn hearing screening produces a "refer" result. This event triggers the diagnostic follow-up workflow, family notification, and tracking to ensure timely diagnostic evaluation per Early Hearing Detection and Intervention (EHDI) guidelines.

### DevelopmentalMilestoneDelayed

Published when a pediatric patient misses an expected developmental milestone. This event triggers clinical review, potential reassessment, and family counseling.

### PediatricFittingCompleted

Published when a pediatric device fitting is finalized. This event carries additional pediatric-specific information such as electroacoustic verification results and family training completion status.

## Clinical Documentation Context Events

### ReportFinalized

Published when a clinical report is finalized and approved. This event triggers report distribution to referring providers, patient portals, and archival systems.

### ReferralCreated

Published when a referral is generated for external services. This event triggers referral tracking and follow-up workflows.

## Event Ordering and Consistency

Domain events in the audiology domain maintain causal ordering within an aggregate. An EvaluationCompleted event always follows the completion of all constituent tests. A DeviceFittingCompleted event always follows successful verification. Cross-context event ordering is managed through event timestamps and correlation identifiers that link related events across contexts.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley. Chapter 8 on domain events.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley. Chapter 8 on domain events.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media. Chapter 9 on domain events and integration.
