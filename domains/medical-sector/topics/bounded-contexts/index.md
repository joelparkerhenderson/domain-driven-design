# Bounded Contexts in Medical Practice

## Overview

A bounded context is a logical boundary within which a particular domain model is consistent and terms have specific, unambiguous meaning. In medical practice, bounded contexts separate the concerns of patient records, clinical workflows, diagnostics, pharmacy, insurance, and telemedicine so that each area can evolve independently while maintaining well-defined integration points.

## Why Bounded Contexts Matter in Medicine

Medical practice involves many specialized disciplines, each with its own vocabulary and workflows. Without bounded contexts:

- The term "order" would be ambiguous (clinical order vs. pharmacy order vs. imaging order)
- Changes to insurance claim processing could break clinical decision support logic
- A single monolithic model would grow unmanageable as clinical complexity increases
- Regulatory compliance requirements (HIPAA, HITECH) would be difficult to isolate and enforce

## The Six Medical Bounded Contexts

### Patient Records Context

Owns the authoritative patient identity, demographics, medical history, problem lists, and allergy records. This context is the source of truth for "who the patient is" across the system.

### Clinical Workflow Context

Manages clinical encounters, order entry, clinical decision support rules, care plans, and referrals. This context models the physician's workflow from patient intake through diagnosis and treatment planning.

### Diagnostics & Imaging Context

Handles lab orders, specimen tracking, lab results, radiology orders, imaging studies, pathology reports, and DICOM image management. This context has its own specialized vocabulary around specimens, modalities, and reporting.

### Pharmacy & Medication Context

Manages prescriptions, drug interaction checking, formulary management, medication administration records, and controlled substance tracking. The concept of "medication" in this context includes dispensing details that do not exist in the clinical context.

### Insurance & Claims Context

Handles insurance eligibility verification, prior authorization, claims submission, adjudication tracking, explanation of benefits, and denial management. This context translates clinical services into billing codes and financial transactions.

### Telemedicine Context

Manages virtual visit scheduling, video session management, remote patient monitoring device integration, and telehealth-specific consent and documentation. This context has unique concepts around connectivity, device data, and virtual encounter workflows.

## Boundary Characteristics

Each bounded context in the medical domain exhibits these properties:

- **Own ubiquitous language**: "Patient" in the Patient Records context includes full demographics; in the Insurance context, it is an "insured member" with policy details
- **Own data store**: Each context manages its own persistence, avoiding shared databases
- **Own team**: Ideally, one team per context to minimize coordination overhead
- **Explicit integration**: Communication between contexts uses domain events, APIs, or anti-corruption layers
- **Independent deployment**: Each context can be versioned and deployed independently

## Context Boundaries and Medical Terminology

The same real-world concept takes different shapes across contexts:

| Real-World Concept | Patient Records | Clinical Workflow | Insurance & Claims |
|---------------------|----------------|-------------------|--------------------|
| Patient | Full demographics, MRN | Encounter participant | Insured member, subscriber |
| Diagnosis | Problem list entry | Assessment finding | ICD-10 claim line |
| Medication | Allergy/history entry | Order/prescription | Covered drug, NDC code |
| Provider | N/A | Treating clinician | Billing provider, NPI |

## Relationships to Other Topics

- [Strategic Design](../strategic-design/) -- Bounded contexts are the primary output of strategic design
- [Context Map](../context-map/) -- Shows relationships between bounded contexts
- [Ubiquitous Language](../ubiquitous-language/) -- Each context defines its own language
- [Anti-Corruption Layer](../anti-corruption-layer/) -- Protects contexts from external model corruption
- [Integration Patterns](../integration-patterns/) -- Defines how contexts communicate
- [Domain Events](../domain-events/) -- The primary mechanism for cross-context communication

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 14. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 2. Addison-Wesley, 2013.
- Vernon, Vaughn. "Domain-Driven Design Distilled." Chapter 4. Addison-Wesley, 2016.
- Millett, Scott & Tune, Nick. "Patterns, Principles, and Practices of Domain-Driven Design." Chapters 6-8. Wrox, 2015.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 7. O'Reilly, 2021.
