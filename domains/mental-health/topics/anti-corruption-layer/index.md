# Anti-Corruption Layer in the Mental Health Domain

## Overview

An anti-corruption layer (ACL) is a translation boundary that protects a
bounded context from the models, terminology, and assumptions of external
systems. When a mental health system must integrate with electronic health
records, pharmacy systems, insurance platforms, or crisis hotline networks,
the ACL ensures that foreign concepts are translated into the domain's
ubiquitous language before they enter the bounded context. Without an ACL,
external system models gradually contaminate the domain model, creating
confusion, coupling, and fragility.

## Why ACLs Matter in Mental Health

Mental health systems operate within a complex ecosystem of external systems,
each with its own data model and terminology:

- Electronic Health Record (EHR) systems use HL7 FHIR resources like
  Condition, Encounter, and Observation, which do not map directly to
  mental health concepts like clinical formulation, therapeutic alliance,
  or safety plan.
- Pharmacy benefit managers use NCPDP SCRIPT messages with pharmacy-centric
  concepts that differ from the prescribing clinician's perspective on
  psychotropic medication management.
- Insurance systems use CPT codes and ICD-10 codes for billing, which are
  administrative classifications rather than clinical models.
- The 988 Suicide & Crisis Lifeline uses its own protocols and data formats
  for crisis call routing and follow-up.

Each of these external models carries assumptions that conflict with the
mental health domain's internal models. The ACL prevents these conflicts
from propagating inward.

## ACL Locations in the Mental Health Domain

### Clinical Assessment Context <-> EHR Systems

The Clinical Assessment Context receives patient history, prior diagnoses,
and demographic information from EHR systems. The ACL translates HL7 FHIR
Condition resources into the domain's Assessment-oriented model, mapping
external diagnosis codes to the domain's DiagnosticCriteria value objects
and filtering out EHR-specific metadata that has no clinical assessment
relevance.

### Treatment Planning Context <-> Insurance/Payer Systems

Insurance systems require prior authorization for certain care levels and
treatment modalities. The ACL translates authorization requests and responses
between the domain's TreatmentPlan aggregate and the payer's authorization
model. The domain model does not adopt insurance terminology; instead, the
ACL maps treatment goals and modalities to CPT codes and medical necessity
criteria at the boundary.

### Medication Management Context <-> Pharmacy Systems

The Medication Management Context integrates with pharmacy benefit managers
for formulary checks, drug utilization review, and electronic prescribing.
The ACL translates NCPDP SCRIPT messages into the domain's Prescription
aggregate model, converting pharmacy-centric concepts (NDC codes, days
supply, dispense quantity) into clinically meaningful terms (medication name,
therapeutic dose, titration schedule).

### Crisis Intervention Context <-> External Crisis Services

The Crisis Intervention Context integrates with the 988 Lifeline and
emergency department systems. The ACL translates external crisis protocols
and triage categories into the domain's CrisisEpisode aggregate, mapping
external severity classifications to the domain's risk assessment model.

### Outcomes Measurement Context <-> Quality Reporting Systems

The Outcomes Measurement Context exports data to quality reporting
organizations (HEDIS, NQF). The ACL translates internal outcome records
into the specific measure specifications required by each reporting body,
without allowing reporting requirements to shape the internal outcome
model.

## ACL Design Principles

In the mental health domain, ACLs follow these design principles:

- Translate, never adopt: external terminology stays outside the boundary.
  The domain model never references NDC codes directly; it uses its own
  Medication value object.
- Fail loudly: when an external system sends data that cannot be translated,
  the ACL raises an explicit error rather than silently dropping or
  misinterpreting information. In clinical systems, silent failures can
  compromise patient safety.
- Version tolerance: external systems evolve their APIs and data formats.
  The ACL absorbs versioning changes so the domain model remains stable.
- Audit trail: every translation through the ACL is logged for regulatory
  compliance and clinical accountability.

## Implementation Considerations

ACLs in the mental health domain are typically implemented as adapter
services that sit at the boundary of each bounded context. They contain
mapping logic, validation rules, and error handling specific to each
external integration. The ACL is not part of the domain model; it is
infrastructure that serves the domain model.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 14 on anti-corruption layers.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 13 on integrating bounded contexts.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 9 on communication patterns.
- HL7 International. FHIR R4 Specification. https://www.hl7.org/fhir/
- National Council for Prescription Drug Programs. NCPDP SCRIPT Standard. NCPDP, 2023.
