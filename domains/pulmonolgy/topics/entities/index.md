# Entities in the Pulmonolgy Domain

## Overview

Entities are domain objects defined by their unique identity rather than their attributes. An entity persists over time, and its identity remains constant even as its attributes change. In the pulmonolgy domain, entities represent clinical concepts that have distinct lifecycles, track state changes, and must be uniquely identifiable across the system.

The distinction between entities and value objects is critical in clinical domains. A patient is an entity because their identity persists regardless of changes in their clinical status, address, or treatment plan. In contrast, a spirometry measurement is a value object because it is defined entirely by its measured values and has no independent lifecycle.

## Entities by Bounded Context

### Pulmonary Assessment Context

**Patient**: The central entity across all contexts, though each context maintains its own projection of the patient. In the assessment context, Patient carries demographic information, referral source, presenting complaint history, and assessment encounter references. Identity is maintained through a unique patient identifier.

**Assessment Encounter**: Represents a specific clinical assessment visit. Tracks the encounter date, ordering physician, clinical indications, and assessment status (scheduled, in-progress, completed, cancelled). The encounter progresses through defined lifecycle states and accumulates findings over its duration.

**PFT Technician**: Represents the qualified technician conducting pulmonary function tests. Tracks certification status, competency validations, and test quality metrics. Identity persists across multiple testing sessions.

### Respiratory Diagnostics Context

**Diagnostic Procedure**: Represents a specific diagnostic intervention such as a bronchoscopy or thoracentesis. Tracks procedural status (ordered, scheduled, consented, in-progress, completed, resulted), performing physician, and associated complications. The procedure entity has a well-defined lifecycle from order through completion.

**Specimen**: Represents a tissue or fluid sample obtained during a diagnostic procedure. Tracks specimen type, collection method, handling chain, and pathology result linkage. Specimen identity is critical for maintaining chain-of-custody integrity.

**Proceduralist**: The physician performing diagnostic procedures. Tracks privileges, procedure volume, complication rates, and credentialing status. Identity persists across all procedures performed.

### Chronic Disease Management Context

**Chronic Disease Record**: Represents a patient's ongoing record for a specific chronic respiratory condition. Tracks disease type (COPD, asthma, ILD, pulmonary hypertension, cystic fibrosis), severity classification, diagnosis date, and disease progression history. A single patient may have multiple chronic disease records.

**Treatment Plan**: Represents the current active treatment strategy for a chronic condition. Tracks medication regimens, monitoring schedules, action plan thresholds, and plan revision history. The treatment plan entity transitions through states (draft, active, suspended, completed, superseded) as clinical needs evolve.

**Exacerbation**: Represents a discrete episode of acute symptom worsening. Tracks onset date, trigger identification, severity classification, treatment response, and resolution status. Each exacerbation is independently identifiable for longitudinal tracking and pattern analysis.

### Critical Care Context

**Ventilation Episode**: Represents a continuous period of mechanical ventilation for a patient. Tracks intubation date, ventilator type, mode progression, and extubation or tracheostomy outcome. The ventilation episode has a lifecycle from initiation through liberation or long-term ventilation transition.

**Weaning Trial**: Represents a specific attempt to reduce or discontinue ventilator support. Tracks trial type (spontaneous breathing trial, pressure support reduction), duration, physiological response, and outcome (success, failure, aborted). Multiple weaning trials may occur within a single ventilation episode.

**ICU Respiratory Therapist**: Represents the respiratory therapist managing ventilator care. Tracks shift assignments, ventilator assessments performed, and weaning trial supervision.

### Sleep Medicine Context

**Sleep Study Order**: Represents a request for polysomnography or home sleep testing. Tracks clinical indication, study type, priority, scheduling status, and insurance authorization. The order progresses through a defined lifecycle from request through completion.

**Therapy Prescription**: Represents the prescription of positive airway pressure therapy (CPAP or BiPAP). Tracks prescribed device type, pressure settings, mask type, and prescription validity period. The prescription entity evolves as settings are titrated and optimized.

**Sleep Patient**: The patient entity within the sleep medicine context, carrying sleep-specific attributes such as Epworth Sleepiness Scale history, sleep hygiene assessment results, and therapy compliance patterns.

### Pulmonary Rehabilitation Context

**Rehabilitation Enrollment**: Represents a patient's enrollment in a rehabilitation program. Tracks enrollment date, referring physician, program type, session attendance, and completion or withdrawal status.

**Exercise Session**: Represents a specific rehabilitation exercise session attended by a patient. Tracks session date, exercise modalities performed, physiological responses (heart rate, oxygen saturation, dyspnea scores), and session-specific modifications.

**Rehabilitation Therapist**: Represents the physiotherapist or respiratory therapist conducting rehabilitation sessions. Tracks specializations, patient assignments, and session delivery history.

## Entity Design Principles

Entities in the pulmonolgy domain follow these design principles:

- **Identity Stability**: Entity identifiers are assigned at creation and never change. A Diagnostic Procedure retains its identity from order through result, regardless of status changes.

- **State Transitions**: Entities with lifecycle states enforce valid transitions. A Ventilation Episode cannot move from "initiated" to "completed" without passing through intermediate states.

- **Behavioral Richness**: Entities encapsulate behavior, not just data. A Treatment Plan entity contains the logic for determining whether a medication change requires monitoring schedule adjustment.

- **Projection Across Contexts**: The same real-world concept (such as a patient) is represented by different entity projections in different contexts, each carrying only the attributes relevant to that context.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 5 on entities, identity, and lifecycle management.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 5 on entity implementation and identity strategies.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 4 on distinguishing entities from value objects in domain modeling.
