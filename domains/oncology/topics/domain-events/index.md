# Domain Events in Oncology

## Overview

Domain events represent significant occurrences within the oncology domain that other parts of the system need to know about. They are named using past tense to indicate that something has already happened: DiagnosisConfirmed, TreatmentPlanApproved, ChemotherapyCycleCompleted. Domain events are the primary mechanism for communication between bounded contexts in the oncology system, enabling loose coupling while maintaining clinical workflow coherence.

## Characteristics of Oncology Domain Events

Domain events in oncology are immutable records of facts. Once a DiagnosisConfirmed event is published, it represents a clinical fact that cannot be retracted, only superseded by subsequent events (such as DiagnosisAmended). Each event carries a timestamp, the identity of the aggregate that produced it, and the relevant domain data needed by consumers.

Oncology domain events must carry sufficient context for consumers to act without querying back to the producing context. A TreatmentPlanApproved event should contain the treatment modalities selected, the treatment intent, and the patient identifier, not just a treatment plan ID that requires a separate lookup. This self-contained design reduces coupling and improves resilience.

## Key Domain Events by Bounded Context

### Diagnosis and Staging Context Events

**ScreeningResultReceived**: Published when a cancer screening test result is received and recorded. Carries the screening type, result, and any findings requiring follow-up. May trigger diagnostic workup workflows.

**BiopsySpecimenCollected**: Published when a tissue sample is obtained for pathological examination. Carries the specimen accession number, collection site, and collection method. Triggers pathology processing workflows.

**PathologyReportFinalized**: Published when the pathologist completes the diagnostic report. Carries the histological diagnosis, grade, and biomarker results. Triggers staging evaluation and treatment planning initiation.

**DiagnosisConfirmed**: Published when the cancer diagnosis is formally confirmed with complete staging. Carries the cancer type, TNM classification, overall stage, and molecular profile. This is a pivotal event that triggers treatment planning across the system.

**StageRevised**: Published when staging is updated based on new information (additional imaging, surgical pathology). Carries the previous stage, updated stage, and the reason for revision. May trigger treatment plan re-evaluation.

### Treatment Planning Context Events

**TumorBoardCasePresented**: Published when a patient case is formally presented to the multidisciplinary tumor board. Carries the case summary and the clinical question being addressed.

**TumorBoardRecommendationIssued**: Published when the tumor board reaches a consensus recommendation. Carries the recommended treatment approach, participating specialists, and any dissenting opinions.

**TreatmentPlanApproved**: Published when the comprehensive treatment plan is finalized and approved. Carries the treatment modalities, sequencing, treatment intent, and responsible physicians. Triggers order creation in the relevant modality contexts.

**ClinicalTrialEligibilityDetermined**: Published when a patient is assessed for clinical trial eligibility. Carries the trial protocol identifier, eligibility determination (eligible, ineligible, pending), and any disqualifying criteria.

### Chemotherapy Management Context Events

**ChemotherapyOrderCreated**: Published when a new chemotherapy order is entered based on an approved treatment plan. Carries the regimen, calculated doses, and planned schedule.

**ChemotherapyCycleStarted**: Published when infusion of the first drug in a cycle begins. Carries the cycle number, drug being administered, and actual start time.

**ChemotherapyCycleCompleted**: Published when all drugs in a cycle have been administered. Carries the cycle number, actual doses administered, and any dose modifications.

**ToxicityRecorded**: Published when an adverse event is assessed and graded. Carries the CTCAE term, grade, attribution to treatment, and any dose modification triggered. High-grade toxicity events may trigger treatment hold or modification workflows.

**DoseModificationApplied**: Published when a drug dose is modified from the protocol-defined dose. Carries the drug, original dose, modified dose, modification percentage, and clinical justification.

### Radiation Therapy Context Events

**SimulationCompleted**: Published when the radiation therapy simulation session is finished. Carries the imaging data reference, immobilization setup, and treatment position.

**RadiationPlanApproved**: Published when the radiation dosimetric plan passes clinical review. Carries the prescribed dose, fractionation scheme, and dose-volume statistics for target and organs at risk.

**FractionDelivered**: Published when a single radiation treatment fraction is administered. Carries the fraction number, delivered dose, and imaging verification results.

**RadiationCourseCompleted**: Published when all planned fractions have been delivered. Carries the total delivered dose, treatment duration, and any treatment breaks.

### Surgical Oncology Context Events

**SurgicalProcedureCompleted**: Published when the operative procedure is finished. Carries the procedure type, intraoperative findings, and specimens submitted for pathology.

**MarginAssessmentReported**: Published when surgical pathology reports the margin status. Carries the margin status (positive, negative, close), closest margin distance, and location. A positive margin event may trigger re-excision planning or adjuvant therapy consideration.

**SentinelNodeResultReported**: Published when sentinel lymph node pathology is completed. Carries the number of nodes examined, number positive, and whether full lymph node dissection is recommended.

### Survivorship Care Context Events

**SurvivorshipCarePlanGenerated**: Published when a new or updated survivorship care plan is created. Carries the treatment summary, follow-up schedule, and recommended surveillance tests.

**SurveillanceTestDue**: Published when a scheduled follow-up test or examination is approaching. Carries the test type, recommended date, and clinical rationale.

**LateEffectIdentified**: Published when a new late treatment effect is diagnosed in a cancer survivor. Carries the effect description, severity, causative treatment, and recommended management.

## Event Ordering and Causality

Oncology domain events often have causal relationships that must be respected. A TreatmentPlanApproved event should only be produced after a DiagnosisConfirmed event for the same patient. A ChemotherapyCycleCompleted event should only occur after a ChemotherapyCycleStarted event for the same cycle. The event infrastructure must support ordering within a single aggregate's event stream, even if global ordering across aggregates is not guaranteed.

## Event Delivery Guarantees

Different oncology events require different delivery guarantees. Safety-critical events, such as high-grade toxicity alerts, require at-least-once delivery with acknowledgment. Informational events, such as a surveillance test reminder, can tolerate best-effort delivery. The domain model should express these delivery requirements as part of the event definition.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 14: Maintaining Model Integrity.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 8: Domain Events.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 10: Domain Events.
