# Context Map in the Psychiatry Domain

## Overview

A context map is a visual and conceptual representation of the relationships between bounded contexts in a domain. It documents how contexts communicate, which context holds authority over shared concepts, and what translation mechanisms exist at boundaries. In the psychiatry domain, the context map reveals a clinical information flow that begins with diagnostic assessment and propagates through treatment contexts, with outcomes measurement serving as a downstream consumer of data from all other contexts.

## Context Relationships

### Diagnostic Assessment to Medication Management (Customer-Supplier)

The Diagnostic Assessment Context acts as the upstream supplier of diagnostic information. The Medication Management Context is the downstream customer that consumes diagnoses to inform prescribing decisions. The diagnostic context publishes Diagnosis Assigned events that the medication context subscribes to. The medication context cannot modify diagnostic data; it receives a read-only projection of the patient's diagnostic profile.

Translation: Diagnostic codes (DSM-5-TR) are translated into pharmacological indication categories within the medication context. A diagnosis of "Major Depressive Disorder, Recurrent, Severe" in the diagnostic context maps to an indication category of "antidepressant therapy indicated" in the medication context.

### Diagnostic Assessment to Psychotherapy (Customer-Supplier)

The Diagnostic Assessment Context supplies diagnoses to the Psychotherapy Context, which uses them to select appropriate therapeutic modalities and treatment protocols. A diagnosis of borderline personality disorder triggers consideration of DBT protocols, while a diagnosis of PTSD triggers consideration of prolonged exposure or EMDR protocols.

Translation: DSM-5-TR diagnoses are mapped to evidence-based treatment protocol recommendations within the psychotherapy context using clinical practice guideline mappings.

### Crisis Management as Shared Kernel

The Crisis Management Context maintains a shared kernel relationship with both the Diagnostic Assessment and Medication Management contexts. During a psychiatric emergency, the crisis context needs immediate access to current diagnoses and active medications. This shared kernel is deliberately narrow, limited to patient identification, active diagnoses, and current medication list.

The shared kernel is governed by a formal agreement: changes to the shared data structures require approval from all three context teams. This constraint is accepted because crisis situations demand real-time access to critical clinical information.

### All Contexts to Outcomes Measurement (Conformist)

The Outcomes Measurement Context acts as a conformist downstream consumer. It accepts data from all other contexts in their published formats without imposing its own model requirements upstream. This is appropriate because outcomes measurement must accommodate whatever clinical data is generated, and imposing measurement requirements on clinical workflows would compromise care delivery.

Data flows:
- Diagnostic Assessment publishes evaluation completion events with diagnostic summaries.
- Medication Management publishes prescription change events and adverse event reports.
- Psychotherapy publishes session completion events with progress notes.
- Crisis Management publishes crisis resolution events with disposition data.
- Rehabilitation Services publishes milestone achievement events.

### Rehabilitation Services to Psychotherapy (Partnership)

The Rehabilitation Services Context and Psychotherapy Context maintain a partnership relationship. Both contexts coordinate on patients receiving concurrent psychotherapy and psychosocial rehabilitation. They share a mutual interest in functional improvement and coordinate treatment goals. Neither context dominates; they negotiate integration points collaboratively.

### External System Integrations

- **EHR Systems**: Anti-corruption layer translates between psychiatric domain model and generic EHR data formats (HL7 FHIR, CDA).
- **Pharmacy Systems**: Anti-corruption layer translates between medication management model and pharmacy dispensing systems (NCPDP SCRIPT).
- **Insurance/Billing Systems**: Anti-corruption layer translates between diagnostic codes and billing claim formats (837P).
- **State Reporting Systems**: Anti-corruption layer translates crisis events into mandated state reporting formats.

## Communication Patterns

- **Synchronous**: Used only for the Crisis Management shared kernel, where real-time data access is clinically necessary.
- **Asynchronous (Events)**: Used for all other inter-context communication. Domain events are published by upstream contexts and consumed by downstream contexts with eventual consistency.
- **Anti-Corruption Layers**: Used at all external system boundaries to prevent external terminology from leaking into the psychiatry domain model.

## Map Governance

The context map is maintained as a living document, reviewed quarterly by a cross-functional team including representatives from each bounded context. Changes to context relationships require formal review to ensure that upstream changes do not break downstream consumers and that shared kernel agreements remain appropriately scoped.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 14 on context maps.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 3 on context mapping.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 8 on context map patterns.
