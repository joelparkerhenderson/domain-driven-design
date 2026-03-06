# Value Objects in Hospital Management

## Overview

A value object is a domain object that is defined by its attributes rather than a unique identity. Value objects are immutable — once created, they cannot be changed. If a different value is needed, a new value object is created. Two value objects with the same attributes are considered equal regardless of which instance is which.

## Relevance to Hospital Management

Healthcare is rich with concepts that are best modeled as value objects: blood types, dosages, vital signs, addresses, diagnosis codes, and monetary amounts. These are descriptive attributes that do not need to be tracked individually over time — a blood type of "O-positive" is the same everywhere it appears.

## Key Concepts

### Immutability

Value objects cannot be modified after creation. To change a patient's address, you replace the old `Address` value object with a new one rather than mutating the existing one. This prevents subtle bugs where shared references cause unintended side effects.

### Equality by Attributes

Two `Dosage` objects with the same amount, unit, frequency, and route are equal, even if they are separate instances. This is in contrast to entities, where equality is based on identity.

### Self-Validation

Value objects validate their own construction. A `BloodType` value object rejects invalid types at creation time; a `Dosage` value object ensures the amount is positive and the unit is recognized.

### Side-Effect-Free Behavior

Value objects can contain methods, but those methods must not change the object. Instead, they return new value objects. For example, `dosage.double()` returns a new `Dosage` with twice the amount.

## Hospital Value Objects

### BloodType

- **Attributes**: Group (A, B, AB, O), Rh factor (positive, negative)
- **Validation**: Only valid group/Rh combinations are accepted
- **Usage**: Patient aggregate, transfusion compatibility checks
- **Example**: `BloodType("O", "negative")` — universal donor

### Address

- **Attributes**: Street, city, state, postal code, country
- **Validation**: Postal code format matches country
- **Usage**: Patient demographics, provider offices, department locations
- **Example**: `Address("123 Main St", "Springfield", "IL", "62701", "US")`

### Dosage

- **Attributes**: Amount, unit (mg, mL, units), frequency (BID, TID, QID), route (oral, IV, topical)
- **Validation**: Amount must be positive; unit, frequency, and route must be recognized medical terms
- **Usage**: Prescription aggregate, medication orders
- **Example**: `Dosage(500, "mg", "BID", "oral")` — 500mg twice daily by mouth

### VitalSigns

- **Attributes**: Temperature, blood pressure (systolic/diastolic), heart rate, respiratory rate, oxygen saturation, timestamp
- **Validation**: Values within physiologically possible ranges
- **Usage**: Clinical encounters, emergency triage, nursing assessments
- **Example**: `VitalSigns(98.6, 120, 80, 72, 16, 98, "2026-03-06T10:30:00Z")`

### DiagnosisCode

- **Attributes**: Code system (ICD-10), code value, description
- **Validation**: Code must be a valid ICD-10 code
- **Usage**: Encounters, claims, problem lists
- **Example**: `DiagnosisCode("ICD-10", "J18.9", "Pneumonia, unspecified organism")`

### Money

- **Attributes**: Amount (decimal), currency code (ISO 4217)
- **Validation**: Amount must not be negative for charges; currency must be valid
- **Usage**: Billing, charges, payments, insurance coverage
- **Example**: `Money(150.00, "USD")`

### DateRange

- **Attributes**: Start date, end date
- **Validation**: Start must not be after end
- **Usage**: Insurance coverage periods, provider availability, appointment time slots
- **Example**: `DateRange("2026-01-01", "2026-12-31")`

### ContactInfo

- **Attributes**: Phone number, email, preferred contact method
- **Validation**: Phone and email format validation
- **Usage**: Patient demographics, emergency contacts, provider contact information

### ESILevel

- **Attributes**: Level (1 through 5)
- **Validation**: Must be an integer between 1 and 5
- **Usage**: Emergency triage assessment
- **Example**: `ESILevel(2)` — emergent, high risk

## Design Benefits

- **Thread safety**: Immutable objects are inherently safe for concurrent access — critical in busy hospital systems
- **Cacheable**: Identical value objects can be shared and cached without risk of modification
- **Testable**: Value objects are easy to test because they have no state changes
- **Expressiveness**: `Dosage(500, "mg", "BID", "oral")` communicates more clearly than four separate primitive fields

## Relationships to Other Topics

- [Entities](../entities/) — Entities contain value objects as attributes
- [Aggregates](../aggregates/) — Value objects are part of aggregates, owned by the aggregate root
- [Ubiquitous Language](../ubiquitous-language/) — Value object names come from the domain vocabulary
- [Healthcare Standards](../healthcare-standards/) — Standard code systems (ICD-10, CPT, LOINC) are modeled as value objects

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 5: A Model Expressed in Software. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 6: Value Objects. Addison-Wesley, 2013.
- Millett, Scott & Tune, Nick. "Patterns, Principles, and Practices of Domain-Driven Design." Chapter 17: Value Objects. Wrox, 2015.
