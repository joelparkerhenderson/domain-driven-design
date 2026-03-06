# Domain Events: Autism Domain

Domain events represent significant occurrences within the autism spectrum management domain that other parts of the system need to know about, enabling reactive behavior and cross-context communication.

## Purpose

In the autism domain, activities in one bounded context frequently trigger required actions in other contexts. When a diagnostic evaluation is completed, the behavioral intervention, sensory processing, communication support, and educational planning contexts all need to respond. Domain events make these cross-context dependencies explicit, enabling loosely coupled communication between contexts without direct model dependencies.

## Diagnostic Assessment Context Events

### ScreeningCompleted

Raised when a screening instrument (M-CHAT-R/F or similar tool) has been fully scored. Payload includes individual identifier, screening instrument name, screening date, result classification (low risk, medium risk, high risk), and recommended follow-up action. Consumers: Diagnostic Assessment Context (to schedule full evaluation if indicated), Outcomes Tracking Context (to record screening milestone).

### DiagnosticEvaluationInitiated

Raised when a formal diagnostic evaluation process begins. Payload includes individual identifier, evaluation identifier, referral source, evaluation type (initial or re-evaluation), and scheduled assessment dates. Consumers: all contexts that may need to prepare for the new individual entering the system.

### DiagnosisConfirmed

Raised when a formal autism spectrum disorder diagnosis has been determined. Payload includes individual identifier, DSM-5 diagnostic code, severity levels for social communication and restricted/repetitive behaviors, co-occurring conditions, and diagnosing clinician identifier. This is a pivotal event that triggers downstream actions in nearly all other contexts. Consumers: Behavioral Intervention Context (to initiate intervention planning), Sensory Processing Context (to schedule sensory profile assessment), Communication Support Context (to schedule communication assessment), Educational Planning Context (to initiate IEP eligibility determination), Outcomes Tracking Context (to establish baselines).

### DiagnosticReportFinalized

Raised when the written diagnostic report has been completed and approved. Payload includes individual identifier, evaluation identifier, report date, and summary of findings and recommendations. Consumers: Educational Planning Context (for eligibility documentation), Outcomes Tracking Context (for record keeping).

## Behavioral Intervention Context Events

### InterventionPlanCreated

Raised when a new behavior intervention plan (BIP) has been developed. Payload includes individual identifier, plan identifier, target behaviors, replacement behaviors, and supervising BCBA identifier. Consumers: Educational Planning Context (to align classroom behavior support), Outcomes Tracking Context (to set intervention baselines).

### SkillMasteryAchieved

Raised when an individual meets mastery criteria for a skill acquisition program. Payload includes individual identifier, program identifier, skill name, mastery date, and mastery criteria met. Consumers: Outcomes Tracking Context (to record milestone), Educational Planning Context (to update IEP goal progress).

### BehaviorIncidentRecorded

Raised when a significant behavioral incident occurs during a therapy session. Payload includes individual identifier, session identifier, incident type, antecedent, behavior description, consequence applied, and severity rating. Consumers: Sensory Processing Context (if the incident has a sensory antecedent), Outcomes Tracking Context (for trend analysis).

### InterventionPlanModified

Raised when an existing BIP is revised based on data review. Payload includes individual identifier, plan identifier, modification date, changes made, and clinical rationale. Consumers: Educational Planning Context (to update aligned classroom strategies), Outcomes Tracking Context (to note intervention changes in outcome analysis).

## Sensory Processing Context Events

### SensoryProfileCompleted

Raised when a comprehensive sensory profile assessment has been completed. Payload includes individual identifier, profile identifier, assessment date, quadrant scores, and areas of concern. Consumers: Behavioral Intervention Context (to inform antecedent strategies), Communication Support Context (to inform AAC environment setup), Educational Planning Context (to inform classroom accommodations).

### SensoryDietUpdated

Raised when a sensory diet plan is created or revised. Payload includes individual identifier, diet identifier, update date, activities added or removed, and target sensory systems. Consumers: Educational Planning Context (for classroom sensory activity scheduling), Outcomes Tracking Context (to track sensory intervention changes).

### EnvironmentalModificationRecommended

Raised when a new environmental modification is recommended based on sensory assessment. Payload includes individual identifier, environment identifier, modification type, and implementation instructions. Consumers: Educational Planning Context (for classroom modifications).

## Communication Support Context Events

### CommunicationAssessmentCompleted

Raised when a communication evaluation has been completed. Payload includes individual identifier, assessment identifier, assessment date, communication modality findings, and AAC candidacy determination. Consumers: Behavioral Intervention Context (to inform FCT goals), Educational Planning Context (to inform IEP communication goals), Outcomes Tracking Context (to establish communication baselines).

### AACDeviceAssigned

Raised when an AAC device or system has been selected and configured for an individual. Payload includes individual identifier, device type, access method, initial vocabulary set, and trial period dates. Consumers: Educational Planning Context (to arrange device availability at school), Outcomes Tracking Context (to track AAC usage metrics).

### CommunicationGoalMet

Raised when a speech-language therapy goal has been achieved. Payload includes individual identifier, goal identifier, achievement date, and evidence summary. Consumers: Educational Planning Context (to update IEP goal progress), Outcomes Tracking Context (to record communication milestone).

## Educational Planning Context Events

### IEPCreated

Raised when a new IEP has been developed and signed. Payload includes individual identifier, IEP identifier, effective date, annual goals, related services, and placement determination. Consumers: all intervention contexts (to align therapy goals with IEP goals), Outcomes Tracking Context (to track educational milestones).

### IEPReviewed

Raised when an annual or interim IEP review has been completed. Payload includes individual identifier, IEP identifier, review date, goal progress summaries, and any changes to services or placement. Consumers: all intervention contexts, Outcomes Tracking Context.

### TransitionPlanActivated

Raised when transition planning begins for a student approaching age 16. Payload includes individual identifier, transition plan identifier, post-secondary goal areas, and planned transition assessments. Consumers: Outcomes Tracking Context (to begin tracking transition readiness metrics).

## Outcomes Tracking Context Events

### OutcomeMeasureRecorded

Raised when a periodic outcome measurement has been completed. Payload includes individual identifier, measurement period, outcome domains measured, and summary scores. Consumers: all intervention contexts (to inform ongoing treatment decisions).

### ProgressReportGenerated

Raised when a progress report has been compiled across all active interventions. Payload includes individual identifier, reporting period, progress summaries by domain, and overall trend indicators. Consumers: Diagnostic Assessment Context (to inform re-evaluation decisions), Educational Planning Context (to inform IEP review).

## Event Design Principles

- Events are immutable records of something that has already happened.
- Events carry sufficient data for consumers to act without querying the source context.
- Event names use past tense to indicate completed occurrences.
- Events include a timestamp and the identity of the originating context.

## References

- Evans, E. (2003). *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley. Chapter 8 on domain events.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley. Chapter 8 on domain events and event-driven architecture.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media. Chapter 10 on domain events and integration.
