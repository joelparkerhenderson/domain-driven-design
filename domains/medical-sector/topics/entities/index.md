# Entities in Medical Practice

## Overview

An entity is a domain object that is defined by its unique identity rather than its attributes. Entities persist over time and can change their attributes while remaining the same conceptual object. In medical practice, entities represent the core actors and artifacts that must be tracked and referenced across the system -- patients, encounters, prescriptions, claims, and providers.

## Why Entities Matter in Medicine

Medical practice requires precise tracking of objects that evolve over time:

- A patient's demographics, conditions, and medications change, but the patient retains the same Medical Record Number (MRN) throughout
- An encounter accumulates notes, orders, and assessments over the course of a visit, but remains the same encounter
- A prescription may be modified, refilled, or discontinued, but its identity persists for auditing and reconciliation
- A claim moves through submission, adjudication, and payment states while retaining its claim identifier

Without clear entity identity, critical medical operations would fail -- lab results could be attached to the wrong patient, prescriptions could be lost during renewal, and insurance claims could become untraceable.

## Key Entities in the Medical Domain

### Patient (Patient Records Context)

- **Identity**: Medical Record Number (MRN) -- system-assigned, unique, immutable
- **Attributes**: Legal name, date of birth, gender, preferred language, marital status, contact information
- **Lifecycle**: Created at registration, updated throughout care, never deleted (archived when inactive)
- **Why an entity**: Two patients with identical names and birth dates are still different patients with different MRNs

### Encounter (Clinical Workflow Context)

- **Identity**: EncounterID -- system-generated unique identifier
- **Attributes**: Date, type (office visit, telehealth, inpatient), status (scheduled, in-progress, completed), associated patient, associated provider
- **Lifecycle**: Created when a visit begins, updated as clinical activity occurs, completed at checkout or discharge
- **Why an entity**: Each visit is a distinct event even if the same patient sees the same provider for the same complaint

### Prescription (Pharmacy & Medication Context)

- **Identity**: PrescriptionID -- system-generated unique identifier
- **Attributes**: Medication, dosage, quantity, refills remaining, prescriber, patient, status (active, completed, discontinued)
- **Lifecycle**: Created when a provider writes a prescription, updated with refills, discontinued or expired over time
- **Why an entity**: Two prescriptions for the same drug to the same patient are distinct tracked objects

### Claim (Insurance & Claims Context)

- **Identity**: ClaimID -- system-generated unique identifier
- **Attributes**: Date of service, billed amount, payer, status (draft, submitted, adjudicated, paid, denied), associated encounter
- **Lifecycle**: Created from encounter data, submitted to payer, adjudicated, paid or denied, potentially appealed
- **Why an entity**: Each claim submission is a distinct financial transaction

### LabOrder (Diagnostics & Imaging Context)

- **Identity**: OrderID -- system-generated unique identifier
- **Attributes**: Ordered tests, ordering provider, clinical indication, priority, status (ordered, collected, in-process, resulted)
- **Lifecycle**: Created when provider orders a test, updated as specimen is collected and processed, completed when results are available
- **Why an entity**: Each order is independently tracked through the laboratory workflow

### VirtualVisit (Telemedicine Context)

- **Identity**: VisitID -- system-generated unique identifier
- **Attributes**: Scheduled time, platform details, patient, provider, session status, duration
- **Lifecycle**: Created at scheduling, activated at connection, completed at session end
- **Why an entity**: Each virtual session is a distinct trackable event

## Entity Design Principles

- **Identity is central**: Choose identifiers that are stable, unique, and meaningful to the domain (MRN for patients, NPI for providers)
- **Equality by identity**: Two entities are equal if and only if they share the same identity, regardless of attribute values
- **Mutable attributes**: Unlike value objects, entity attributes can change over time
- **Lifecycle management**: Entities have clear creation, modification, and archival rules
- **Avoid attribute bloat**: Entities should contain only attributes relevant to their bounded context

## Relationships to Other Topics

- [Aggregates](../aggregates/) -- Entities serve as aggregate roots or internal entities within aggregates
- [Value Objects](../value-objects/) -- Entities contain value objects that describe their attributes
- [Domain Events](../domain-events/) -- Entity state changes trigger domain events
- [Repositories](../repositories/) -- Repositories store and retrieve entities by identity
- [Ubiquitous Language](../ubiquitous-language/) -- Entity names come from the ubiquitous language

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 5. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 5. Addison-Wesley, 2013.
- Millett, Scott & Tune, Nick. "Patterns, Principles, and Practices of Domain-Driven Design." Chapter 17. Wrox, 2015.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 5. O'Reilly, 2021.
