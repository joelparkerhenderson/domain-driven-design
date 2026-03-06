# Domain Events in Hospital Management

## Overview

A domain event is a record of something significant that happened in the domain. Events are named in past tense using ubiquitous language (e.g., PatientAdmitted, AppointmentScheduled) and carry data about what occurred. Domain events enable loose coupling between bounded contexts — when something happens in one context, other contexts can react without direct dependencies.

## Relevance to Hospital Management

Hospital workflows are inherently event-driven: a patient arrives, is triaged, gets admitted, receives lab orders, gets results, and is eventually discharged. Each step triggers downstream actions — admitting a patient notifies billing to open an account, receiving a critical lab result alerts the attending physician. Domain events model these real-world occurrences.

## Key Concepts

### Event Naming

Events use past tense to indicate something that already happened:

- `PatientRegistered` — not "RegisterPatient" (that is a command)
- `AppointmentScheduled` — not "ScheduleAppointment"
- `LabResultReceived` — not "ReceiveLabResult"

### Event Payload

Each event carries enough data for consumers to react without querying back to the source:

- **Event ID**: Unique identifier for idempotency
- **Timestamp**: When the event occurred
- **Aggregate ID**: Which aggregate produced the event
- **Domain data**: Relevant attributes (e.g., patient MRN, diagnosis codes, appointment time)

### Event Immutability

Once published, events are facts — they cannot be modified or deleted. If a mistake occurred, a compensating event is published (e.g., `AppointmentCancelled` compensates for an erroneous `AppointmentScheduled`).

## Hospital Domain Events

### Patient Context Events

| Event | Trigger | Key Data | Consumers |
|-------|---------|----------|-----------|
| PatientRegistered | New patient registration | MRN, name, DOB, insurance | Clinical, Billing, Scheduling |
| PatientUpdated | Demographics change | MRN, changed fields | All contexts |
| PatientMerged | Duplicate records merged | Surviving MRN, retired MRN | All contexts |
| AllergyRecorded | New allergy documented | MRN, substance, reaction, severity | Clinical, Emergency |

### Scheduling Context Events

| Event | Trigger | Key Data | Consumers |
|-------|---------|----------|-----------|
| AppointmentScheduled | New booking | Appointment ID, patient MRN, provider, time | Clinical, Patient |
| AppointmentCancelled | Booking cancelled | Appointment ID, reason, cancelled by | Clinical, Patient |
| AppointmentRescheduled | Time/provider changed | Appointment ID, old time, new time | Clinical, Patient |
| AppointmentCheckedIn | Patient arrives | Appointment ID, check-in time | Clinical |

### Clinical/EMR Context Events

| Event | Trigger | Key Data | Consumers |
|-------|---------|----------|-----------|
| EncounterStarted | Clinical encounter begins | Encounter ID, patient MRN, provider, type | Billing |
| OrderPlaced | Lab/imaging/pharmacy order | Order ID, encounter ID, order type, priority | Lab systems, Pharmacy |
| LabResultReceived | Lab result available | Order ID, encounter ID, results, abnormal flags | Clinical alerts |
| PrescriptionIssued | New prescription | Rx ID, patient MRN, medication, dosage | Pharmacy, Patient |
| EncounterCompleted | Encounter closed | Encounter ID, diagnoses, procedures | Billing |

### Billing Context Events

| Event | Trigger | Key Data | Consumers |
|-------|---------|----------|-----------|
| ClaimSubmitted | Claim sent to payer | Claim ID, payer, amount, codes | Finance |
| PaymentReceived | Payment processed | Claim ID, amount, payer | Finance |
| ClaimDenied | Payer denies claim | Claim ID, denial reason | Revenue cycle |

### Emergency Context Events

| Event | Trigger | Key Data | Consumers |
|-------|---------|----------|-----------|
| TriageCompleted | Triage assessment done | Encounter ID, ESI level, chief complaint | Clinical |
| TraumaActivated | Trauma protocol initiated | Encounter ID, trauma level, ETA | Clinical, Staffing |
| PatientAdmittedFromED | ED patient moves to inpatient | Encounter ID, patient MRN, admitting diagnosis | Clinical, Billing, Bed Management |

## Event Handling Patterns

### Event Notification

The simplest pattern — an event signals that something happened, and consumers query the source for details if needed.

### Event-Carried State Transfer

The event carries all the data consumers need, reducing the need for callbacks. Used for cross-context communication where consumers should not depend on the source's API.

### Event Sourcing

The aggregate's state is derived entirely from its event history. Useful for clinical audit trails where the full history of changes is required (e.g., all modifications to a patient's allergy list).

## Relationships to Other Topics

- [Aggregates](../aggregates/) — Aggregates produce domain events when state changes
- [Event-Driven Architecture](../event-driven-architecture/) — Infrastructure for publishing and consuming events
- [Integration Patterns](../integration-patterns/) — Events are the primary inter-context communication mechanism
- [Regulatory Compliance](../regulatory-compliance/) — Events support audit trail requirements (HIPAA)
- [Bounded Contexts](../bounded-contexts/) — Events flow between bounded contexts

## References

- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 8: Domain Events. Addison-Wesley, 2013.
- Vernon, Vaughn. "Domain-Driven Design Distilled." Chapter 6: Tactical Design with Domain Events. Addison-Wesley, 2016.
- Evans, Eric. "Domain Events." Essay in "Domain-Driven Design Reference." 2014.
- Millett, Scott & Tune, Nick. "Patterns, Principles, and Practices of Domain-Driven Design." Chapter 20: Domain Events. Wrox, 2015.
- Richardson, Chris. "Microservices Patterns." Chapter 4: Managing Transactions with Sagas. Manning, 2018.
