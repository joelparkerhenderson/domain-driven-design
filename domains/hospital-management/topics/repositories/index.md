# Repositories in Hospital Management

## Overview

A repository is an abstraction that encapsulates the storage, retrieval, and search behavior for aggregate roots. It provides a collection-like interface to the domain layer, hiding the details of database technology, query language, and data mapping. Domain code interacts with repositories using domain concepts, not SQL or ORM specifics.

## Relevance to Hospital Management

Hospital systems manage large volumes of persistent data — patient records, encounters, appointments, prescriptions, and claims. Repositories ensure that domain logic remains focused on healthcare concepts rather than data access mechanics. They also provide a natural seam for testing, allowing in-memory implementations during development and unit testing.

## Key Concepts

### One Repository per Aggregate Root

Repositories exist only for aggregate roots, not for internal entities or value objects. To access a patient's allergies, you retrieve the Patient aggregate through `PatientRepository`, not through a separate `AllergyRepository`.

### Collection-Oriented vs. Persistence-Oriented

- **Collection-oriented**: The repository mimics an in-memory collection. You add and remove objects; the repository tracks changes and persists them (Unit of Work pattern). Common with ORMs like Hibernate.
- **Persistence-oriented**: The repository requires explicit save/update calls. Each modification must be explicitly persisted. Common with document stores and explicit data mappers.

### Query Specifications

Complex queries are encapsulated in specification objects that express domain criteria:

- `PatientsWithAllergyTo("Penicillin")` — find patients allergic to penicillin
- `AppointmentsForProviderOnDate(npi, date)` — find a provider's appointments on a given date
- `EncountersAwaitingDischarge()` — find encounters ready for discharge

## Hospital Repositories

### PatientRepository

- **Aggregate root**: Patient
- **Key operations**:
  - `findByMrn(mrn)` — Retrieve patient by Medical Record Number
  - `findByNameAndDob(name, dateOfBirth)` — Search for potential duplicates
  - `save(patient)` — Persist new or updated patient
  - `findWithAllergyTo(substance)` — Find patients with a specific allergy
- **Considerations**: Patient data is accessed by nearly every bounded context; read performance is critical. Consider CQRS with read-optimized projections.

### AppointmentRepository

- **Aggregate root**: Appointment
- **Key operations**:
  - `findById(appointmentId)` — Retrieve specific appointment
  - `findByPatient(mrn, dateRange)` — Patient's appointments in a date range
  - `findByProvider(npi, date)` — Provider's schedule for a date
  - `findConflicts(provider, timeSlot)` — Check for scheduling conflicts
  - `save(appointment)` — Persist new or updated appointment
- **Considerations**: Conflict detection queries must be highly performant to support real-time scheduling.

### EncounterRepository

- **Aggregate root**: Encounter
- **Key operations**:
  - `findById(encounterId)` — Retrieve specific encounter
  - `findActiveByPatient(mrn)` — Find patient's open encounters
  - `findByProviderAndDateRange(npi, startDate, endDate)` — Provider's encounters over a period
  - `findAwaitingResults(encounterId)` — Encounters with pending orders
  - `save(encounter)` — Persist new or updated encounter
- **Considerations**: Encounters accumulate data over time (orders, results, notes). Consider lazy loading for large encounter histories.

### PrescriptionRepository

- **Aggregate root**: Prescription
- **Key operations**:
  - `findById(prescriptionId)` — Retrieve specific prescription
  - `findActiveByPatient(mrn)` — Patient's current medications
  - `findByMedication(medicationCode)` — All prescriptions for a specific drug (for recalls)
  - `save(prescription)` — Persist new or updated prescription

### ClaimRepository

- **Aggregate root**: Claim
- **Key operations**:
  - `findById(claimId)` — Retrieve specific claim
  - `findByEncounter(encounterId)` — Claims associated with an encounter
  - `findPendingByPayer(payerId)` — Outstanding claims for a payer
  - `findDenied(dateRange)` — Denied claims for review
  - `save(claim)` — Persist new or updated claim

## Design Guidelines

- **No domain logic in repositories**: Repositories should only handle persistence concerns, not business rules
- **Return aggregate roots**: Never return internal entities or raw database rows
- **Interface in domain layer, implementation in infrastructure**: The repository interface is defined in the domain; the implementation (SQL, MongoDB, etc.) lives in infrastructure
- **Avoid generic repositories**: `Repository<T>` loses domain meaning. Prefer specific repositories with domain-meaningful query methods
- **Consider read models**: For complex queries that span aggregates, use CQRS read models rather than forcing queries through aggregate repositories

## Relationships to Other Topics

- [Aggregates](../aggregates/) — One repository per aggregate root
- [Entities](../entities/) — Repositories retrieve and persist aggregate root entities
- [Domain Services](../domain-services/) — Domain services use repositories to load aggregates for cross-aggregate operations
- [Bounded Contexts](../bounded-contexts/) — Each context has its own repositories for its own aggregates

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 6: The Life Cycle of a Domain Object. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 12: Repositories. Addison-Wesley, 2013.
- Millett, Scott & Tune, Nick. "Patterns, Principles, and Practices of Domain-Driven Design." Chapter 21: Repositories. Wrox, 2015.
- Fowler, Martin. "Patterns of Enterprise Application Architecture." Chapter 10: Data Source Architectural Patterns. Addison-Wesley, 2002.
