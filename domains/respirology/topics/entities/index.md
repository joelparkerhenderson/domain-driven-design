# Entities in the Respirology Domain

## Overview

An entity is a domain object defined by its unique identity rather than its attributes. Entities persist over time and may change their attributes while retaining the same identity. In the respirology domain, entities represent clinical concepts that have distinct identities and lifecycles: a patient remains the same patient even as their respiratory condition changes, and a care plan remains the same care plan even as its treatment recommendations are updated.

Entities are distinguished from value objects, which are defined solely by their attributes and have no independent identity. The choice between modeling a concept as an entity or a value object depends on whether identity matters in the domain.

## Entity Characteristics

### Identity

Every entity has a unique identifier that distinguishes it from all other entities of the same type. In the respirology domain, identifiers are typically system-generated (UUIDs or sequential IDs) rather than derived from clinical attributes, because clinical attributes may change or be corrected while the entity's identity must remain stable.

### Continuity

Entities have continuity over time. A Patient entity created at admission persists through assessment, diagnostics, treatment, and rehabilitation. The entity accumulates history and its current state reflects the culmination of all changes made to it.

### Mutability

Unlike value objects, entities are mutable. Their attributes change as clinical information is updated, treatments are administered, and conditions evolve. The Respiratory Assessment entity changes as the clinician records examination findings, adds symptom scores, and finalizes the assessment.

## Key Entities by Bounded Context

### Clinical Assessment Context

**Patient**: Represents the patient within the assessment context, carrying assessment-relevant attributes such as demographic information, respiratory history, smoking status, occupational exposures, and allergy information. Identity is maintained by PatientId.

**Assessment Encounter**: Represents a single clinical encounter during which a respiratory assessment is performed. It tracks the encounter date, clinician, location, and status (in-progress, completed, amended). Identity is maintained by EncounterId.

**Symptom History Entry**: Represents a dated record of reported symptoms, their onset, duration, severity, and associated triggers. Each entry has a unique HistoryEntryId and contributes to the longitudinal symptom record.

### Respiratory Diagnostics Context

**Test Execution Record**: Represents the execution of a specific diagnostic test, including the technician, equipment used, quality indicators, and raw measurements. Each execution has a unique ExecutionId, distinct from the order that requested it.

**Bronchoscopy Report**: Represents the findings from a bronchoscopy procedure, including airway visualization findings, biopsy locations, lavage results, and proceduralist notes. Identity is maintained by BronchoscopyReportId.

### Airway Management Context

**Intubation Record**: Represents a single intubation event, tracking the technique used, tube specifications, number of attempts, confirmation method, and any complications. Identity is maintained by IntubationId.

**Tracheostomy Record**: Represents a tracheostomy procedure and its ongoing management, including the procedure date, tube type and size, care protocols, and decannulation planning. Identity is maintained by TracheostomyId.

### Chronic Disease Management Context

**Action Plan**: Represents a patient's individualized self-management action plan, containing zone-based instructions (green/stable, yellow/worsening, red/emergency) with specific medication adjustments and criteria for seeking medical attention. Identity is maintained by ActionPlanId.

**Exacerbation Record**: Represents a single exacerbation episode, tracking onset date, severity, triggers, treatment provided, and resolution. Each exacerbation has a unique ExacerbationId and contributes to the exacerbation history that informs future risk assessment.

**Monitoring Protocol**: Represents the scheduled monitoring activities for a patient with a chronic respiratory condition, including test frequencies, review intervals, and escalation criteria. Identity is maintained by ProtocolId.

### Ventilatory Support Context

**Weaning Protocol**: Represents the structured plan for reducing ventilatory support, including readiness criteria, spontaneous breathing trial parameters, progression steps, and failure criteria. Identity is maintained by WeaningProtocolId.

**Home Ventilation Program**: Represents a patient's home ventilation arrangement, including device specifications, parameter settings, supply schedules, and follow-up plans. Identity is maintained by HomeProgramId.

### Pulmonary Rehabilitation Context

**Exercise Prescription**: Represents the individualized exercise program for a rehabilitation patient, including modalities (walking, cycling, upper-limb), intensity targets, duration, frequency, and progression rules. Identity is maintained by PrescriptionId.

**Education Module Assignment**: Represents the assignment of a specific education module to a patient, tracking completion status, comprehension assessment, and follow-up needs. Identity is maintained by AssignmentId.

**Discharge Plan**: Represents the plan for transitioning a patient from the rehabilitation program to independent self-management, including maintenance exercise recommendations, follow-up appointments, and community resource referrals. Identity is maintained by DischargePlanId.

## Entity Design Guidelines

- Entities should carry only the attributes necessary for their bounded context. A Patient entity in the Clinical Assessment Context does not need billing information.
- Entity identity should be immutable once assigned. Clinical identifiers such as medical record numbers may be stored as attributes but should not serve as the entity's primary identity.
- Entities should expose behavior through methods that enforce business rules, not through public setters that allow arbitrary modification.
- Entity state changes that are significant to other bounded contexts should result in domain event publication.

## References

- Evans, E. (2003). *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley. Chapter 5 on entities and identity.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley. Chapter 5 on entity design.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media. Chapter 9 on modeling business logic with entities.
