# Anti-Corruption Layer in Hospital Management

## Overview

An anti-corruption layer (ACL) is a translation boundary that protects a bounded context's domain model from the concepts, data structures, and semantics of external systems. It acts as an adapter that converts between the external system's model and the internal domain model, preventing external complexity from leaking into the domain.

## Relevance to Hospital Management

Hospital systems integrate with numerous external systems, each with its own data formats, terminology, and protocols:

- Insurance company APIs with proprietary claim formats
- Laboratory information systems using HL7 v2 messages
- Pharmacy systems with NDC drug codes
- Medical device interfaces with vendor-specific protocols
- Government reporting systems (CMS, state health departments)
- Health Information Exchanges (HIEs)

Without anti-corruption layers, the hospital's domain model would be polluted by dozens of external formats, making it fragile and difficult to evolve.

## Key Concepts

### How an ACL Works

The anti-corruption layer sits between the external system and the bounded context:

1. **Receive** external data in the external system's format
2. **Translate** data into internal domain concepts using adapters and translators
3. **Deliver** clean domain objects to the bounded context
4. **Reverse translate** when sending data back to the external system

### ACL Components

- **Facade**: Simplified interface that hides external system complexity
- **Adapter**: Converts external data structures to internal domain objects
- **Translator**: Maps between external terminology and ubiquitous language
- **Gateway**: Handles communication protocols (REST, SOAP, HL7, EDI)

## Hospital ACL Examples

### Insurance APIs → Billing Context

External insurance systems use EDI 837/835 formats with proprietary codes. The ACL:

- Translates EDI claim responses into domain `ClaimResponse` value objects
- Maps proprietary denial reason codes to internal `DenialReason` enum
- Converts external `SubscriberID` to internal `InsurancePolicyNumber`
- Shields billing domain from changes in individual payer APIs

### Laboratory Systems → Clinical/EMR Context

External lab systems send results via HL7 v2 messages. The ACL:

- Parses HL7 ORU messages into domain `LabResult` entities
- Maps LOINC codes to internal `TestType` value objects
- Converts lab-specific units to standardized clinical units
- Translates abnormal flag codes to internal `ResultInterpretation`

### Pharmacy Systems → Clinical/EMR Context

External pharmacy systems use NCPDP SCRIPT for e-prescribing. The ACL:

- Translates SCRIPT messages into domain `PrescriptionFulfillment` events
- Maps NDC drug codes to internal `Medication` value objects
- Converts pharmacy dispensing status to domain `FulfillmentStatus`

### Government Reporting → Billing Context

CMS and state agencies require specific reporting formats. The ACL:

- Translates internal encounter data into required CMS formats
- Maps internal diagnosis codes to the specific ICD-10 version required
- Generates CMS-1500 or UB-04 forms from domain claim objects
- Handles format version changes without impacting the domain model

### Medical Devices → Clinical/EMR Context

Medical devices (monitors, ventilators, infusion pumps) send data via proprietary interfaces. The ACL:

- Translates device-specific vital sign formats into domain `VitalSigns` value objects
- Normalizes measurement units across different device manufacturers
- Converts device alarm codes into domain `ClinicalAlert` events

## Design Considerations

### When to Use an ACL

- Integrating with external systems whose models differ significantly from the domain
- When the external system is controlled by another organization and may change independently
- When multiple external systems serve the same purpose (e.g., multiple insurance payers)
- When the external model would distort the domain language if adopted directly

### When Not to Use an ACL

- When the external model closely matches the domain model (use Conformist pattern instead)
- When two internal contexts are tightly aligned (use Shared Kernel instead)
- When the cost of translation exceeds the benefit (evaluate case by case)

## Relationships to Other Topics

- [Context Map](../context-map/) — ACLs appear as a pattern on the context map
- [Bounded Contexts](../bounded-contexts/) — ACLs protect context boundaries
- [Integration Patterns](../integration-patterns/) — ACL is one of several integration patterns
- [Healthcare Standards](../healthcare-standards/) — HL7, FHIR, and other standards define the external formats that ACLs translate
- [Domain Events](../domain-events/) — ACLs often translate external events into domain events

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 14: Maintaining Model Integrity. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 3: Context Maps. Addison-Wesley, 2013.
- Hohpe, Gregor & Woolf, Bobby. "Enterprise Integration Patterns." Addison-Wesley, 2003.
- Millett, Scott & Tune, Nick. "Patterns, Principles, and Practices of Domain-Driven Design." Chapter 7: Context Mapping. Wrox, 2015.
- HL7 International. "HL7 Version 2 Product Suite." https://www.hl7.org/implement/standards/product_brief.cfm?product_id=185
