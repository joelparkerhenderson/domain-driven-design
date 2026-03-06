# Context Map in Medical Practice

## Overview

A context map is a visual and conceptual representation of the relationships and interactions between bounded contexts in a system. In medical practice, the context map documents how patient records, clinical workflows, diagnostics, pharmacy, insurance, and telemedicine contexts communicate, which contexts depend on which, and what integration patterns govern their interactions.

## Why Context Maps Matter in Medicine

Medical systems involve numerous interconnected subsystems that must exchange data reliably while respecting each context's autonomy. A context map makes these relationships explicit, revealing:

- Dependencies between contexts (e.g., Claims depends on Clinical Workflow for encounter data)
- Power dynamics (e.g., external insurance payers dictate claim formats)
- Integration risks (e.g., a change in lab result format affects both clinical and pharmacy contexts)
- Opportunities for anti-corruption layers to shield internal models from external system complexity

## Medical Context Map

### Patient Records <-> Clinical Workflow (Customer-Supplier)

The Clinical Workflow context is a downstream consumer of patient identity and medical history. It subscribes to PatientRegistered and ProblemListUpdated events and maintains a local PatientSummary projection containing clinically relevant data (MRN, name, DOB, allergies, active problems).

### Clinical Workflow <-> Diagnostics & Imaging (Customer-Supplier)

The Clinical Workflow context creates diagnostic orders (lab orders, imaging orders). The Diagnostics context processes these orders and publishes LabResultAvailable and ImagingStudyCompleted events back to the clinical context for review and incorporation into the care plan.

### Clinical Workflow <-> Pharmacy & Medication (Partnership)

These two contexts have a close partnership. The Clinical Workflow context creates prescriptions; the Pharmacy context validates them against formulary and drug interaction rules. Both contexts collaborate on medication reconciliation and share a published language for medication concepts.

### Clinical Workflow <-> Insurance & Claims (Customer-Supplier)

The Insurance context is downstream of Clinical Workflow. It consumes encounter completion events, diagnosis codes, and procedure codes to generate and submit claims. The Insurance context does not influence clinical decisions.

### Clinical Workflow <-> Telemedicine (Partnership)

The Telemedicine context coordinates virtual encounters with the Clinical Workflow context. A virtual visit in Telemedicine maps to an encounter in Clinical Workflow, with shared lifecycle events (visit started, visit completed).

### Patient Records <-> Insurance & Claims (Customer-Supplier)

The Insurance context consumes patient demographics and insurance policy information from Patient Records to verify eligibility and submit claims with correct subscriber information.

### Diagnostics & Imaging <-> External Lab Systems (Conformist / ACL)

External reference laboratories and PACS systems impose their own data formats (HL7 v2, DICOM). The Diagnostics context uses anti-corruption layers to translate between external formats and the internal domain model.

### Insurance & Claims <-> External Payers (Conformist)

Insurance payers dictate claim submission formats (837P, 837I), remittance formats (835), and eligibility check protocols (270/271). The Insurance context must conform to these external standards.

### Pharmacy & Medication <-> External Drug Databases (Conformist / ACL)

Drug interaction databases, formulary services, and prescription drug monitoring programs (PDMPs) impose their own APIs and data models. The Pharmacy context uses anti-corruption layers to integrate while keeping its internal model clean.

## Integration Pattern Summary

| Upstream Context | Downstream Context | Pattern | Mechanism |
|------------------|-------------------|---------|-----------|
| Patient Records | Clinical Workflow | Customer-Supplier | Domain events |
| Patient Records | Insurance & Claims | Customer-Supplier | Domain events |
| Clinical Workflow | Diagnostics & Imaging | Customer-Supplier | Domain events, APIs |
| Clinical Workflow | Insurance & Claims | Customer-Supplier | Domain events |
| Clinical Workflow | Pharmacy & Medication | Partnership | Published language |
| Clinical Workflow | Telemedicine | Partnership | Shared events |
| External Labs | Diagnostics & Imaging | ACL | HL7 v2 translation |
| External Payers | Insurance & Claims | Conformist | EDI standards |
| External Drug DBs | Pharmacy & Medication | ACL | API translation |

## Relationships to Other Topics

- [Strategic Design](../strategic-design/) -- Context maps are a key strategic design artifact
- [Bounded Contexts](../bounded-contexts/) -- The nodes in the context map
- [Anti-Corruption Layer](../anti-corruption-layer/) -- Used to protect contexts from external models
- [Integration Patterns](../integration-patterns/) -- The patterns applied between contexts
- [Domain Events](../domain-events/) -- The primary mechanism for event-based integration

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 14. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 3. Addison-Wesley, 2013.
- Brandolini, Alberto. "Introducing EventStorming." Leanpub, 2021.
- Millett, Scott & Tune, Nick. "Patterns, Principles, and Practices of Domain-Driven Design." Chapter 8. Wrox, 2015.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 8. O'Reilly, 2021.
