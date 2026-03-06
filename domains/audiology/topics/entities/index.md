# Entities in the Audiology Domain

## Overview

Entities are domain objects defined by their unique identity rather than their attributes. An entity persists over time, and its identity remains constant even as its attributes change. In the audiology domain, entities represent the core clinical and operational objects that have distinct lifecycles and must be individually tracked throughout the system.

## Entity Characteristics

Entities in the audiology domain share several characteristics. Each entity has a unique identifier that distinguishes it from all other entities of the same type. An entity's state changes over time as clinical activities occur. Two entities with identical attributes but different identities are considered distinct objects. Entity equality is determined by identity, not by attribute comparison.

## Hearing Assessment Context Entities

### AudiometricEvaluation

The AudiometricEvaluation entity represents a single diagnostic hearing evaluation session. Its identity is established when the evaluation begins and persists through the complete lifecycle of that clinical encounter. The entity accumulates threshold measurements, speech recognition results, and clinical impressions over the course of the session. Its state transitions from "in progress" through "completed" to "finalized" as the audiologist conducts and reviews tests.

### Patient (Assessment Projection)

The Patient entity in the Hearing Assessment Context represents the patient as a subject of diagnostic evaluation. This projection of the patient carries assessment-specific information: hearing history, previous audiograms, risk factors for hearing loss, and otologic medical history. The patient's identity is shared across contexts, but each context maintains its own projection with context-relevant attributes.

## Device Management Context Entities

### DeviceFitting

The DeviceFitting entity represents the process of configuring a hearing device for a specific patient. Its identity tracks the fitting through initial selection, programming, verification, and subsequent adjustments. A single patient may have multiple DeviceFitting entities representing different devices or different fitting sessions for the same device over time.

### Device

The Device entity represents a specific physical hearing device identified by its serial number. The entity tracks the device through its lifecycle: acquisition, initial fitting, adjustments, repairs, and eventual retirement. The Device entity maintains the current programming state and full maintenance history.

### Patient (Device Projection)

The Patient entity in the Device Management Context carries device-relevant information: current devices, fitting history, ear canal characteristics, manual dexterity considerations, and lifestyle factors that influence device selection.

## Rehabilitation Context Entities

### RehabilitationPlan

The RehabilitationPlan entity represents an ongoing program of aural rehabilitation for a specific patient. Its identity persists as the plan evolves: goals are added or modified, interventions are adjusted based on progress, and outcome measurements are recorded. A patient may have multiple rehabilitation plans over time, each with its own identity and lifecycle.

### InterventionActivity

The InterventionActivity entity represents a specific therapeutic activity within a rehabilitation plan. Each activity has its own identity, schedule, and progress tracking. Activities may be individual counseling sessions, group communication classes, or self-directed auditory training exercises.

## Vestibular Services Context Entities

### VestibularEvaluation

The VestibularEvaluation entity represents a vestibular assessment session. Its identity persists as test results are accumulated (VNG, caloric testing, positional testing) and as the clinical picture of the patient's balance function develops. The entity state transitions from initial assessment through testing to diagnostic conclusion.

### VestibularRehabilitationProgram

The VestibularRehabilitationProgram entity tracks a vestibular rehabilitation therapy program. Its identity connects exercise prescriptions, compliance monitoring, and functional improvement measurements across the rehabilitation timeline.

## Pediatric Audiology Context Entities

### PediatricPatient

The PediatricPatient entity extends the base patient concept with developmental context. This entity tracks the child through screening, diagnosis, intervention, and developmental monitoring. Its identity connects newborn screening results to early intervention activities to school-age services, providing a longitudinal record of the child's auditory development.

### ScreeningEvent

The ScreeningEvent entity represents a specific hearing screening occurrence for a child. Each screening has its own identity, allowing tracking of pass/refer results, follow-up compliance, and connections to subsequent diagnostic evaluations.

## Clinical Documentation Context Entities

### ClinicalReport

The ClinicalReport entity represents a formal clinical document. Its identity persists through drafting, review, finalization, and distribution. The report entity tracks its versioning, approval status, and distribution history.

### Referral

The Referral entity represents a request for services from one provider to another. Its identity tracks the referral through creation, sending, acknowledgment, fulfillment, and closure. Referral entities connect clinical activities across bounded contexts and external providers.

## Identity Management

Entity identity in the audiology domain must be carefully managed. Patient identity spans all bounded contexts and must be consistent, typically linked to a medical record number or other healthcare identifier. Device identity uses manufacturer serial numbers supplemented by internal identifiers. Clinical document identity must support versioning and audit trail requirements mandated by regulatory compliance.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley. Chapter 5 on entities.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley. Chapter 5 on entity design.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media. Chapter 7 on entity patterns.
