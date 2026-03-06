# Domain Events in Medical Practice

## Overview

A domain event is a record of something significant that happened in the domain, expressed in past tense using the ubiquitous language. Domain events capture state changes within aggregates and enable communication between bounded contexts without creating tight coupling. In medical practice, domain events represent clinically and administratively significant occurrences -- a prescription being issued, a lab result becoming available, a claim being submitted, or a virtual visit being completed.

## Why Domain Events Matter in Medicine

Medical workflows are inherently event-driven:

- When a provider issues a prescription, the pharmacy must be notified to dispense the medication
- When a lab result arrives, the ordering provider must be alerted, and the patient record must be updated
- When an encounter is completed, the insurance context must generate and submit a claim
- When a patient's allergy list changes, all contexts with medication or treatment logic must be informed
- When a controlled substance is prescribed, regulatory monitoring systems must be notified

Domain events make these cross-context flows explicit, traceable, and auditable -- a critical requirement in healthcare.

## Key Domain Events by Bounded Context

### Patient Records Context

| Event | Trigger | Typical Consumers |
|-------|---------|-------------------|
| PatientRegistered | New patient created | Clinical, Insurance, Telemedicine |
| PatientDemographicsUpdated | Demographics changed | All contexts with patient projections |
| ProblemListUpdated | Condition added/resolved | Clinical, Pharmacy |
| AllergyRecorded | New allergy documented | Clinical, Pharmacy, Diagnostics |
| InsurancePolicyUpdated | Coverage changed | Insurance & Claims |

### Clinical Workflow Context

| Event | Trigger | Typical Consumers |
|-------|---------|-------------------|
| EncounterStarted | Visit begins | Patient Records, Telemedicine |
| EncounterCompleted | Visit finishes | Insurance, Patient Records |
| OrderPlaced | Provider creates order | Diagnostics, Pharmacy |
| ClinicalDecisionSupportAlertFired | CDS rule triggered | Audit, Clinical |
| ReferralCreated | Provider refers patient | Scheduling, Patient Records |
| CarePlanUpdated | Care plan modified | Patient Records |

### Diagnostics & Imaging Context

| Event | Trigger | Typical Consumers |
|-------|---------|-------------------|
| LabResultAvailable | Lab test completed | Clinical, Patient Records |
| CriticalResultFlagged | Critical value detected | Clinical (urgent alert) |
| ImagingStudyCompleted | Imaging performed | Clinical, Patient Records |
| PathologyReportFinalized | Pathology review done | Clinical, Patient Records |

### Pharmacy & Medication Context

| Event | Trigger | Typical Consumers |
|-------|---------|-------------------|
| PrescriptionIssued | New prescription created | Pharmacy dispensing |
| DrugInteractionDetected | Interaction found | Clinical (alert) |
| MedicationDispensed | Pharmacy fills prescription | Patient Records, Clinical |
| MedicationAdministered | Medication given to patient | Patient Records |
| ControlledSubstancePrescribed | Schedule II-V drug prescribed | Regulatory monitoring |

### Insurance & Claims Context

| Event | Trigger | Typical Consumers |
|-------|---------|-------------------|
| EligibilityVerified | Coverage confirmed | Clinical, Scheduling |
| PriorAuthorizationApproved | Pre-auth granted | Clinical, Pharmacy |
| ClaimSubmitted | Claim sent to payer | Billing, Audit |
| ClaimAdjudicated | Payer processes claim | Billing, Finance |
| ClaimDenied | Payer denies claim | Billing (appeal workflow) |

### Telemedicine Context

| Event | Trigger | Typical Consumers |
|-------|---------|-------------------|
| VirtualVisitScheduled | Telehealth appointment set | Scheduling, Patient |
| VirtualVisitStarted | Video session begins | Clinical Workflow |
| VirtualVisitCompleted | Session ends | Clinical, Insurance |
| RemoteMonitoringDataReceived | Device sends readings | Clinical, Patient Records |

## Event Design Principles

- **Past tense naming**: Events describe what happened (PrescriptionIssued, not IssuePrescription)
- **Immutable**: Once published, an event's data cannot be changed
- **Self-contained**: Events carry enough data for consumers to act without querying the source
- **Ordered within aggregate**: Events from the same aggregate are processed in order
- **Idempotent consumers**: Consumers must handle receiving the same event more than once

## Relationships to Other Topics

- [Aggregates](../aggregates/) -- Aggregates publish domain events when their state changes
- [Event-Driven Architecture](../event-driven-architecture/) -- The architectural pattern that uses domain events for system-wide communication
- [Bounded Contexts](../bounded-contexts/) -- Domain events are the primary mechanism for cross-context communication
- [Repositories](../repositories/) -- Event sourcing is an alternative persistence strategy where events are the source of truth
- [Regulatory Compliance](../regulatory-compliance/) -- Domain events provide a natural audit trail for compliance

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 8. Addison-Wesley, 2013.
- Millett, Scott & Tune, Nick. "Patterns, Principles, and Practices of Domain-Driven Design." Chapter 20. Wrox, 2015.
- Kleppmann, Martin. "Designing Data-Intensive Applications." Chapter 11. O'Reilly, 2017.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 9. O'Reilly, 2021.
