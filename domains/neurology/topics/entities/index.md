# Entities - Neurology Domain

## Overview

Entities are domain objects defined by their unique identity rather than their attributes. An entity persists over time, and its attributes may change while its identity remains constant. In the neurology domain, entities represent clinical concepts that have a distinct lifecycle and must be tracked individually.

Entities are distinguished from value objects by the question: "Does this object need to be uniquely identified and tracked over time?" If a neurological finding must be individually referenced, updated, or historically tracked, it is an entity. If it is merely a descriptive measurement with no need for individual identity, it is a value object.

## Entity Design Principles

### Identity

Every entity has a unique identity that distinguishes it from all other entities of the same type. In the neurology domain, identities are typically system-generated identifiers, though some entities have natural identifiers (e.g., accession numbers for imaging studies, encounter numbers for examinations).

### Mutability

Unlike value objects, entities may change over time. A patient's neurological status evolves, a seizure episode may be reclassified, and a rehabilitation goal may be updated. The entity's identity remains stable through these changes.

### Lifecycle

Entities have meaningful lifecycles. A neurological examination progresses from initiated to in-progress to completed to signed. An imaging study moves from ordered to acquired to interpreted to finalized. These lifecycle transitions are significant domain events.

## Entities by Bounded Context

### Clinical Assessment Context

**Patient**
- Identity: PatientId (system-assigned medical record number).
- Attributes: demographics, neurological history, current medications, allergies.
- Lifecycle: created at first encounter, persists indefinitely, attributes updated over time.
- Role: the central entity referenced by all bounded contexts.

**Examiner**
- Identity: ExaminerId (provider credential number).
- Attributes: name, credentials, specialty, subspecialty.
- Lifecycle: created at onboarding, persists through career.
- Role: the clinician who performs and is accountable for examination findings.

**NeurologicalEncounter**
- Identity: EncounterId (system-generated).
- Attributes: encounter date, location, type (initial, follow-up, emergency), chief complaint.
- Lifecycle: opened at patient arrival, closed at encounter completion.
- Role: the container for all clinical activities during a single patient visit.

### Neuroimaging Context

**ImagingOrder**
- Identity: OrderId (system-generated) and AccessionNumber (DICOM accession).
- Attributes: ordering provider, clinical indication, requested modality, priority, protocol.
- Lifecycle: created when ordered, transitions through scheduled, acquired, interpreted, finalized.
- Role: initiates and tracks the imaging workflow.

**InterpretingPhysician**
- Identity: PhysicianId (provider number).
- Attributes: name, credentials, board certifications, subspecialty (neuroradiology, neurophysiology).
- Lifecycle: persists through career.
- Role: the physician responsible for interpreting imaging and neurophysiology studies.

### Neurodegenerative Disease Context

**DiseaseCaseRecord**
- Identity: CaseRecordId (system-generated).
- Attributes: patient reference, diagnosis, date of onset, current disease stage, treatment history.
- Lifecycle: created at diagnosis, updated at each visit, persists for duration of disease.
- Role: the longitudinal record of a patient's neurodegenerative disease journey.

**ClinicalTrialEnrollment**
- Identity: EnrollmentId (system-generated).
- Attributes: trial identifier, patient reference, enrollment date, randomization arm, status.
- Lifecycle: created at enrollment, updated at each study visit, closed at trial completion or withdrawal.
- Role: tracks a patient's participation in a clinical research trial.

### Epilepsy Management Context

**EpilepsyCase**
- Identity: CaseId (system-generated).
- Attributes: patient reference, epilepsy syndrome, seizure type classification, age at onset, current AED regimen.
- Lifecycle: created at epilepsy diagnosis, updated at each clinical encounter.
- Role: the central entity for a patient's epilepsy management.

**EMUAdmission**
- Identity: AdmissionId (system-generated).
- Attributes: patient reference, admission date, indication, monitoring duration, outcome.
- Lifecycle: created at admission to epilepsy monitoring unit, closed at discharge.
- Role: tracks the inpatient video-EEG monitoring episode.

### Neuromuscular Disorders Context

**NeuromuscularCase**
- Identity: CaseId (system-generated).
- Attributes: patient reference, suspected diagnosis, diagnostic status, confirmed diagnosis.
- Lifecycle: created at initial evaluation, updated through diagnostic workup, persists through treatment.
- Role: the longitudinal record of a patient's neuromuscular disorder evaluation and management.

**GeneticTestRequest**
- Identity: RequestId (system-generated).
- Attributes: patient reference, test panel, laboratory, specimen type, clinical indication.
- Lifecycle: created when ordered, updated with results, closed when counseling is completed.
- Role: tracks the genetic testing workflow from order to result interpretation.

### Neurorehabilitation Context

**RehabilitationPatient**
- Identity: RehabPatientId (system-generated).
- Attributes: patient reference, primary neurological diagnosis, date of injury/onset, rehabilitation phase.
- Lifecycle: created at rehabilitation admission, updated through rehabilitation, persists for follow-up.
- Role: the patient entity within the rehabilitation context, distinct from the clinical assessment patient.

**TherapyGoal**
- Identity: GoalId (system-generated).
- Attributes: description, target functional level, target date, status, current progress.
- Lifecycle: created during goal-setting, updated at each therapy session, closed when achieved or revised.
- Role: represents a specific, measurable rehabilitation objective.

## Entity vs. Aggregate Root

Not all entities are aggregate roots. An entity may exist within an aggregate, subordinate to the aggregate root. For example, a TherapyGoal entity exists within a RehabilitationEpisode aggregate. The TherapyGoal cannot be accessed directly from outside the aggregate; it must be accessed through the RehabilitationEpisode aggregate root.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 5 on entities.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 5 on entity design.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 4 on entities and identity.
