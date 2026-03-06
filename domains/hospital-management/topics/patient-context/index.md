# Patient Context in Hospital Management

## Overview

The Patient Context is the bounded context responsible for patient identity, demographics, registration, medical history, insurance information, and consent management. It serves as the authoritative source of "who the patient is" for the entire hospital system. Other contexts reference patient identity from this context but maintain their own representations of patient data relevant to their domain.

## Core Responsibilities

- **Patient Registration**: Creating new patient records with demographics, contact information, and insurance details
- **Identity Management**: Assigning and managing Medical Record Numbers (MRNs), detecting and merging duplicate records
- **Demographics Maintenance**: Updating address, phone, email, emergency contacts, and other personal information
- **Medical History**: Recording allergies, past surgical history, family history, and chronic conditions
- **Insurance Management**: Tracking active insurance policies, coverage periods, and member IDs
- **Consent Management**: Recording patient consent for treatment, data sharing, and research participation

## Aggregate: Patient

### Aggregate Root: Patient

- **Identity**: Medical Record Number (MRN) — system-assigned, unique, immutable
- **Attributes**: Legal name, date of birth, gender, preferred language, marital status, ethnicity

### Internal Value Objects

- **Address**: Street, city, state, postal code, country
- **ContactInfo**: Phone numbers, email, preferred contact method
- **EmergencyContact**: Name, relationship, phone number
- **Allergy**: Substance, reaction type, severity, onset date
- **BloodType**: Group (A/B/AB/O), Rh factor (+/-)
- **InsurancePolicy**: Payer, member ID, group number, coverage type, effective dates

### Invariants

- MRN is unique across the system and cannot be reassigned
- A patient must have at least one form of contact information
- Allergies cannot be duplicated for the same substance
- At most one primary insurance policy is active at a time
- Date of birth cannot be in the future

## Domain Events

| Event | Trigger | Consumers |
|-------|---------|-----------|
| PatientRegistered | New patient created | Clinical, Billing, Scheduling |
| PatientUpdated | Demographics changed | All contexts with patient projections |
| PatientMerged | Duplicate records merged | All contexts (must update references) |
| AllergyRecorded | New allergy added | Clinical, Emergency, Pharmacy |
| InsurancePolicyUpdated | Coverage changed | Billing |
| ConsentGranted | Patient grants consent | Clinical, Research |
| ConsentRevoked | Patient revokes consent | Clinical, Research |

## Integration with Other Contexts

### Patient → Clinical/EMR (Customer-Supplier)

The Clinical context is a downstream consumer of patient identity. It subscribes to PatientRegistered and PatientUpdated events and maintains a local PatientSummary projection containing only clinically relevant data (MRN, name, DOB, allergies, blood type).

### Patient → Billing (Customer-Supplier)

The Billing context consumes insurance policy information to determine coverage and submit claims. It maintains a local AccountHolder projection with billing-relevant patient data and insurance details.

### Patient → Emergency (Shared Kernel)

Emergency services require immediate access to critical patient data (allergies, blood type, current medications) without event propagation delays. A minimal shared kernel provides real-time access to life-critical patient information.

### Patient → Scheduling (Customer-Supplier)

The Scheduling context needs basic patient identity to associate appointments. It maintains a lightweight patient reference (MRN, name, contact info).

## Key Workflows

### Patient Registration

1. Registration clerk enters patient demographics
2. System checks for potential duplicates (name + DOB match)
3. If no duplicate found, system assigns MRN and creates Patient aggregate
4. PatientRegistered event published
5. Downstream contexts create local projections

### Duplicate Detection and Merge

1. System or staff identifies potential duplicate patients (same name, DOB, SSN)
2. Clinical review confirms duplicates
3. Records are merged: surviving MRN retains all data, retired MRN is deactivated
4. PatientMerged event published with surviving and retired MRNs
5. All contexts update their references from retired MRN to surviving MRN

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) — Patient Context is one of five hospital bounded contexts
- [Entities](../entities/) — Patient is a core entity with MRN identity
- [Value Objects](../value-objects/) — Address, BloodType, Allergy are value objects within the Patient aggregate
- [Aggregates](../aggregates/) — Patient is an aggregate root
- [Domain Events](../domain-events/) — Patient context publishes identity and demographic events
- [Regulatory Compliance](../regulatory-compliance/) — Patient data is subject to HIPAA privacy and security rules

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 14. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 2. Addison-Wesley, 2013.
- HL7 International. "FHIR R4 Patient Resource." https://hl7.org/fhir/patient.html
- U.S. Department of Health and Human Services. "HIPAA Privacy Rule." 45 CFR Part 164.
- AHIMA. "Patient Identity Integrity." Journal of AHIMA, 2014.
