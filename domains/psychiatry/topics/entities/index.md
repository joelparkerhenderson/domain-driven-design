# Entities in the Psychiatry Domain

## Overview

An entity is a domain object defined by its unique identity rather than its attributes. Entities have a lifecycle during which their attributes may change, but their identity remains constant. In the psychiatry domain, entities represent the persistent clinical actors and artifacts that are tracked across time, encounters, and care episodes. A patient remains the same patient even as their diagnoses, medications, and treatment status change. A clinician remains the same clinician even as their credentials, caseload, and schedule evolve.

## Characteristics

### Identity

Every entity has a unique identifier that distinguishes it from all other entities of the same type. In psychiatry, identifiers must be carefully managed because patients may present to multiple facilities, use different names, or have records in multiple systems. The entity's identity is the anchor point for all associated clinical data.

### Lifecycle

Entities are created, modified, and sometimes deactivated over time. Their lifecycle reflects real clinical processes. A Treatment Plan entity is created during initial evaluation, modified as clinical status changes, and closed when treatment goals are achieved or the patient discharges. The entity persists across these state transitions, maintaining continuity.

### Mutability

Unlike value objects, entities are mutable. A Patient entity's address, insurance, emergency contacts, and clinical status change throughout their care. The entity's identity ensures that all changes are tracked against the same individual.

## Key Entities by Bounded Context

### Diagnostic Assessment Context

**Patient (within this context)**
- Identity: Patient ID (system-assigned unique identifier)
- Key attributes: demographics, presenting concerns, diagnostic history reference, current evaluation status
- Lifecycle: Created at first contact, persists throughout all care episodes

**Clinician (Evaluator)**
- Identity: Clinician ID
- Key attributes: name, credentials (MD, DO, NP, PA), license number, specialty certifications, evaluation privileges
- Lifecycle: Created upon credentialing, updated with credential renewals

**Rating Scale Administration**
- Identity: Administration ID
- Key attributes: instrument type, administration date, raw scores, calculated subscale scores, administering clinician reference
- Lifecycle: Created at administration, immutable after scoring is finalized

### Medication Management Context

**Prescribing Clinician**
- Identity: Clinician ID
- Key attributes: prescriptive authority level, DEA registration, state license, formulary access permissions
- Lifecycle: Created upon credentialing, updated with authority changes

**Patient (within this context)**
- Identity: Patient ID
- Key attributes: allergy profile, current weight, renal function status, hepatic function status, metabolizer status (if pharmacogenomic testing completed), active medication references
- Lifecycle: Active throughout medication management engagement

**Pharmacogenomic Profile**
- Identity: Profile ID linked to Patient ID
- Key attributes: gene-drug pairs tested, metabolizer phenotypes (CYP2D6, CYP2C19, CYP3A4), test date, laboratory reference
- Lifecycle: Created upon testing, rarely updated (genetic data is stable)

### Psychotherapy Context

**Therapist**
- Identity: Therapist ID
- Key attributes: name, credentials, licensed modalities, supervision status, caseload capacity, therapeutic orientation
- Lifecycle: Created upon credentialing, updated with training and supervision changes

**Patient (within this context)**
- Identity: Patient ID
- Key attributes: therapy history, current therapy course reference, engagement level, homework compliance history
- Lifecycle: Active during therapy engagement, may be reactivated for new therapy courses

### Crisis Management Context

**Crisis Responder**
- Identity: Responder ID
- Key attributes: name, credentials, crisis intervention certification, authority level (can initiate involuntary holds), shift assignment
- Lifecycle: Active during employment, credentials updated per training cycles

**Emergency Contact**
- Identity: Contact ID linked to Patient ID
- Key attributes: contact name, relationship to patient, phone numbers, authorization level (e.g., authorized to receive clinical information), availability
- Lifecycle: Created by patient or guardian, updated as relationships change

### Rehabilitation Services Context

**Case Manager**
- Identity: Case Manager ID
- Key attributes: name, credentials, assigned caseload, specialty areas (employment, housing, skills training)
- Lifecycle: Active during employment, caseload reassigned upon departure

**Service Provider**
- Identity: Provider ID
- Key attributes: organization name, service types offered, geographic coverage, contract status, quality rating
- Lifecycle: Created upon contracting, updated with contract renewals and quality reviews

### Outcomes Measurement Context

**Measurement Administrator**
- Identity: Administrator ID
- Key attributes: name, training certifications for specific instruments, administration privileges
- Lifecycle: Active while certified, updated with recertification

## Entity Design Considerations

### Cross-Context Identity

The Patient entity exists in every bounded context but with different attributes in each. A shared patient identifier links these representations. The identity is shared; the model is not. Each context maintains only the patient attributes relevant to its responsibilities.

### Avoiding Anemic Entities

Entities in the psychiatry domain carry behavior, not just data. A Treatment Plan entity enforces rules about goal modification, review scheduling, and status transitions. It is not merely a data container; it encodes clinical business logic about what constitutes a valid treatment plan state.

### Entity vs. Value Object Decision

The distinction is driven by identity significance. A specific therapy session has identity (it occurred at a particular time with a particular patient); it is an entity. A diagnosis code (F32.1) has no identity independent of its attributes; it is a value object. When in doubt, ask: "Does it matter which specific instance this is, or only what attributes it has?"

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 5 on entities.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 5 on entities and identity.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 4 on entities.
