# Domain Services in Hospital Management

## Overview

A domain service is a stateless operation that belongs to the domain but does not naturally fit within a single entity or value object. Domain services encapsulate business logic that spans multiple aggregates or requires coordination between domain objects that do not own the behavior individually.

## Relevance to Hospital Management

Hospital workflows frequently involve operations that cross aggregate boundaries: checking bed availability before admitting a patient, verifying insurance before scheduling a procedure, or detecting drug interactions across a patient's prescriptions. These operations are natural candidates for domain services.

## Key Concepts

### When to Use a Domain Service

Use a domain service when:

- The operation involves multiple aggregates (e.g., checking appointment conflicts across a provider's schedule)
- The operation represents a domain concept that does not belong to any single entity (e.g., triage assessment)
- The operation requires domain logic that would be awkward to place on an entity (e.g., insurance verification)

### When Not to Use a Domain Service

Avoid domain services when:

- The behavior clearly belongs on a single entity or value object (prefer rich entities over anemic models)
- The operation is purely technical (logging, caching, database access — these are infrastructure services)
- The service is merely a CRUD wrapper (this is an application service, not a domain service)

### Characteristics

- **Stateless**: Domain services do not hold state between calls
- **Named with domain language**: Service names use ubiquitous language verbs (e.g., `AssessTriage`, `VerifyInsurance`)
- **Defined in the domain layer**: Not in infrastructure or application layers
- **Accept and return domain objects**: Parameters and return types are entities, value objects, or domain events

## Hospital Domain Services

### BedAssignmentService

- **Purpose**: Assigns an available bed to a patient based on care requirements and availability
- **Inputs**: Patient (with care level), preferred ward, required equipment
- **Logic**:
  - Queries available beds matching care level (ICU, general, isolation)
  - Checks proximity to nursing station for high-acuity patients
  - Verifies infection control constraints (isolation requirements)
  - Assigns the best matching bed
- **Output**: BedAssignment value object or failure with reason
- **Aggregates involved**: Patient, Ward/Bed

### TriageAssessmentService

- **Purpose**: Evaluates an emergency patient and assigns an ESI level
- **Inputs**: Chief complaint, vital signs, medical history, allergies
- **Logic**:
  - Applies ESI algorithm based on presenting symptoms and vital signs
  - Considers patient age, comorbidities, and medication history
  - Flags patients requiring immediate intervention (ESI-1)
  - Determines resource needs for ESI-3 through ESI-5
- **Output**: TriageAssessment value object with ESI level and recommended area
- **Aggregates involved**: EmergencyEncounter, Patient

### AppointmentConflictChecker

- **Purpose**: Determines whether a proposed appointment conflicts with existing schedules
- **Inputs**: Provider NPI, proposed time slot, appointment type
- **Logic**:
  - Retrieves provider's existing appointments for the date
  - Checks for time overlap considering appointment duration
  - Accounts for buffer time between appointments
  - Considers provider-specific scheduling rules (e.g., maximum daily appointments)
- **Output**: ConflictResult (no conflict, soft conflict with override option, hard conflict)
- **Aggregates involved**: Appointment (multiple instances)

### InsuranceVerificationService

- **Purpose**: Verifies a patient's insurance coverage for a planned service
- **Inputs**: Patient insurance details, planned procedure codes, service date
- **Logic**:
  - Checks active coverage for the service date
  - Verifies procedure is covered under the plan
  - Determines copay, deductible, and out-of-pocket estimates
  - Checks prior authorization requirements
- **Output**: VerificationResult with coverage details or denial reason
- **Aggregates involved**: Patient, (external insurance system via ACL)

### DrugInteractionChecker

- **Purpose**: Checks for harmful drug interactions before a new prescription is issued
- **Inputs**: Proposed medication, patient's current active prescriptions, patient allergies
- **Logic**:
  - Cross-references proposed drug against active medications
  - Checks allergy list for contraindications
  - Classifies interactions by severity (minor, moderate, severe, contraindicated)
  - Flags interactions requiring physician review
- **Output**: InteractionReport with findings and severity levels
- **Aggregates involved**: Prescription (multiple), Patient

### DischargeEligibilityService

- **Purpose**: Determines whether a patient is eligible for discharge
- **Inputs**: Encounter with all orders and results
- **Logic**:
  - Verifies all critical orders have results reviewed by a provider
  - Checks that discharge medications are reconciled
  - Confirms follow-up appointments are scheduled
  - Validates discharge instructions are documented
- **Output**: DischargeEligibility (eligible, not eligible with reasons)
- **Aggregates involved**: Encounter, Prescription, Appointment

## Relationships to Other Topics

- [Aggregates](../aggregates/) — Domain services coordinate operations across aggregates
- [Entities](../entities/) — Domain services accept entities as inputs and may modify them
- [Repositories](../repositories/) — Domain services use repositories to retrieve aggregates
- [Domain Events](../domain-events/) — Domain services may produce domain events as side effects
- [Value Objects](../value-objects/) — Domain services often return value objects as results

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 5: A Model Expressed in Software. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 7: Services. Addison-Wesley, 2013.
- Millett, Scott & Tune, Nick. "Patterns, Principles, and Practices of Domain-Driven Design." Chapter 22: Domain Services. Wrox, 2015.
