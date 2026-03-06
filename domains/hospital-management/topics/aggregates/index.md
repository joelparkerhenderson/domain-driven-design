# Aggregates in Hospital Management

## Overview

An aggregate is a cluster of domain objects that are treated as a single unit for the purpose of data changes and consistency enforcement. Each aggregate has a root entity (the aggregate root) through which all external access is managed. The aggregate root enforces invariants — business rules that must always be true — across all objects within the aggregate boundary.

## Relevance to Hospital Management

Hospital data involves complex relationships: a patient has medical records, allergies, and insurance details; an encounter includes orders, results, and notes. Aggregates define which objects must be consistent together in a single transaction and which can be eventually consistent across transactions.

## Key Concepts

### Aggregate Root

The single entity through which all modifications to the aggregate pass. External code never directly modifies objects inside the aggregate — it always goes through the root.

### Invariants

Business rules that must hold at all times within the aggregate boundary. For example:

- A patient cannot have duplicate allergy records for the same substance
- An appointment cannot be scheduled outside provider availability hours
- A prescription must have a valid dosage before it can be issued

### Transactional Consistency

All changes within an aggregate are committed or rolled back as a single transaction. Changes between aggregates are eventually consistent, communicated via domain events.

## Hospital Aggregates

### Patient Aggregate

- **Root**: Patient (identified by MRN)
- **Internal entities**: MedicalHistory, InsurancePolicy
- **Internal value objects**: Address, EmergencyContact, Allergy, BloodType
- **Invariants**:
  - MRN is unique and immutable once assigned
  - At most one active insurance policy per payer
  - Allergies are not duplicated for the same substance
- **Events produced**: PatientRegistered, PatientUpdated, AllergyRecorded

### Encounter Aggregate

- **Root**: Encounter (identified by EncounterID)
- **Internal entities**: ClinicalNote, Order
- **Internal value objects**: DiagnosisCode, VitalSigns, ChiefComplaint
- **Invariants**:
  - An encounter must have an attending provider
  - Orders can only be placed on active encounters
  - Discharge requires all critical results to be reviewed
- **Events produced**: EncounterStarted, OrderPlaced, EncounterCompleted

### Appointment Aggregate

- **Root**: Appointment (identified by AppointmentID)
- **Internal value objects**: TimeSlot, AppointmentType, CancellationReason
- **Invariants**:
  - Appointment time must fall within provider availability
  - Cannot double-book the same provider for overlapping time slots
  - Cancellation requires a reason
- **Events produced**: AppointmentScheduled, AppointmentCancelled, AppointmentRescheduled

### Prescription Aggregate

- **Root**: Prescription (identified by PrescriptionID)
- **Internal value objects**: Dosage, Medication, Frequency, Route
- **Invariants**:
  - Dosage must be within safe therapeutic range for the medication
  - Prescriber must be authorized to prescribe the medication class
  - Controlled substances require additional verification
- **Events produced**: PrescriptionIssued, PrescriptionDispensed

### Claim Aggregate

- **Root**: Claim (identified by ClaimID)
- **Internal entities**: ChargeItem, ClaimLineItem
- **Internal value objects**: BillingCode (CPT), DiagnosisCode (ICD-10), Money
- **Invariants**:
  - Every charge item must have a valid CPT code
  - Claim total must equal the sum of line items
  - Submitted claims cannot be modified (only adjusted via new claims)
- **Events produced**: ClaimSubmitted, ClaimAdjudicated, PaymentReceived

## Aggregate Design Rules

1. **Keep aggregates small**: Prefer smaller aggregates with fewer internal objects. Large aggregates create contention and performance problems.
2. **Reference other aggregates by identity**: An Encounter references a Patient by MRN, not by containing the Patient object.
3. **Use domain events for cross-aggregate coordination**: When a PatientAdmitted event occurs, the Billing context creates a new Account — but this happens asynchronously, not in the same transaction.
4. **Design for eventual consistency between aggregates**: Accept that the Billing context may learn about a new encounter milliseconds after it starts, not at the exact same instant.

## Relationships to Other Topics

- [Entities](../entities/) — Aggregate roots are entities; aggregates contain entities
- [Value Objects](../value-objects/) — Aggregates contain value objects for immutable domain concepts
- [Domain Events](../domain-events/) — Aggregates produce events to communicate changes
- [Repositories](../repositories/) — One repository per aggregate root
- [Domain Services](../domain-services/) — Coordinate operations across multiple aggregates

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 6: The Life Cycle of a Domain Object. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 10: Aggregates. Addison-Wesley, 2013.
- Vernon, Vaughn. "Domain-Driven Design Distilled." Chapter 5: Tactical Design with Aggregates. Addison-Wesley, 2016.
- Millett, Scott & Tune, Nick. "Patterns, Principles, and Practices of Domain-Driven Design." Chapter 19: Aggregates. Wrox, 2015.
