# Patient Records Context in Medical Practice

## Overview

The Patient Records Context is the bounded context responsible for patient identity, demographics, electronic health records (EHR), medical history, problem lists, allergy records, and insurance policy information. It serves as the authoritative source of "who the patient is" and "what conditions the patient has" for the entire medical system. Other contexts reference patient identity from this context but maintain their own representations of patient data relevant to their domain.

## Core Responsibilities

- **Patient Registration**: Creating new patient records with demographics, contact information, and insurance details
- **Identity Management**: Assigning and managing Medical Record Numbers (MRNs), detecting and merging duplicate records
- **Demographics Maintenance**: Updating address, phone, email, emergency contacts, and other personal information
- **Problem List Management**: Maintaining a curated list of active and resolved medical conditions with ICD-10 codes
- **Medical History**: Recording past surgical history, family history, social history, and prior hospitalizations
- **Allergy Documentation**: Recording allergies and adverse reactions with substance, reaction type, and severity
- **Insurance Policy Tracking**: Maintaining active insurance policies, coverage periods, and member identifiers

## Aggregate: PatientRecord

### Aggregate Root: PatientRecord

- **Identity**: Medical Record Number (MRN) -- system-assigned, unique, immutable
- **Attributes**: Legal name, date of birth, gender, preferred language, marital status, ethnicity, primary care provider

### Internal Value Objects

- **Demographics**: Name components (first, middle, last, suffix), date of birth, gender, preferred language
- **Address**: Street, city, state, postal code, country, type (home, work, mailing)
- **ContactInfo**: Phone numbers, email addresses, preferred contact method
- **EmergencyContact**: Name, relationship, phone number, address
- **Allergy**: Substance, reaction type, severity (mild, moderate, severe, life-threatening), onset date, source
- **ActiveCondition**: ICD-10 code, description, onset date, clinical status
- **ResolvedCondition**: ICD-10 code, description, onset date, resolution date
- **InsurancePolicy**: Payer name, member ID, group number, coverage type, effective dates, subscriber relationship
- **MedicalHistoryEntry**: Type (surgical, family, social), description, date, notes

### Invariants

- MRN is unique across the system and cannot be reassigned
- A patient must have at least one form of contact information
- Allergies cannot be duplicated for the same substance
- At most one primary insurance policy is active at a time
- Date of birth cannot be in the future
- Problem list entries must have valid ICD-10 codes
- All changes to the patient record are audit-logged with timestamp and user identity

## Domain Events

| Event | Trigger | Consumers |
|-------|---------|-----------|
| PatientRegistered | New patient created | Clinical, Insurance, Telemedicine |
| PatientDemographicsUpdated | Demographics changed | All contexts with patient projections |
| PatientMerged | Duplicate records merged | All contexts (must update references) |
| ProblemListUpdated | Condition added or resolved | Clinical, Pharmacy |
| AllergyRecorded | New allergy documented | Clinical, Pharmacy, Diagnostics |
| InsurancePolicyUpdated | Coverage changed | Insurance & Claims |

## Integration with Other Contexts

### Patient Records -> Clinical Workflow (Customer-Supplier)

The Clinical Workflow context subscribes to PatientRegistered, ProblemListUpdated, and AllergyRecorded events. It maintains a local PatientSummary projection with clinically relevant data (MRN, name, DOB, allergies, active problems).

### Patient Records -> Insurance & Claims (Customer-Supplier)

The Insurance context consumes insurance policy information and patient demographics to verify eligibility and submit claims. It maintains a local InsuredMember projection with billing-relevant data.

### Patient Records -> Pharmacy & Medication (Customer-Supplier)

The Pharmacy context subscribes to AllergyRecorded and ProblemListUpdated events to maintain current allergy and condition data for drug interaction checking.

### Patient Records -> Telemedicine (Customer-Supplier)

The Telemedicine context needs basic patient identity and contact information to schedule virtual visits and send session links.

## Key Workflows

### Patient Registration

1. Registration clerk or patient self-service portal enters demographics
2. System checks for potential duplicates (name + DOB match, insurance member ID match)
3. If no duplicate found, system assigns MRN and creates PatientRecord aggregate
4. PatientRegistered event published
5. Downstream contexts create local projections

### Duplicate Detection and Merge

1. System or staff identifies potential duplicate patients
2. Clinical review confirms duplicates
3. Records are merged: surviving MRN retains all data, retired MRN is deactivated
4. PatientMerged event published with surviving and retired MRNs
5. All contexts update their references from retired MRN to surviving MRN

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) -- Patient Records is one of six medical bounded contexts
- [Entities](../entities/) -- Patient is a core entity with MRN identity
- [Value Objects](../value-objects/) -- Address, Allergy, ICD10Code are value objects within the PatientRecord aggregate
- [Aggregates](../aggregates/) -- PatientRecord is an aggregate root
- [Domain Events](../domain-events/) -- Patient Records publishes identity and demographic events
- [Regulatory Compliance](../regulatory-compliance/) -- Patient data is subject to HIPAA privacy and security rules
- [Medical Standards](../medical-standards/) -- ICD-10 codes, SNOMED CT terms, and FHIR Patient resource inform the data model

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 14. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 2. Addison-Wesley, 2013.
- HL7 International. "FHIR R4 Patient Resource." https://hl7.org/fhir/patient.html
- HL7 International. "FHIR R4 Condition Resource." https://hl7.org/fhir/condition.html
- U.S. Department of Health and Human Services. "HIPAA Privacy Rule." 45 CFR Part 164.
- AHIMA. "Patient Identity Integrity." Journal of AHIMA, 2014.
