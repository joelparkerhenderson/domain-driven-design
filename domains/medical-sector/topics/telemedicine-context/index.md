# Telemedicine Context in Medical Practice

## Overview

The Telemedicine Context is the bounded context responsible for managing virtual clinical encounters, remote patient monitoring, telehealth scheduling, video session management, and telehealth-specific consent and documentation. This context enables the delivery of clinical care at a distance and integrates with the Clinical Workflow context to ensure virtual visits are documented as proper clinical encounters.

## Core Responsibilities

- **Virtual Visit Scheduling**: Managing telehealth appointment booking with provider availability, patient preferences, and time zone handling
- **Session Management**: Initiating, monitoring, and concluding video/audio sessions between patients and providers
- **Telehealth Consent**: Capturing and managing patient consent for telemedicine services, which has unique requirements distinct from in-person consent
- **Remote Patient Monitoring (RPM)**: Receiving, validating, and routing health data from connected devices (blood pressure monitors, glucometers, pulse oximeters, wearables)
- **Asynchronous Consultation**: Managing store-and-forward interactions where clinical information is collected and reviewed at different times
- **Platform Integration**: Managing connectivity, session quality, and technical support for virtual encounters

## Aggregates

### VirtualVisit Aggregate

- **Aggregate Root**: VirtualVisit (identified by VisitID)
- **Value Objects**: SessionDetails (platform, connection type, session token), ScheduledTime (date, time, time zone, duration), TelehealthConsent (consent date, scope, expiration), VisitOutcome (completed, patient no-show, technical failure, rescheduled)
- **Invariants**: A virtual visit requires active telehealth consent before session start; patient identity must be verified before clinical interaction begins; session cannot exceed platform-defined maximum duration; visit must be associated with a valid patient and provider

### RemoteMonitoringSession Aggregate

- **Aggregate Root**: RemoteMonitoringSession (identified by SessionID)
- **Value Objects**: DeviceReading (device type, measurement value, unit, timestamp, device identifier), ReadingThreshold (low alert, high alert, critical alert boundaries), MonitoringPlan (frequency, duration, enrolled devices)
- **Invariants**: Each reading must have a valid timestamp and device identifier; readings outside critical thresholds must generate immediate alerts; a monitoring session must have an associated care plan

### TelehealthProgram Aggregate

- **Aggregate Root**: TelehealthProgram (identified by ProgramID)
- **Value Objects**: ProgramEligibility (conditions, insurance requirements), EnrollmentCriteria, DeviceKit (list of provided devices)
- **Invariants**: A patient can be enrolled in at most one instance of a given program; enrollment requires verified insurance coverage for RPM services

## Domain Events

| Event | Trigger | Consumers |
|-------|---------|-----------|
| VirtualVisitScheduled | Telehealth appointment booked | Patient notification, Provider schedule |
| VirtualVisitStarted | Video session begins | Clinical Workflow (encounter creation) |
| VirtualVisitCompleted | Session ends normally | Clinical Workflow, Insurance & Claims |
| VirtualVisitCancelled | Visit cancelled | Scheduling |
| TechnicalFailureRecorded | Session interrupted | Support, Rescheduling |
| PatientNoShow | Patient does not join | Provider notification, Scheduling |
| RemoteMonitoringDataReceived | Device sends reading | Clinical Workflow, Patient Records |
| CriticalReadingDetected | Reading exceeds threshold | Clinical Workflow (urgent alert) |
| TelehealthConsentGranted | Patient consents to telehealth | Compliance, Audit |

## Integration with Other Contexts

### Telemedicine <-> Clinical Workflow (Partnership)

A VirtualVisitStarted event triggers encounter creation in the Clinical Workflow context. The clinical encounter captures all clinical documentation, orders, and assessments. VirtualVisitCompleted triggers encounter completion and downstream claim generation.

### Telemedicine <- Patient Records (Customer-Supplier)

The Telemedicine context consumes patient demographics and contact information for scheduling, session link delivery, and identity verification.

### Telemedicine -> Insurance & Claims (Customer-Supplier)

Completed virtual visits generate billing events. The Insurance context must apply the correct place of service code (02 for telehealth) and telehealth-specific modifiers (modifier -95 or GT for synchronous telehealth).

### Telemedicine -> Patient Records (Customer-Supplier)

Remote monitoring data and telehealth encounter summaries are published to Patient Records for longitudinal record keeping.

## Telemedicine-Specific Considerations

### Licensure and Geography

Providers must be licensed in the state where the patient is physically located during a telemedicine encounter. The system must capture and verify patient location at the time of each virtual visit.

### Consent Requirements

Telehealth consent is distinct from general medical consent and may include:

- Acknowledgment of telehealth limitations
- Consent for audio/video recording (if applicable)
- Understanding of privacy risks inherent in electronic communication
- State-specific consent requirements

### Billing and Reimbursement

Telehealth billing requires specific codes and modifiers:

- Place of service 02 (telehealth) or 10 (patient home)
- Modifier -95 (synchronous telehealth) or GT (via interactive telecommunications)
- CPT codes eligible for telehealth vary by payer and state
- Remote patient monitoring has separate CPT codes (99453, 99454, 99457, 99458)

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) -- Telemedicine is one of six medical bounded contexts
- [Clinical Workflow Context](../clinical-workflow-context/) -- Partner for virtual encounter documentation
- [Insurance & Claims Context](../insurance-claims-context/) -- Consumer of telehealth billing events
- [Patient Records Context](../patient-records-context/) -- Source of patient demographics, consumer of RPM data
- [Domain Events](../domain-events/) -- Virtual visit lifecycle events drive cross-context workflows
- [Regulatory Compliance](../regulatory-compliance/) -- State licensure, consent, and HIPAA apply to telemedicine

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
- AMA. "Telehealth Implementation Playbook." https://www.ama-assn.org/
- CMS. "Medicare Telehealth Services." https://www.cms.gov/
- CCHP. "State Telehealth Laws and Reimbursement Policies." Center for Connected Health Policy.
- HL7 International. "FHIR R4 Encounter Resource." https://hl7.org/fhir/encounter.html
- ATA. "American Telemedicine Association Practice Guidelines." https://www.americantelemed.org/
