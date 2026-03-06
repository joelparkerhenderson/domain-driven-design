# Repositories in Medical Practice

## Overview

A repository is an abstraction that encapsulates the storage, retrieval, and search behavior for aggregate roots. Repositories provide a collection-like interface to the domain layer, hiding the details of persistence infrastructure. In medical practice, repositories manage the persistence of patient records, encounters, prescriptions, lab orders, claims, and virtual visits while enforcing aggregate boundaries and consistency rules.

## Why Repositories Matter in Medicine

Medical data persistence has unique requirements:

- Patient records must be retrievable by MRN, name, date of birth, and insurance member ID
- Encounter histories must support temporal queries (all visits in a date range, most recent visit)
- Prescriptions must be searchable by patient, medication, prescriber, and status
- Lab results must support queries by order, patient, test type, and abnormal flag
- Claims must be trackable across submission states and payer responses
- All data access must be auditable for HIPAA compliance

Repositories abstract these query capabilities while keeping the domain model free of persistence concerns.

## Key Repositories in the Medical Domain

### PatientRecordRepository (Patient Records Context)

- **Aggregate Root**: PatientRecord
- **Key Operations**: findByMRN, findByNameAndDOB, findByInsuranceMemberId, save, exists
- **Query Capabilities**: Duplicate detection (name + DOB match), active patient search, demographic search
- **Constraints**: MRN uniqueness enforced; soft delete only (records are never physically deleted)

### EncounterRepository (Clinical Workflow Context)

- **Aggregate Root**: Encounter
- **Key Operations**: findById, findByPatientAndDateRange, findActiveByProvider, save
- **Query Capabilities**: Patient encounter history, provider daily schedule, open encounters
- **Constraints**: Encounter IDs are unique; completed encounters are read-only

### PrescriptionRepository (Pharmacy & Medication Context)

- **Aggregate Root**: Prescription
- **Key Operations**: findById, findActiveByPatient, findByMedication, findByPrescriber, save
- **Query Capabilities**: Active medication list, controlled substance history, refill-eligible prescriptions
- **Constraints**: Controlled substance queries must log the requesting user for DEA compliance

### LabOrderRepository (Diagnostics & Imaging Context)

- **Aggregate Root**: LabOrder
- **Key Operations**: findById, findByPatient, findPendingByLab, findByStatus, save
- **Query Capabilities**: Pending orders by laboratory, resulted orders by date range, critical results awaiting acknowledgment
- **Constraints**: Orders with critical results must be flagged until acknowledged by the ordering provider

### ClaimRepository (Insurance & Claims Context)

- **Aggregate Root**: Claim
- **Key Operations**: findById, findByPatientAndDateRange, findByStatus, findByPayer, save
- **Query Capabilities**: Claims pending submission, denied claims for appeal, aging report by payer
- **Constraints**: Submitted claims are immutable; corrections require new claim submission

### VirtualVisitRepository (Telemedicine Context)

- **Aggregate Root**: VirtualVisit
- **Key Operations**: findById, findByPatientAndDateRange, findUpcomingByProvider, save
- **Query Capabilities**: Scheduled visits, completed sessions with duration, visits requiring follow-up
- **Constraints**: Completed visit records include session metadata for billing

## Repository Design Principles

- **Aggregate root access only**: Repositories store and retrieve complete aggregates, not individual entities or value objects within them
- **Collection semantics**: The repository interface behaves like an in-memory collection (add, remove, find), hiding persistence details
- **Domain-oriented queries**: Query methods use domain language (findActiveByPatient), not infrastructure language (SELECT WHERE status = 'active')
- **No business logic**: Repositories do not contain business rules; they only manage persistence
- **Unit of work**: Repository operations participate in a unit of work to ensure transactional consistency within an aggregate

## Event Sourcing Consideration

For certain medical aggregates, event sourcing -- where the repository stores domain events rather than current state -- provides significant advantages:

- Complete audit trail of every change to a patient record (required by HIPAA)
- Ability to reconstruct the state of a prescription at any point in time
- Natural support for regulatory inquiries about what data was available when a clinical decision was made

## Relationships to Other Topics

- [Aggregates](../aggregates/) -- Repositories persist and retrieve aggregate roots
- [Entities](../entities/) -- Aggregate roots stored by repositories are entities
- [Domain Events](../domain-events/) -- Event-sourced repositories store events as the source of truth
- [Domain Services](../domain-services/) -- Domain services use repositories to load and save aggregates
- [Regulatory Compliance](../regulatory-compliance/) -- Repository access patterns support HIPAA audit requirements

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 6. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 12. Addison-Wesley, 2013.
- Millett, Scott & Tune, Nick. "Patterns, Principles, and Practices of Domain-Driven Design." Chapter 21. Wrox, 2015.
- Fowler, Martin. "Patterns of Enterprise Application Architecture." Chapter 10. Addison-Wesley, 2002.
- Kleppmann, Martin. "Designing Data-Intensive Applications." Chapters 3, 11. O'Reilly, 2017.
