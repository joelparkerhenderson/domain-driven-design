# Entities - Otolaryngology Domain

## Overview

Entities are domain objects defined by a unique identity that persists over time, even as their attributes change. In the otolaryngology domain, entities represent clinical objects that have a distinct lifecycle and must be tracked individually. Unlike value objects, which are interchangeable when their attributes match, entities maintain identity across state changes, making them essential for modeling patients, clinical encounters, surgical cases, and treatment protocols.

## Entity Characteristics in Otolaryngology

Healthcare entities have several distinguishing characteristics:

- **Persistent identity**: A surgical case retains its identity from scheduling through post-operative follow-up, even as every attribute (status, findings, notes) changes over its lifecycle.
- **Lifecycle tracking**: ENT clinical objects transition through well-defined states. A sleep evaluation moves from screening to diagnosis to treatment to outcome measurement.
- **Audit requirements**: Healthcare regulations require tracking who created, modified, and accessed clinical entities, making identity and lifecycle management critical.
- **Temporal relevance**: Many ENT entities carry temporal context. An audiogram is meaningful at a point in time, and the same patient will have multiple audiograms over their hearing journey.

## Key Entities by Bounded Context

### Clinical Assessment Context

**ENT Encounter** (Aggregate Root Entity)
- Identity: Encounter ID (unique across the system)
- Attributes: Date, provider, patient reference, encounter type (new, follow-up, urgent), status (scheduled, in-progress, completed, signed)
- Lifecycle: Scheduled, checked-in, in-progress, documentation-complete, signed, amended

**Examination Finding**
- Identity: Finding ID (unique within the encounter)
- Attributes: Anatomical site, finding type, description, severity, laterality, clinical significance
- Lifecycle: Recorded during examination, may be amended until encounter is signed

### Surgical Management Context

**Surgical Case** (Aggregate Root Entity)
- Identity: Case ID (unique across the system)
- Attributes: Procedure type, scheduled date, surgeon, patient reference, case status, priority
- Lifecycle: Requested, scheduled, pre-operative-cleared, in-surgery, post-operative, closed

**Operative Note**
- Identity: Note ID (unique to the surgical case)
- Attributes: Procedure performed, findings, technique description, specimens, estimated blood loss, complications
- Lifecycle: Created at procedure completion, finalized within 24 hours, may be amended with addendum

### Voice Disorders Context

**Voice Assessment** (Aggregate Root Entity)
- Identity: Assessment ID
- Attributes: Date, clinician, assessment type (initial, follow-up, post-surgical), referral source
- Lifecycle: Scheduled, in-progress, completed, reviewed

**Voice Therapy Protocol**
- Identity: Protocol ID
- Attributes: Diagnosis, therapy goals, session frequency, duration, assigned therapist, status
- Lifecycle: Created, active, on-hold, completed, discontinued

### Sleep Disorders Context

**Sleep Evaluation** (Aggregate Root Entity)
- Identity: Evaluation ID
- Attributes: Patient reference, referral reason, evaluation status, treatment pathway
- Lifecycle: Initiated, screened, study-ordered, diagnosed, treatment-initiated, follow-up, outcome-measured

**CPAP Prescription**
- Identity: Prescription ID
- Attributes: Device type, pressure settings, mask type, initiation date, prescribing provider
- Lifecycle: Prescribed, dispensed, titrated, active, modified, discontinued

### Allergy Management Context

**Allergy Profile** (Aggregate Root Entity)
- Identity: Profile ID (one per patient in this context)
- Attributes: Patient reference, allergy history, sensitization summary, treatment status
- Lifecycle: Created at initial evaluation, maintained longitudinally

**Immunotherapy Protocol**
- Identity: Protocol ID
- Attributes: Allergen extract formulation, dosing schedule, current phase (build-up, maintenance), start date
- Lifecycle: Prescribed, build-up-phase, maintenance-phase, completed, discontinued

### Hearing Services Context

**Audiological Record** (Aggregate Root Entity)
- Identity: Record ID (one per patient in this context)
- Attributes: Patient reference, hearing status summary, device history, rehabilitation status
- Lifecycle: Created at first audiological encounter, maintained indefinitely

**Hearing Aid Fitting**
- Identity: Fitting ID
- Attributes: Device manufacturer, model, ear (left, right, bilateral), fitting date, verification method, programming parameters
- Lifecycle: Ordered, fitted, verified, adjusted, active, replaced, discontinued

## Entity Identity Strategies

In the otolaryngology domain, entity identity follows these patterns:

- **System-generated IDs**: Encounter IDs, case IDs, and assessment IDs use system-generated unique identifiers (UUIDs or similar).
- **Natural identifiers**: Where applicable, entities reference natural clinical identifiers (medical record number for patients, accession number for imaging studies).
- **Cross-context references**: When an entity in one context references an entity in another, it uses the referenced entity's ID without importing the full entity model. A Surgical Case references a patient by patient ID, not by embedding the patient's allergy profile.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 5 on entities.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 5 on entity design.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 5 on entity modeling.
