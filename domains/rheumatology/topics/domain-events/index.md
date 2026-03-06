# Domain Events in the Rheumatology Domain

## Overview

Domain events represent significant occurrences within the domain that other parts of the system need to know about. In the rheumatology domain, domain events capture clinically meaningful state changes such as a completed assessment, a detected disease flare, the initiation of biologic therapy, or the recording of an outcome measure. Domain events enable loose coupling between bounded contexts, allowing each context to react to changes in other contexts without direct dependencies.

## Clinical Assessment Context Events

### AssessmentCompleted

Published when a clinician finalizes a patient assessment including joint examination, serology results, and disease activity score calculation. This event carries the patient identifier, assessment date, clinician identifier, DAS28 or other composite score, tender and swollen joint counts, and key serology results. Consumers include the Autoimmune Disease Context (to evaluate disease activity against treatment targets), the Joint Disease Context (to update joint disease staging), and the Outcomes Tracking Context (to record the assessment as a longitudinal data point).

### SerologyResultReceived

Published when laboratory results for autoantibody or inflammatory marker tests become available. This event carries the patient identifier, test type, result value, positivity determination, and reference range. The Autoimmune Disease Context consumes this event to update classification criteria scoring and disease monitoring. The Clinical Assessment Context uses it internally to complete pending assessments.

### CriticalLabResultDetected

Published when a laboratory result falls outside safety parameters, such as a critically elevated CRP suggesting infection in an immunosuppressed patient. This event triggers urgent clinical review workflows. The Biologic Therapy Context may consume this event to evaluate whether therapy should be held.

## Autoimmune Disease Context Events

### DiseaseFlareDetected

Published when disease activity rises above a predefined threshold from a prior stable or remission state. This event carries the patient identifier, disease type, prior disease activity level, current disease activity level, the scoring instrument used, and the date of detection. Consumers include the Pain Management Context (to reassess pain interventions), the Biologic Therapy Context (to consider therapy escalation), and the Outcomes Tracking Context (to record the flare in the longitudinal record).

### RemissionAchieved

Published when a patient's disease activity falls below the remission threshold for a sustained period. This event carries the patient identifier, disease type, remission criteria met (ACR/EULAR Boolean, DAS28 below 2.6, CDAI at or below 2.8), and the date remission was determined. The Outcomes Tracking Context records this milestone. The Biologic Therapy Context may consider therapy tapering.

### TreatmentEscalated

Published when the disease management plan escalates therapy due to inadequate response. This event carries the patient identifier, disease type, prior therapy, new therapy, and clinical rationale. The Biologic Therapy Context consumes this event when escalation involves biologic agents.

## Joint Disease Context Events

### InjectionAdministered

Published when a joint injection is performed. This event carries the patient identifier, target joint, injection type (corticosteroid, hyaluronic acid), medications used, and procedure date. The Pain Management Context may use this to update pain management plans. The Outcomes Tracking Context records the intervention.

### GoutFlareOccurred

Published when a gout flare is documented. This event carries the patient identifier, affected joints, serum urate level, and treatment administered. The Pain Management Context assesses acute pain needs. The Joint Disease Context may adjust urate-lowering therapy.

## Biologic Therapy Context Events

### TherapyStarted

Published when a biologic or targeted synthetic DMARD therapy is initiated after pre-treatment screening is completed. This event carries the patient identifier, drug name, dose, route, frequency, indication, and start date. The Outcomes Tracking Context records the therapy initiation as a landmark event in the patient timeline.

### InfusionReactionOccurred

Published when an adverse reaction occurs during an infusion session. This event carries the patient identifier, drug, reaction severity grade, symptoms, interventions performed, and outcome. The Biologic Therapy Context uses this internally to adjust future infusion protocols. The Outcomes Tracking Context records the adverse event.

### BiosimilarSwitchCompleted

Published when a patient is transitioned from a reference biologic to a biosimilar or vice versa. This event carries the patient identifier, reference product, biosimilar product, switch date, and clinical rationale. The Outcomes Tracking Context monitors post-switch outcomes.

## Pain Management Context Events

### PainPlanCreated

Published when a new multimodal pain management plan is established. This event carries the patient identifier, pain assessment scores, pharmacologic interventions planned, therapy referrals made, and functional goals set. The Outcomes Tracking Context records the plan as a baseline for functional outcome measurement.

### TherapyProgressReported

Published when a physical or occupational therapy provider reports progress. This event carries the patient identifier, therapy type, sessions completed, functional improvements measured, and remaining goals. The Pain Management Context updates the plan accordingly.

## Outcomes Tracking Context Events

### OutcomeRecorded

Published when a validated outcome measure is recorded. This event carries the patient identifier, instrument used, score, date, and comparison with prior measurement (improved, stable, worsened). This event is primarily consumed within the Outcomes Tracking Context but may trigger alerts in other contexts when significant changes occur.

### TreatToTargetReviewDue

Published when a scheduled treat-to-target assessment interval has elapsed. This event notifies the Clinical Assessment Context that a new assessment should be scheduled and the Autoimmune Disease Context that a treatment plan review is due.

## Event Design Principles

Domain events in the rheumatology domain are immutable records of something that happened. They carry sufficient data for consumers to react without querying the source context. Events use past tense naming to indicate they represent completed actions. Events are published asynchronously to avoid temporal coupling between contexts.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 8.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 8.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 10.
- Brandolini, Alberto. "Introducing EventStorming." Leanpub, 2021.
