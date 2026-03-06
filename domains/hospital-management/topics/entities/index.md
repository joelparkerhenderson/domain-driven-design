# Entities in Hospital Management

## Overview

An entity is a domain object that is defined by its unique identity rather than its attributes. Entities persist over time — their attributes may change, but their identity remains constant. In hospital management, entities represent the core objects that the system tracks throughout their lifecycle.

## Relevance to Hospital Management

Hospital operations revolve around identifiable objects: patients with medical record numbers, doctors with NPI numbers, appointments with unique IDs, and departments with codes. These objects change state frequently (a patient's address changes, an appointment is rescheduled, a doctor's schedule updates) but their identity — the ability to say "this is the same patient" — remains stable.

## Key Concepts

### Identity

Every entity has a unique identifier that distinguishes it from all other entities of the same type, even if all other attributes are identical. Two patients named "John Smith" born on the same date are distinct entities if they have different MRNs.

### Identity Generation Strategies

- **System-assigned**: Auto-generated IDs (UUIDs, sequential IDs) — used for internal entities like Encounters and Appointments
- **Domain-assigned**: Identifiers with domain meaning — MRN for patients, NPI for providers
- **External-assigned**: Identifiers from external systems — insurance member IDs, lab accession numbers

### Lifecycle

Entities are created, modified over time, and may be deactivated or archived. The system must track the entity across its full lifecycle.

## Hospital Entities

### Patient

- **Identity**: Medical Record Number (MRN)
- **Key attributes**: Name, date of birth, gender, address, phone, email, blood type, allergies, insurance
- **Lifecycle**: Registered → Active → (may have multiple encounters) → Deceased/Inactive
- **Context**: Aggregate root in the Patient Context
- **Notes**: Patient identity management is critical; duplicate MRNs must be detected and merged

### Doctor / Provider

- **Identity**: National Provider Identifier (NPI) or internal Staff ID
- **Key attributes**: Name, specialties, credentials, department affiliations, license status, schedule
- **Lifecycle**: Credentialed → Active → (schedule changes, department transfers) → Retired/Inactive
- **Context**: Referenced across Clinical, Scheduling, and Billing contexts

### Appointment

- **Identity**: System-generated Appointment ID
- **Key attributes**: Patient reference (MRN), provider reference (NPI), date/time, type, status, location
- **Lifecycle**: Scheduled → Confirmed → Checked-In → Completed / Cancelled / No-Show
- **Context**: Aggregate root in the Scheduling Context

### Encounter

- **Identity**: System-generated Encounter ID
- **Key attributes**: Patient reference, provider reference, type (inpatient/outpatient/emergency), start time, status, diagnoses
- **Lifecycle**: Started → In Progress → (orders placed, results received) → Completed → Billed
- **Context**: Aggregate root in the Clinical/EMR Context

### Department

- **Identity**: Department Code
- **Key attributes**: Name, location, type (clinical/administrative), head physician, capacity
- **Lifecycle**: Established → Active → (reorganizations) → Merged/Closed
- **Context**: Referenced across multiple contexts for organizational structure

### Ward / Bed

- **Identity**: Ward Code / Bed Number
- **Key attributes**: Ward type (ICU, general, maternity), capacity, current occupancy, housekeeping status
- **Lifecycle**: Available → Occupied → Cleaning → Available
- **Context**: Used in Patient and Emergency contexts for bed management

### Lab Order

- **Identity**: Order ID / Accession Number
- **Key attributes**: Ordering provider, patient, test type, priority, specimen type, clinical indication
- **Lifecycle**: Ordered → Collected → Processing → Resulted → Reviewed
- **Context**: Entity within the Encounter aggregate in the Clinical/EMR Context

## Entity Design Guidelines

- **Equality by identity**: Two entity instances are equal if and only if they have the same identifier, regardless of attribute values
- **Mutable attributes**: Unlike value objects, entity attributes change over time
- **Encapsulate behavior**: Entities should contain domain logic, not just data (e.g., `patient.recordAllergy(substance)` rather than setting a list directly)
- **Avoid anemic entities**: Entities with only getters and setters push business logic outside the domain model

## Relationships to Other Topics

- [Aggregates](../aggregates/) — Entities serve as aggregate roots or internal aggregate members
- [Value Objects](../value-objects/) — Entities contain value objects for attributes that lack identity
- [Repositories](../repositories/) — Repositories retrieve and persist entities (specifically aggregate roots)
- [Domain Events](../domain-events/) — Entities produce domain events when significant state changes occur
- [Ubiquitous Language](../ubiquitous-language/) — Entity names come from the ubiquitous language

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 5: A Model Expressed in Software. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 5: Entities. Addison-Wesley, 2013.
- Millett, Scott & Tune, Nick. "Patterns, Principles, and Practices of Domain-Driven Design." Chapter 18: Entities. Wrox, 2015.
