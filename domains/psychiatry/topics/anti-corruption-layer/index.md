# Anti-Corruption Layer in the Psychiatry Domain

## Overview

An anti-corruption layer (ACL) is a translation boundary that protects a bounded context from the concepts, terminology, and data structures of external systems or other bounded contexts. In the psychiatry domain, ACLs are essential because psychiatric systems must integrate with electronic health record (EHR) systems, pharmacy dispensing systems, insurance billing platforms, and state reporting systems, each of which uses terminology and data structures that differ from the psychiatry domain model.

## Purpose

Without an anti-corruption layer, external system concepts would leak into the psychiatry domain model, corrupting its ubiquitous language and introducing inconsistencies. For example, an insurance billing system represents a psychiatric diagnosis as a billing code with a date of service, while the Diagnostic Assessment Context represents a diagnosis as a clinically validated classification with severity specifiers, course modifiers, onset context, and supporting evidence. If the billing system's simplified representation were allowed to influence the diagnostic model, the clinical richness would be lost.

The ACL translates between the external representation and the internal domain model, ensuring that each bounded context maintains its own language and model integrity.

## ACL Implementations in Psychiatry

### EHR System Integration ACL

Psychiatry systems commonly integrate with general-purpose EHR systems (Epic, Cerner, Allscripts) that use standardized healthcare data formats.

Translation responsibilities:
- Convert HL7 FHIR Condition resources into domain-specific Diagnosis value objects with full DSM-5-TR specifier detail that FHIR does not natively capture.
- Convert HL7 FHIR MedicationRequest resources into domain-specific Prescription aggregates with psychiatric-specific fields (titration schedule, black box warning acknowledgment, monitoring requirements).
- Convert generic FHIR Encounter resources into context-specific evaluation, session, or crisis event records.
- Map FHIR patient demographics into context-specific patient representations, filtering only the attributes relevant to each bounded context.

### Pharmacy System Integration ACL

The Medication Management Context integrates with pharmacy dispensing systems via NCPDP SCRIPT standards.

Translation responsibilities:
- Convert internal Prescription aggregates into NCPDP SCRIPT NewRx messages for transmission to pharmacies.
- Translate pharmacy dispensing confirmations back into domain events (Medication Dispensed).
- Map internal drug identifiers (using RxNorm concepts) to pharmacy system NDC codes.
- Handle prior authorization requests from pharmacy benefit managers, translating their requirements into domain-specific prior authorization workflow steps.

### Insurance Billing ACL

All contexts generate billable events that must be translated into insurance claim formats.

Translation responsibilities:
- Convert Diagnosis value objects into ICD-10-CM billing codes, selecting the appropriate code from the more granular DSM-5-TR classification.
- Convert clinical service records into CPT procedure codes (90791 for psychiatric evaluation, 90834 for psychotherapy, 90833 for add-on psychotherapy with E/M).
- Map clinical documentation into the structured format required for medical necessity justification.
- Translate insurance authorization decisions back into domain-understandable coverage status updates.

### State Reporting ACL

Crisis Management and Diagnostic Assessment contexts must report certain events to state mental health authorities.

Translation responsibilities:
- Convert involuntary hold records into state-mandated reporting formats, which vary by jurisdiction.
- Translate crisis disposition data into state behavioral health outcome reporting formats.
- Map internal patient identifiers to state-assigned client identifiers for mandated reporting.
- Convert clinical documentation into the structured formats required for civil commitment proceedings.

## Design Principles

### Isolation

The ACL is implemented as a separate component that sits between the external system and the bounded context. The bounded context has no direct knowledge of external system data structures. All communication passes through the ACL, which performs bidirectional translation.

### Faithfulness to the Domain Model

The ACL always translates external data into domain model terms, never the reverse. If an external system provides data that does not map cleanly to the domain model, the ACL either enriches the data using domain rules or flags it for manual review. The domain model is never simplified to accommodate external system limitations.

### Explicit Failure Handling

When translation fails (e.g., an unrecognized diagnosis code from an external system), the ACL surfaces the failure explicitly rather than silently dropping or defaulting data. In psychiatry, silent data loss can have clinical safety implications.

### Versioning

External system formats change over time (ICD-10 to ICD-11 transitions, DSM-5 to DSM-5-TR updates). The ACL maintains version-aware translation logic, supporting multiple format versions simultaneously during transition periods.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 14 on anti-corruption layers.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 3 on integration with ACLs.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 9 on anti-corruption layers.
- HL7 International. *FHIR R4 Specification*. https://hl7.org/fhir/R4/.
