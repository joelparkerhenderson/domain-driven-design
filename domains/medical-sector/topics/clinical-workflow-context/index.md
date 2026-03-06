# Clinical Workflow Context in Medical Practice

## Overview

The Clinical Workflow Context is the bounded context responsible for managing the clinical care process from patient intake through diagnosis, treatment planning, and follow-up. It encompasses encounter management, order entry, clinical decision support (CDS), care plans, clinical notes, and referrals. This context models the physician's and care team's workflow and is the core domain of the medical practice system.

## Core Responsibilities

- **Encounter Management**: Creating and managing clinical encounters (office visits, consultations, follow-ups)
- **Order Entry**: Placing orders for lab tests, imaging studies, medications, and referrals
- **Clinical Decision Support**: Evaluating evidence-based rules and generating alerts at the point of care
- **Care Plan Management**: Documenting goals, interventions, and timelines for ongoing patient management
- **Clinical Documentation**: Recording assessments, diagnoses, treatment decisions, and clinical notes
- **Referral Management**: Creating and tracking referrals to specialists and external providers

## Aggregate: Encounter

### Aggregate Root: Encounter

- **Identity**: EncounterID -- system-generated unique identifier
- **Attributes**: Date, type (office visit, telehealth, consultation), status (scheduled, in-progress, completed), associated patient MRN, associated provider NPI

### Internal Entities

- **ClinicalNote**: Note ID, note type (progress note, procedure note, consultation note), author, timestamp, content

### Internal Value Objects

- **ChiefComplaint**: Description text, onset date, severity
- **Assessment**: Clinical findings, impression, plan
- **Diagnosis**: ICD-10 code, description, type (principal, secondary), status (working, confirmed)
- **VitalSigns**: Temperature, blood pressure, heart rate, respiratory rate, oxygen saturation, weight, height
- **OrderReference**: Order type (lab, imaging, medication, referral), order ID, status

### Invariants

- An encounter must have an associated patient and provider
- An encounter cannot be completed without at least one assessment or diagnosis
- Orders can only be placed within an active (in-progress) encounter
- A completed encounter is immutable except for addendum notes
- Each encounter must have documented vital signs (unless clinically contraindicated)

## Clinical Decision Support

Clinical decision support operates within this context, evaluating rules against the current encounter and patient data:

- **Drug-condition alerts**: Warn when an ordered medication is contraindicated for a patient's condition
- **Duplicate order detection**: Alert when a provider orders a test that was recently performed
- **Preventive care reminders**: Recommend age-appropriate screenings based on patient demographics
- **Critical lab result alerts**: Notify the provider when a lab result requires immediate action
- **Evidence-based order sets**: Suggest standard order bundles for common diagnoses

CDS rules are pure domain logic, independent of any user interface or infrastructure.

## Domain Events

| Event | Trigger | Consumers |
|-------|---------|-----------|
| EncounterStarted | Visit begins | Patient Records, Telemedicine |
| EncounterCompleted | Visit finishes | Insurance & Claims, Patient Records |
| OrderPlaced | Provider creates order | Diagnostics, Pharmacy |
| DiagnosisRecorded | Provider documents diagnosis | Patient Records (problem list), Insurance |
| ClinicalDecisionSupportAlertFired | CDS rule triggered | Audit |
| ReferralCreated | Provider refers patient | Patient Records, Scheduling |
| CarePlanUpdated | Care plan modified | Patient Records |

## Integration with Other Contexts

### Clinical Workflow -> Diagnostics & Imaging (Customer-Supplier)

When a provider places a lab or imaging order, the Clinical Workflow context publishes an OrderPlaced event. The Diagnostics context picks up the order, processes the specimen or performs the study, and publishes results back.

### Clinical Workflow -> Pharmacy & Medication (Partnership)

Medication orders flow from Clinical Workflow to Pharmacy. The Pharmacy context validates prescriptions against formulary and drug interaction rules and may send alerts back to the clinical context. A published language for medication concepts ensures consistency.

### Clinical Workflow -> Insurance & Claims (Customer-Supplier)

When an encounter is completed, the Insurance context consumes the encounter data (diagnoses, procedures, provider) to generate and submit a claim.

### Clinical Workflow <- Patient Records (Customer-Supplier)

The Clinical Workflow context consumes patient identity, allergy, and problem list data from Patient Records to display at the point of care and drive CDS rules.

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) -- Clinical Workflow is the core bounded context
- [Aggregates](../aggregates/) -- Encounter is a key aggregate with clinical notes and orders
- [Domain Services](../domain-services/) -- ClinicalDecisionSupportService and ReferralRoutingService operate here
- [Domain Events](../domain-events/) -- Encounter lifecycle events drive cross-context workflows
- [Patient Records Context](../patient-records-context/) -- Upstream source of patient identity and clinical history
- [Diagnostics & Imaging Context](../diagnostics-imaging-context/) -- Downstream consumer of clinical orders
- [Pharmacy & Medication Context](../pharmacy-medication-context/) -- Partner for medication management

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 14. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 2. Addison-Wesley, 2013.
- HL7 International. "FHIR R4 Encounter Resource." https://hl7.org/fhir/encounter.html
- Berner, Eta S. "Clinical Decision Support Systems: Theory and Practice." Springer, 2016.
- Kawamoto, Kensaku et al. "Improving Clinical Practice Using Clinical Decision Support Systems." BMJ, 2005.
- ONC. "Clinical Decision Support." HealthIT.gov.
