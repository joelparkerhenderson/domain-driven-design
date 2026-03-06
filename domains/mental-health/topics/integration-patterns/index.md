# Integration Patterns in the Mental Health Domain

## Overview

Integration patterns define how bounded contexts collaborate, share data, and
maintain their boundaries while working together to deliver mental health care.
Domain-Driven Design identifies several integration patterns, each suited to
different relationship dynamics between contexts. In the mental health domain,
the choice of integration pattern for each context relationship reflects the
clinical workflow dependencies, team structures, and regulatory requirements
that govern mental health care delivery.

## Patterns Applied

### Customer-Supplier

The customer-supplier pattern describes a relationship where one context
(supplier/upstream) provides data or services that another context
(customer/downstream) depends on. The upstream context accommodates the
downstream context's needs within reason, but the upstream context controls
its own model and release schedule.

In the mental health domain, customer-supplier relationships include:

- Clinical Assessment (supplier) to Treatment Planning (customer): Assessment
  provides diagnostic results that Treatment Planning needs to create
  individualized plans. Assessment controls the format and timing of its
  outputs, but accommodates Treatment Planning's need for DSM-5 codes,
  severity ratings, and formulation summaries.

- Treatment Planning (supplier) to Therapy Management (customer): Treatment
  Planning provides modality selections, session frequency requirements, and
  treatment goals that Therapy Management executes. Treatment Planning
  controls plan structure, but accommodates Therapy Management's need for
  actionable session directives.

- Treatment Planning (supplier) to Medication Management (customer): Treatment
  Planning provides pharmacotherapy recommendations that Medication
  Management translates into prescriptions. Medication Management may push
  back on recommendations that conflict with pharmacological safety rules.

### Published Language

The published language pattern establishes a shared, well-documented data
format that multiple contexts agree to use for communication. The language
is versioned and maintained as a shared artifact.

In the mental health domain, published languages include:

- Clinical event schema: a standardized set of event types and payload
  structures used for domain events flowing between contexts. All contexts
  agree on the structure of events like AssessmentCompleted, SessionCompleted,
  and MedicationPrescribed.

- Diagnostic code format: DSM-5 and ICD-11 diagnostic codes are a published
  language shared between Clinical Assessment, Treatment Planning, and
  Outcomes Measurement. Each context interprets the codes differently but
  agrees on their format.

- Outcome measure schema: standardized instrument scores and change metrics
  shared between Outcomes Measurement and Treatment Planning for treatment
  response evaluation.

### Conformist

The conformist pattern describes a relationship where the downstream context
adopts the upstream context's model without translation. The downstream
context has no influence over the upstream model and simply conforms to
whatever the upstream provides.

In the mental health domain, the conformist pattern applies to:

- Outcomes Measurement conforming to event schemas published by other
  contexts. Outcomes Measurement subscribes to events and accepts their
  structure as-is, building its longitudinal records from the data provided
  without requesting changes to event formats.

### Partnership

The partnership pattern describes a relationship where two contexts coordinate
closely, with both sides accommodating the other's needs through mutual
agreement. Changes to shared interfaces are negotiated collaboratively.

In the mental health domain, the partnership pattern applies to:

- Crisis Intervention and all other contexts: during a crisis episode, Crisis
  Intervention needs real-time information from other contexts (current
  medications, recent assessments, treatment history) and other contexts need
  to respond to crisis events (cancel sessions, review medications, escalate
  plans). Both sides must accommodate each other's needs for patient safety.

### Anti-Corruption Layer

The anti-corruption layer pattern protects a bounded context from external
models that do not align with its ubiquitous language. The ACL translates
between external and internal representations.

In the mental health domain, ACLs are used at all external system boundaries:

- EHR integration: translates HL7 FHIR resources into domain-specific models.
- Pharmacy integration: translates NCPDP SCRIPT messages into the Medication
  Management context's model.
- Insurance integration: translates payer authorization formats into the
  Treatment Planning context's model.
- Crisis hotline integration: translates 988 Lifeline protocols into the
  Crisis Intervention context's model.

### Open Host Service

The open host service pattern exposes a bounded context's capabilities through
a well-defined protocol that external consumers can use without special
negotiation.

In the mental health domain:

- Medication Management exposes an open host service for drug interaction
  checking that can be consumed by Crisis Intervention, Treatment Planning,
  or external systems using a standardized request-response protocol.

### Shared Kernel

The shared kernel pattern involves two contexts sharing a small, explicitly
defined subset of the domain model. Changes to the shared kernel require
agreement from both contexts.

In the mental health domain, shared kernels are used sparingly:

- Patient identity: a minimal shared kernel containing patient identifier,
  demographic summary, and consent status is shared across all contexts.
  This kernel is kept as small as possible to minimize coupling.

## Pattern Selection Criteria

The choice of integration pattern depends on:

- Clinical workflow dependency: tightly coupled clinical workflows
  (assessment to treatment planning) use customer-supplier.
- Team autonomy needs: contexts with independent teams use published
  language or conformist patterns.
- Safety requirements: patient safety concerns (crisis intervention) demand
  partnership patterns with tight coordination.
- External system control: systems outside the organization's control
  require anti-corruption layers.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 14 on context map patterns.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 13 on integration.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapters 8-9 on integration patterns.
