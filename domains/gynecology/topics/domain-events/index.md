# Domain Events in the Gynecology Domain

## Overview

A domain event represents a significant occurrence within the domain that other parts of the system need to know about. Domain events capture the fact that something meaningful happened, expressed in the ubiquitous language. In the gynecology domain, domain events are the primary mechanism for coordinating actions across bounded contexts without creating tight coupling between them.

## Purpose

Gynecological workflows frequently cross bounded context boundaries. A clinical assessment may result in a surgical referral. An abnormal screening result may trigger a diagnostic workup. A prenatal risk identification may require specialist consultation. Domain events enable these cross-context workflows by allowing each context to publish what happened without needing to know what other contexts will do in response.

## Key Domain Events by Bounded Context

### Clinical Assessment Context Events

**DiagnosticImpressionFormed**
- Published when: A clinician finalizes the diagnostic impression for a clinical encounter.
- Contains: Encounter ID, patient ID, diagnosis codes, severity assessment, recommended next steps.
- Consumed by: Reproductive Health Context (to update health plans), Surgical Services Context (if surgery is recommended), Patient Education Context (to trigger relevant education).

**ClinicalReferralCreated**
- Published when: A clinical encounter results in a referral to another service.
- Contains: Encounter ID, patient ID, referral type, referral reason, urgency level.
- Consumed by: Surgical Services Context, Prenatal Care Context, or Screening Programs Context depending on referral type.

### Reproductive Health Context Events

**ReproductiveHealthPlanUpdated**
- Published when: A patient's reproductive health plan is materially changed.
- Contains: Plan ID, patient ID, change type (goal change, contraception change, treatment change), new plan summary.
- Consumed by: Clinical Assessment Context (to inform future encounters), Patient Education Context (to provide updated guidance).

**FertilityAssessmentCompleted**
- Published when: A fertility evaluation is finalized.
- Contains: Assessment ID, patient ID, findings summary, recommended interventions.
- Consumed by: Patient Education Context (to deliver fertility education materials).

### Surgical Services Context Events

**SurgeryScheduled**
- Published when: A surgical case has consent documented and a date scheduled.
- Contains: Case ID, patient ID, procedure type, scheduled date, surgeon ID.
- Consumed by: Patient Education Context (to deliver preoperative education).

**SurgeryCompleted**
- Published when: A surgical procedure has been performed.
- Contains: Case ID, patient ID, procedure performed, outcome summary, postoperative plan.
- Consumed by: Clinical Assessment Context (to schedule postoperative follow-up), Patient Education Context (to deliver postoperative education).

### Prenatal Care Context Events

**PregnancyConfirmed**
- Published when: A pregnancy is clinically confirmed and a prenatal record is created.
- Contains: Record ID, patient ID, estimated due date, initial risk assessment.
- Consumed by: Screening Programs Context (to initiate prenatal screening schedule), Patient Education Context (to begin prenatal education).

**HighRiskIdentified**
- Published when: A prenatal risk stratification identifies high-risk status.
- Contains: Record ID, patient ID, risk factors, recommended interventions.
- Consumed by: Clinical Assessment Context (for specialist consultation), Patient Education Context (for high-risk pregnancy education).

**LaborPlanFinalized**
- Published when: A labor and delivery plan is agreed upon with the patient.
- Contains: Record ID, patient ID, planned delivery mode, planned facility, contingency plans.
- Consumed by: Surgical Services Context (if cesarean delivery is planned).

### Screening Programs Context Events

**AbnormalResultDetected**
- Published when: A screening test returns an abnormal result.
- Contains: Episode ID, patient ID, screening type, result classification, recommended follow-up.
- Consumed by: Clinical Assessment Context (to initiate diagnostic follow-up), Patient Education Context (to provide result explanation).

**ScreeningDue**
- Published when: A patient's screening interval has elapsed and a new screening is due.
- Contains: Patient ID, screening type, due date, previous result summary.
- Consumed by: Clinical Assessment Context (to prompt screening at next visit).

### Patient Education Context Events

**EducationSessionCompleted**
- Published when: An education session is completed and comprehension is assessed.
- Contains: Session ID, patient ID, topics covered, comprehension score, decisions documented.
- Consumed by: All clinical contexts as appropriate, to record that patient education has been provided.

## Domain Event Design Principles

1. Events are named in past tense, reflecting that something has already happened.
2. Events are immutable. Once published, an event's data cannot be changed.
3. Events carry sufficient data for consumers to act without querying the publishing context.
4. Events do not contain the full domain model; they contain a summary relevant to consumers.
5. Event handling is asynchronous. Publishers do not wait for consumers to process events.
6. Events include metadata: event ID, timestamp, correlation ID, and source context.

## Event Ordering and Idempotency

Clinical safety requires that event consumers handle out-of-order delivery and duplicate delivery gracefully. Each consumer must be idempotent, producing the same outcome whether an event is processed once or multiple times. Event ordering is managed through correlation IDs and timestamps rather than strict sequential guarantees.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 8 on domain events.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 8 on domain events.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 7 on domain events and event-driven architecture.
