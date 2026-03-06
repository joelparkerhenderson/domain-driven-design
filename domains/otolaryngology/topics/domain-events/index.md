# Domain Events - Otolaryngology Domain

## Overview

Domain events represent significant occurrences within the domain that other parts of the system need to know about. They capture something that happened in the past and are named using past-tense verbs. In the otolaryngology domain, domain events enable communication between bounded contexts without creating direct dependencies, supporting the event-driven architecture that connects ENT subspecialty workflows.

## Domain Event Principles

Domain events in otolaryngology follow core principles:

- **Past tense naming**: Events describe what already happened ("EncounterCompleted," not "CompleteEncounter").
- **Immutability**: Once published, an event cannot be modified. If a correction is needed, a new compensating event is published.
- **Self-contained**: Each event carries all data needed for consumers to react, avoiding the need for callbacks to the originating context.
- **Clinically meaningful**: Events correspond to clinically significant moments, not technical state changes.

## Key Domain Events by Bounded Context

### Clinical Assessment Context (Publisher)

**EncounterCompleted**: Published when an ENT encounter is finalized and signed by the provider. Carries encounter ID, patient ID, provider ID, encounter date, primary diagnosis codes, and a summary of key findings. Consumed by all downstream contexts to trigger follow-up workflows.

**EndoscopyPerformed**: Published when a nasal or laryngeal endoscopy is completed. Carries endoscopy type, findings summary, and any identified pathology. Consumed by Voice Disorders (for laryngeal findings) and Allergy Management (for sinonasal findings).

**AudiometricScreeningCompleted**: Published when an initial hearing screening is completed in the general ENT clinic. Carries screening results and whether further audiological evaluation is recommended. Consumed by Hearing Services to initiate comprehensive evaluation.

**ImagingStudyReviewed**: Published when imaging results (CT, MRI, ultrasound) are reviewed and interpreted. Carries study type, findings summary, and clinical significance. Consumed by Surgical Management for pre-operative planning.

### Surgical Management Context (Publisher)

**SurgicalCaseScheduled**: Published when a surgical case is placed on the operative schedule. Carries case ID, procedure type, scheduled date, surgeon, and pre-operative requirements.

**SurgeryCompleted**: Published when a surgical procedure is finished. Carries case ID, procedure performed, operative findings, and post-operative plan summary. Consumed by Voice Disorders (after phonosurgery) and Hearing Services (after cochlear implant surgery or ear surgery).

**PathologyResultReceived**: Published when surgical pathology results are available. Carries case ID, specimen description, pathological diagnosis, and staging information if applicable.

**PostOperativeComplicationRecorded**: Published when a post-operative complication is documented. Carries case ID, complication type, severity, and management plan.

### Voice Disorders Context (Publisher)

**VoiceAssessmentCompleted**: Published when a comprehensive voice evaluation is completed. Carries assessment ID, patient ID, VHI score, laryngoscopic findings summary, and therapy recommendations.

**VoiceTherapyProtocolInitiated**: Published when a voice therapy program begins. Carries protocol ID, patient ID, therapy goals, and expected duration.

**VoiceTherapyOutcomeMeasured**: Published when a therapy outcome is formally measured. Carries pre-treatment and post-treatment scores, therapy duration, and outcome classification.

### Sleep Disorders Context (Publisher)

**SleepStudyCompleted**: Published when polysomnography results are interpreted. Carries study ID, patient ID, AHI, oxygen nadir, sleep architecture summary, and diagnosis.

**CPAPPrescribed**: Published when CPAP therapy is initiated. Carries prescription ID, patient ID, prescribed settings, and follow-up schedule.

**CPAPComplianceAssessed**: Published when CPAP adherence data is reviewed. Carries compliance metrics (hours of use, AHI on therapy), and whether the patient meets compliance thresholds.

**SleepSurgeryReferred**: Published when a patient is referred for surgical intervention after failing conservative therapy. Carries referral reason, AHI, CPAP trial details, and anatomical assessment.

### Allergy Management Context (Publisher)

**AllergyTestingCompleted**: Published when allergy testing results are finalized. Carries test type (skin prick, intradermal, serum IgE), allergen results, and sensitization summary.

**ImmunotherapyProtocolStarted**: Published when an immunotherapy protocol begins. Carries protocol ID, allergen formulation, dosing schedule, and safety monitoring plan.

**ImmunotherapyReactionRecorded**: Published when an adverse reaction to immunotherapy is documented. Carries reaction type, severity, treatment administered, and protocol modification.

**SinusitisSurgeryReferred**: Published when chronic rhinosinusitis is referred for surgical management after medical therapy failure. Carries referral reason, symptom duration, failed treatments, and CT staging.

### Hearing Services Context (Publisher)

**HearingEvaluationCompleted**: Published when a comprehensive audiological evaluation is finished. Carries evaluation ID, audiogram data, speech recognition scores, and recommendations.

**HearingAidFitted**: Published when a hearing aid is fitted and verified. Carries fitting ID, device details, verification results, and patient satisfaction measures.

**CochlearImplantCandidacyDetermined**: Published when cochlear implant candidacy evaluation is completed. Carries candidacy determination (candidate, not a candidate), audiological criteria met, and recommendation.

**AuralRehabilitationMilestoneReached**: Published when a patient achieves a rehabilitation milestone. Carries milestone type, performance metrics, and next rehabilitation goals.

## Event Consumption Patterns

Events are consumed asynchronously. The Surgical Management Context subscribes to "SleepSurgeryReferred" to initiate pre-operative evaluation workflows. Hearing Services subscribes to "SurgeryCompleted" for cochlear implant cases to begin post-operative activation planning. Each consumer processes events according to its own bounded context rules.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 8 on domain events.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 8 on domain events.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 7 on domain events and event-driven architecture.
