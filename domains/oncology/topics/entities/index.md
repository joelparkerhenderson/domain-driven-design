# Entities in Oncology

## Overview

Entities are domain objects defined by a unique identity that persists over time, even as their attributes change. In oncology, entities represent the clinical concepts that have a lifecycle and whose identity matters: a specific patient, a particular tumor, a given treatment course, or an individual chemotherapy cycle. The identity of an entity distinguishes it from all other objects of the same type, and tracking that identity through state changes is fundamental to accurate clinical record-keeping.

## Identity in Oncology Entities

Identity in oncology entities is clinically meaningful. A patient is identified by a medical record number. A cancer diagnosis is identified by a combination of patient identity and a diagnosis identifier assigned by the diagnosing institution. A chemotherapy order is identified by an order number that persists from the moment the order is created through administration, completion, and archival.

The choice of identity strategy matters because oncology entities participate in long-running clinical processes. A treatment course may span months or years. A survivorship care plan may be active for decades. The identity mechanism must support this longevity and must enable reliable cross-referencing between entities in different bounded contexts.

## Key Entities by Bounded Context

### Diagnosis and Staging Context

**Patient**: The central entity around which all oncology care revolves. In the diagnosis context, the patient entity carries demographic attributes, cancer risk factors, and screening history. The patient entity exists across all bounded contexts but may carry different attributes in each context's model.

**Tumor**: An entity representing a specific neoplasm, identified uniquely within the patient's medical record. A patient may have multiple tumors (synchronous or metachronous), and each tumor has its own staging, molecular profile, and treatment history. The tumor entity's identity is essential for tracking response to treatment and for cancer registry reporting.

**Biopsy Specimen**: An entity representing a tissue sample obtained for diagnostic evaluation. Each specimen has a unique accession number and progresses through states: collected, received, processed, examined, reported. The specimen identity links the physical tissue sample to its pathological findings.

### Treatment Planning Context

**Tumor Board Case**: An entity representing a patient case presented to the multidisciplinary tumor board for review. Each case presentation has a unique identifier, a presentation date, attending specialists, and a documented recommendation. The same patient may be presented multiple times as their clinical situation evolves.

**Clinical Trial Enrollment**: An entity tracking a patient's participation in a specific clinical trial. Each enrollment has a unique identifier, a protocol reference, a randomization assignment (if applicable), and tracks protocol compliance through the trial duration.

### Chemotherapy Management Context

**Chemotherapy Cycle**: An entity representing a single iteration of a chemotherapy regimen. Each cycle is uniquely identified within the treatment course and tracks the planned drugs and doses, actual administered doses, administration dates, and observed toxicities. The cycle entity progresses through states: planned, prepared, in-progress, completed, or cancelled.

**Infusion Session**: An entity representing a single visit during which chemotherapy drugs are administered. An infusion session has a unique identifier, a scheduled date and time, the drugs to be administered, and tracks the actual start time, end time, and any infusion reactions.

### Radiation Therapy Context

**Treatment Fraction**: An entity representing a single radiation treatment delivery session. Each fraction is uniquely identified within the treatment course and records the planned dose, delivered dose, imaging verification results, and any treatment interruptions. Fractions progress through states: scheduled, imaged, treated, verified.

**Simulation Session**: An entity representing the planning session where the patient is positioned and imaged to define treatment geometry. The simulation session captures the immobilization devices used, the imaging data acquired, and the reference marks placed on the patient.

### Surgical Oncology Context

**Operative Procedure**: An entity representing a surgical intervention, identified by a procedure identifier. It captures the planned procedure type, the actual procedure performed, intraoperative findings, estimated blood loss, and complications. The procedure entity links to specimen entities in the pathology system for margin assessment.

**Sentinel Node**: An entity representing an identified sentinel lymph node during surgery. Each sentinel node has a unique identifier, a location, and a pathological status (positive or negative for cancer cells).

### Survivorship Care Context

**Follow-Up Visit**: An entity representing a scheduled surveillance encounter after treatment completion. Each visit has a unique identifier, a scheduled date, the surveillance tests to be performed, and the clinical findings documented during the visit.

**Late Effect**: An entity tracking a long-term or delayed treatment side effect in a survivor. Each late effect has a unique identifier, the causative treatment, the date of onset, the current severity, and the management approach.

## Entity Lifecycle

Oncology entities have clinically meaningful lifecycles that the domain model must capture. A chemotherapy cycle progresses from planned to prepared to in-progress to completed, with possible transitions to held or cancelled at various points. A tumor entity may progress through states reflecting treatment response: newly diagnosed, responding to treatment, stable, progressing, or in remission.

State transitions in oncology entities often trigger domain events. When a chemotherapy cycle transitions to completed, a ChemotherapyCycleCompleted event is published. When a tumor is restaged, a TumorRestaged event notifies the treatment planning context. These state-driven events are the primary mechanism for cross-context communication.

## Entity Equality

Two entities are equal if and only if they have the same identity, regardless of their current attribute values. A chemotherapy cycle with order number CYC-2024-001 is the same entity whether its status is "planned" or "completed." This identity-based equality distinguishes entities from value objects, which are defined by their attributes.

## Persistence Considerations

Entities in oncology must support auditability. Clinical systems require the ability to reconstruct the state of an entity at any point in time. This is not an infrastructure concern that the domain model ignores; rather, it is a domain requirement that the entity model must support. Event sourcing is a natural fit for many oncology entities, where the sequence of clinical events that shaped the entity's current state is itself clinically meaningful.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 5: A Model Expressed in Software.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 5: Entities.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 8: Domain Model Patterns.
