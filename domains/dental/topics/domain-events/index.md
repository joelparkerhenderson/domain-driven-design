# Domain Events - Dental Domain

## Overview

Domain events represent significant occurrences within the dental domain that other parts of the system need to know about. They capture something that happened in the past and are named using past-tense verbs. In the dental domain, domain events drive the workflow from examination through treatment planning, procedure execution, and administrative processing. Events enable loose coupling between bounded contexts while maintaining data consistency across the system.

## Domain Event Design Principles

Dental domain events carry all the information the consuming context needs to react appropriately, without requiring the consumer to query back into the producing context. Events are immutable records of what occurred. They include a timestamp, the identity of the aggregate that produced them, and the relevant data payload.

## Events by Bounded Context

### Clinical Examination Context Events

**ExaminationCompleted**
Raised when a clinical examination is finalized. Payload includes patient identifier, examination type, examining provider, list of findings with tooth numbers and condition codes, and examination date. Consumed by the Treatment Planning Context to initiate treatment plan creation.

**CariesDetected**
Raised when a carious lesion is identified during examination. Payload includes patient identifier, tooth number, affected surfaces, caries classification, and detection method (visual, radiographic, or both). Consumed by the Treatment Planning Context to suggest restorative treatment items.

**PeriodontalProbingRecorded**
Raised when a full-mouth periodontal probing is completed. Payload includes patient identifier, assessment date, complete probing measurements (six sites per tooth), bleeding on probing indicators, and overall periodontal disease classification. Consumed by the Periodontal Care Context to update the longitudinal periodontal record.

**RadiographInterpreted**
Raised when a radiographic image interpretation is finalized. Payload includes patient identifier, image type, findings with tooth references, and interpreting provider. Consumed by the Treatment Planning Context for treatment plan development.

### Treatment Planning Context Events

**TreatmentPlanApproved**
Raised when a patient approves a treatment plan. Payload includes patient identifier, treatment plan identifier, list of approved treatment items with CDT codes, sequencing order, and cost estimates. Consumed by the Restorative Services, Orthodontic Management, and Periodontal Care contexts to initiate their respective procedures.

**InformedConsentObtained**
Raised when a patient signs informed consent for a specific procedure. Payload includes patient identifier, treatment item identifier, consent date, risks acknowledged, and alternatives discussed. Consumed by the clinical contexts as a prerequisite for procedure initiation.

**TreatmentPlanModified**
Raised when a treatment plan is modified after initial approval. Payload includes the treatment plan identifier, added items, removed items, and resequenced items. Consumed by downstream clinical contexts to adjust their procedure schedules.

### Restorative Services Context Events

**RestorationCompleted**
Raised when a restorative procedure is completed. Payload includes patient identifier, tooth number, surfaces restored, material used, CDT procedure code, and completing provider. Consumed by the Clinical Examination Context to update the dental chart and by the Practice Management Context to initiate billing.

**ImplantPlaced**
Raised when a dental implant is surgically placed. Payload includes patient identifier, implant site, implant specifications, placement date, and surgeon. Consumed by the Clinical Examination Context for chart update and the Practice Management Context for billing.

**EndodonticTreatmentCompleted**
Raised when root canal therapy is completed. Payload includes patient identifier, tooth number, number of canals treated, obturation material, and completing provider. Consumed by the Clinical Examination Context and the Treatment Planning Context to sequence follow-up restorative work.

### Orthodontic Management Context Events

**OrthodonticCaseStarted**
Raised when active orthodontic treatment begins with appliance placement. Payload includes patient identifier, case identifier, appliance type, and treatment start date. Consumed by the Practice Management Context to schedule recurring adjustment appointments.

**OrthodonticProgressRecorded**
Raised when an adjustment visit documents treatment progress. Payload includes case identifier, visit date, measurements recorded, and comparison to treatment goals. Consumed internally for progress tracking and by the Clinical Examination Context for chart updates.

**OrthodonticTreatmentCompleted**
Raised when active orthodontic treatment objectives are met. Payload includes case identifier, completion date, and retention plan details. Consumed by the Practice Management Context to adjust scheduling and the Treatment Planning Context for any follow-up planning.

### Periodontal Care Context Events

**ScalingAndRootPlaningCompleted**
Raised when a scaling and root planing procedure is completed for one or more quadrants. Payload includes patient identifier, quadrants treated, date, and performing provider. Consumed by the Practice Management Context for billing and the Clinical Examination Context for chart notation.

**PeriodontalReevaluationCompleted**
Raised when a post-treatment re-evaluation is performed. Payload includes patient identifier, updated probing measurements, disease classification changes, and recommended next steps. Consumed by the Treatment Planning Context if additional treatment is indicated.

**PeriodontalMaintenanceScheduled**
Raised when a maintenance appointment interval is established or modified. Payload includes patient identifier, recommended interval, and next due date. Consumed by the Practice Management Context to configure the recall schedule.

### Practice Management Context Events

**AppointmentScheduled**
Raised when a patient appointment is booked. Payload includes patient identifier, appointment date, provider, operatory, procedure type, and duration.

**InsuranceVerified**
Raised when insurance eligibility and benefits are verified. Payload includes patient identifier, coverage details, benefit limits, and verification date. Consumed by the Treatment Planning Context for cost estimation accuracy.

**ClaimSubmitted**
Raised when an insurance claim is submitted. Payload includes claim identifier, patient identifier, procedure codes, and submission date.

**ClaimAdjudicated**
Raised when an insurance claim receives a payment determination. Payload includes claim identifier, paid amount, patient responsibility, and any denial reasons. Consumed internally for accounts receivable management.

## Event Flow Patterns

The primary event flow follows the clinical workflow: examination events trigger treatment planning, planning approval events trigger procedure contexts, procedure completion events trigger chart updates and billing. This creates a natural event-driven pipeline that mirrors the dental care delivery process.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 8 on domain events.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 8 on domain events and event sourcing.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 6 on domain events.
