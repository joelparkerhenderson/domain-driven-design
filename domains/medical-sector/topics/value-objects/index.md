# Value Objects in Medical Practice

## Overview

A value object is an immutable domain object defined entirely by its attributes rather than by a unique identity. Two value objects with identical attributes are interchangeable. In medical practice, value objects represent the precise, well-defined concepts that carry clinical meaning -- diagnosis codes, dosages, vital sign measurements, billing codes, and address information.

## Why Value Objects Matter in Medicine

Medical data is rich with concepts that are naturally value objects:

- An ICD-10 diagnosis code "E11.9" (Type 2 diabetes without complications) is the same concept wherever it appears
- A dosage of "500mg oral twice daily" has the same meaning regardless of which prescription contains it
- A blood pressure reading of "120/80 mmHg" is defined by its values, not by an identity
- A CPT code "99213" (office visit, established patient, moderate complexity) has universal meaning

Value objects provide type safety, self-validation, and domain expressiveness. Instead of passing raw strings and numbers, the domain model uses richly-typed objects that enforce their own invariants.

## Key Value Objects in the Medical Domain

### Clinical Value Objects

- **ICD10Code**: Code string, description, category; validates against the ICD-10-CM code set
- **Diagnosis**: ICD10Code, onset date, status (active, resolved), type (principal, secondary)
- **VitalSigns**: Temperature, blood pressure (systolic/diastolic), heart rate, respiratory rate, oxygen saturation, weight; each with unit of measure
- **ChiefComplaint**: Description text, onset date, severity; recorded in the patient's words
- **Allergy**: Substance, reaction type, severity (mild, moderate, severe, life-threatening), onset date

### Medication Value Objects

- **Dosage**: Amount, unit, route (oral, IV, topical), frequency, duration, PRN indicator
- **RxNormCode**: Normalized drug identifier maintained by the National Library of Medicine
- **NDCCode**: National Drug Code identifying labeler, product, and package size
- **DrugInteraction**: Interacting drugs, severity level, clinical effect description, recommendation
- **FormularyTier**: Tier level (1-4), copay amount, prior authorization required flag

### Diagnostic Value Objects

- **LOINCCode**: Code, component, property, timing, system, scale, method; identifies lab observations
- **LabResultValue**: Measured value, unit, reference range (low, high), abnormal flag
- **SpecimenType**: Type (blood, urine, tissue), collection method, required volume
- **ImagingModality**: Type (X-ray, CT, MRI, ultrasound, PET), body region, contrast required

### Insurance Value Objects

- **CPTCode**: Procedure code, description, relative value unit (RVU), modifier
- **InsurancePolicy**: Payer name, member ID, group number, coverage type, effective dates
- **CoverageStatus**: Eligible flag, copay amount, deductible remaining, coinsurance percentage
- **PlaceOfService**: Code and description (office, inpatient, telehealth, home)

### Administrative Value Objects

- **Address**: Street, city, state, postal code, country; validates postal code format
- **ContactInfo**: Phone numbers, email, preferred contact method
- **NPI**: National Provider Identifier, 10-digit number with check digit validation
- **DEANumber**: DEA registration number with prefix and check digit validation

## Value Object Design Principles

- **Immutability**: Once created, a value object's attributes never change; create a new instance to represent different values
- **Equality by value**: Two value objects are equal if all their attributes are equal
- **Self-validation**: Value objects validate their own invariants at construction (e.g., ICD-10 code format, valid dosage ranges)
- **Side-effect-free behavior**: Methods on value objects return new value objects rather than modifying state
- **Conceptual wholeness**: Group attributes that belong together (systolic and diastolic blood pressure are always paired)

## Relationships to Other Topics

- [Entities](../entities/) -- Entities contain value objects as their attributes
- [Aggregates](../aggregates/) -- Value objects are internal components of aggregates
- [Domain Events](../domain-events/) -- Domain events carry value objects as payload data
- [Ubiquitous Language](../ubiquitous-language/) -- Value object names come from the ubiquitous language
- [Medical Standards](../medical-standards/) -- Standards like ICD-10, LOINC, CPT, and RxNorm inform value object design
- [Anti-Corruption Layer](../anti-corruption-layer/) -- ACLs translate external data into internal value objects

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 5. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 6. Addison-Wesley, 2013.
- Millett, Scott & Tune, Nick. "Patterns, Principles, and Practices of Domain-Driven Design." Chapter 18. Wrox, 2015.
- WHO. "International Classification of Diseases, 10th Revision (ICD-10)." https://www.who.int/classifications/icd/
- AMA. "Current Procedural Terminology (CPT)." https://www.ama-assn.org/practice-management/cpt
- NLM. "RxNorm Overview." https://www.nlm.nih.gov/research/umls/rxnorm/
