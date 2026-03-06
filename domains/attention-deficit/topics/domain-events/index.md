# Domain Events: Attention-Deficit Domain

Domain events represent significant occurrences within the ADHD management domain that other parts of the system need to know about. They capture the fact that something important happened at a specific point in time, enabling loosely coupled communication between bounded contexts.

## Purpose

ADHD management is inherently event-driven. A completed diagnostic assessment triggers treatment planning. A medication adjustment triggers intensified behavioral monitoring. A significant symptom change triggers treatment plan review. Domain events make these causal relationships explicit in the domain model, enabling bounded contexts to react to changes in other contexts without creating direct dependencies.

## Clinical Assessment Context Events

### AssessmentCompleted

Raised when a clinician finalizes a comprehensive ADHD assessment, including all rating scales, clinical interviews, and neuropsychological testing. Carries the patient identifier, the diagnostic impression (ADHD presentation type, severity, comorbidities), the assessment date, and the assessing clinician identifier. Triggers treatment planning initiation in the Treatment Management Context and medication evaluation in the Medication Management Context.

### DiagnosticImpressionRevised

Raised when a follow-up assessment results in a changed diagnosis, such as a presentation type reclassification or newly identified comorbidity. Carries the previous and updated diagnostic impressions. Triggers review of active treatment plans, medication regimens, and accommodation plans.

### RatingScaleAdministered

Raised when a standardized rating scale (Conners, SNAP-IV, Vanderbilt) is completed and scored. Carries the scale name, the informant type (parent, teacher, self), the subscale scores, and the clinical significance indicators. Consumed by the Behavioral Monitoring Context for trend analysis and the Outcomes Tracking Context for treatment response evaluation.

## Treatment Management Context Events

### TreatmentPlanCreated

Raised when a new multimodal treatment plan is established for a patient. Carries the plan identifier, the selected treatment modalities, the behavioral objectives, and the planned review date. Triggers monitoring setup in the Behavioral Monitoring Context and baseline measurement in the Outcomes Tracking Context.

### TreatmentModalityChanged

Raised when a treatment modality is added, removed, or modified within an active treatment plan. Carries the specific change details including the modality type, the reason for change, and the effective date. Triggers updated monitoring parameters and outcome measurement recalibration.

### TreatmentEpisodeConcluded

Raised when a treatment episode reaches completion, discontinuation, or transition. Carries the episode outcome summary, the duration, and the reason for conclusion. Triggers final outcome measurement and treatment effectiveness evaluation.

## Behavioral Monitoring Context Events

### SignificantSymptomChangeDetected

Raised when behavioral monitoring data reveals a clinically meaningful change in symptom frequency, severity, or pattern. Carries the symptom dimension (inattention, hyperactivity, impulsivity), the direction of change (improvement, worsening), the magnitude, and the time period. Triggers treatment plan review and potential medication adjustment evaluation.

### ExecutiveFunctionDeclineObserved

Raised when executive function monitoring data shows a measurable decline in working memory, cognitive flexibility, planning, or inhibitory control performance. Carries the specific executive function domain, the baseline and current performance levels, and the observation period. Triggers clinical reassessment consideration.

### BehaviorTargetAchieved

Raised when a monitored behavior reaches the target level specified in the treatment plan. Carries the target behavior, the achievement date, and the setting context. Triggers treatment goal review and potential plan modification.

## Educational Accommodation Context Events

### AccommodationPlanCreated

Raised when a new IEP or 504 plan is established for a student with ADHD. Carries the plan type, the eligibility category, the accommodation items, and the review schedule. Triggers monitoring setup for accommodation effectiveness tracking.

### AccommodationReviewCompleted

Raised when a scheduled IEP or 504 plan review is completed. Carries the review outcome (continued, modified, discontinued), any changes to accommodations, and the next review date. Triggers updated outcome measurement parameters.

### AccommodationEffectivenessReported

Raised when teacher or educator feedback indicates whether specific accommodations are improving the student's academic and behavioral functioning. Carries the accommodation type, the effectiveness rating, and supporting observations.

## Medication Management Context Events

### MedicationTrialInitiated

Raised when a new medication is prescribed for ADHD management. Carries the medication name, the starting dosage, the formulation type, the prescriber identifier, and the titration schedule. Triggers intensified behavioral monitoring and side effect tracking setup.

### DosageAdjusted

Raised when a medication dosage is increased, decreased, or the administration schedule is modified. Carries the previous and new dosage details, the reason for adjustment, and the expected review date. Triggers behavioral monitoring for treatment response and side effect observation.

### SideEffectReported

Raised when a medication side effect is observed and documented. Carries the side effect type, severity, onset timing, and impact on daily functioning. Triggers clinical review if severity exceeds threshold and outcome measurement update.

### MedicationDiscontinued

Raised when a medication is stopped, whether due to side effects, lack of efficacy, or treatment plan change. Carries the discontinuation reason, the taper schedule if applicable, and the date. Triggers treatment plan review and alternative intervention consideration.

## Outcomes Tracking Context Events

### OutcomeMeasurementCompleted

Raised when a standardized outcome measure is administered and scored. Carries the instrument name, the scores, the measurement timepoint, and the comparison to baseline. Consumed by the Treatment Management Context for treatment response evaluation.

### TreatmentResponseClassified

Raised when sufficient outcome data is available to classify the patient's treatment response as full response, partial response, or non-response. Carries the response classification, the supporting evidence, and the recommendation for treatment continuation or modification.

## Event Design Principles

**Events are immutable facts.** Once raised, a domain event cannot be modified. It records what happened, when, and the relevant data at that point in time.

**Events carry sufficient context.** Each event includes enough information for consumers to act without querying the source context.

**Events enable eventual consistency.** Cross-context consistency is achieved through event-driven reactions, not distributed transactions.

## References

- Evans, E. (2003). *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media.
