# Domain Events in the Psychology Domain

## Overview

A domain event is a record of something significant that happened within the domain. Domain events capture clinically meaningful occurrences that other parts of the system may need to know about. In the psychology domain, domain events drive workflows across bounded contexts, trigger regulatory documentation requirements, and enable loose coupling between clinical subsystems.

Domain events are named in the past tense because they describe something that has already occurred. They are immutable records of facts. Once an AssessmentCompleted event is raised, it cannot be undone -- it represents a historical truth that the assessment reached completion at a specific point in time.

## Events in the Psychological Assessment Context

AssessmentReferralReceived: Raised when a new referral for psychological evaluation is accepted. This event triggers the creation of an Assessment aggregate and initiates the test selection process. The event carries the referral source, the referral question, the client identifier, and the requested assessment type.

TestBatteryAdministered: Raised when all tests in an assessment battery have been administered. This event triggers the scoring workflow and notifies the supervising psychologist that raw data is ready for processing.

AssessmentCompleted: Raised when the full assessment cycle -- administration, scoring, interpretation, and report generation -- is finished. This event is consumed by the Therapeutic Intervention Context to inform treatment planning and by the Outcomes Measurement Context to establish baseline measures. The event carries the assessment identifier, the client identifier, diagnostic impressions, and a summary of key findings.

ValidityThreatDetected: Raised when test validity indicators suggest that assessment results may not accurately reflect the client's true functioning. This event triggers additional review procedures and may halt the scoring process until validity concerns are resolved.

## Events in the Therapeutic Intervention Context

TreatmentPlanActivated: Raised when a treatment plan is approved and treatment begins. This event notifies the Outcomes Measurement Context to begin tracking treatment progress and initiates the scheduling of therapy sessions.

SessionCompleted: Raised after each therapy session is documented. This event carries the session identifier, the session date, interventions used, and any outcome measures administered during the session. The Outcomes Measurement Context consumes this event to update progress tracking.

TreatmentGoalAchieved: Raised when a client meets the criteria for a specific treatment goal. This event triggers a review of the treatment plan and may lead to goal revision or treatment completion planning.

TreatmentEpisodeClosed: Raised when treatment is formally concluded, whether through successful completion, mutual agreement, or client discontinuation. This event triggers final outcome measurement and case closure documentation.

## Events in the Behavioral Analysis Context

FunctionalAssessmentCompleted: Raised when a functional behavior assessment has identified the environmental variables maintaining a target behavior. This event triggers the creation of a Behavior Intervention Plan and is consumed by the Therapeutic Intervention Context when behavioral findings inform treatment.

BehaviorThresholdReached: Raised when a target behavior reaches a predetermined criterion level, indicating either successful reduction of a problem behavior or successful increase of a replacement behavior. This event triggers plan review and potential modification.

DataCollectionPointRecorded: Raised each time behavioral data is recorded. This high-frequency event enables real-time progress monitoring and is consumed by the Outcomes Measurement Context for aggregate analysis.

## Events in the Cognitive Assessment Context

CognitiveScreeningCompleted: Raised when a cognitive screening has been administered and scored. If screening results suggest possible impairment, this event triggers a referral for comprehensive neuropsychological evaluation.

CognitiveProfileGenerated: Raised when a comprehensive cognitive evaluation produces a complete cognitive profile. This event is consumed by the Psychological Assessment Context for integration into broader assessment reports and by the Therapeutic Intervention Context for treatment planning.

## Events in the Research Methods Context

IRBApprovalGranted: Raised when an Institutional Review Board approves a research protocol. This event enables participant recruitment and data collection to begin.

ParticipantEnrolled: Raised when a participant completes informed consent and is enrolled in a study. This event initiates the participant's data collection schedule.

StudyDataCollectionCompleted: Raised when all planned data collection for a study is finished. This event triggers the analysis phase of the research workflow.

## Events in the Outcomes Measurement Context

ReliableChangeDetected: Raised when a client's outcome scores demonstrate statistically reliable change, whether improvement or deterioration. This event is consumed by the Therapeutic Intervention Context to inform treatment decisions.

ClinicalSignificanceAchieved: Raised when a client's scores cross from the clinical range into the normative range, indicating clinically meaningful recovery. This event triggers treatment completion consideration.

## Event Design Principles

Events in the psychology domain should carry sufficient context for consumers to act without querying back to the producing context. An AssessmentCompleted event should carry enough clinical summary information that the Therapeutic Intervention Context can begin treatment planning without requesting the full assessment record.

Events should not carry more information than consumers need. Raw test data, detailed scoring algorithms, and proprietary test content should remain within the Assessment Context. The event carries interpreted results, not raw data.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley. Chapter 8 on domain events.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley. Chapter 8 on domain events.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media. Chapter 8 on domain events as a communication mechanism.
