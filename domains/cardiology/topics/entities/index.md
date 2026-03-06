# Entities in Cardiology

## Overview

Entities are domain objects defined by their unique identity rather than their attributes. In the cardiology domain, entities represent objects that persist over time, undergo changes in state, and must be tracked individually. A patient, a procedure, a cardiac device -- each has a distinct identity that remains constant even as its attributes evolve. Two patients with identical demographics are still distinct entities; two pacemakers with identical specifications but different serial numbers are still distinct entities.

## Entity Characteristics in Cardiology

Cardiology entities exhibit several important characteristics:

- **Identity continuity**: A patient's identity persists across encounters, procedures, and rehabilitation sessions. The domain model must track this patient across all bounded contexts.
- **State evolution**: Entities change over time. A heart failure patient's NYHA class may improve from III to II with GDMT optimization. The entity's identity remains constant while its state evolves.
- **Lifecycle management**: Cardiac entities have defined lifecycles. A procedure entity transitions from planned to in-progress to completed (or cancelled). A device entity transitions from implanted to active to explanted or decommissioned.
- **Clinical traceability**: Entity identity enables clinical audit trails, linking outcomes to specific encounters, procedures, and devices.

## Key Entities by Bounded Context

### Clinical Assessment Context

**Patient**: The central entity across all contexts. Identified by a medical record number (MRN). Attributes include demographics, cardiac history, comorbidities, and allergies. The patient entity is shared across contexts through identity references, not direct object sharing.

**ClinicalEncounter**: Represents a single clinical assessment event. Identified by an encounter ID. Attributes include encounter date, provider, encounter type (initial, follow-up, urgent), and status. Contains references to findings, vital signs, and orders generated during the encounter.

**ReferringProvider**: The clinician who orders a cardiac evaluation. Identified by a provider ID (NPI). Attributes include specialty, affiliation, and contact information.

### Diagnostic Imaging Context

**ImagingStudy**: A complete cardiac imaging examination. Identified by an accession number. Attributes include modality (echo, CT, MRI, nuclear), acquisition date, performing technologist, interpreting physician, and study status.

**ImagingReport**: The formal interpretation document for an imaging study. Identified by a report ID. Attributes include reporting physician, report status (preliminary, final, amended), findings narrative, and structured measurements.

### Interventional Procedures Context

**Procedure**: A cardiac catheterization or intervention event. Identified by a procedure ID. Attributes include procedure type, date, operators, access site, fluoroscopy time, contrast volume, and procedure status.

**CoronaryLesion**: A specific stenosis in a coronary artery segment. Identified by a lesion ID within a procedure. Attributes include vessel name, segment location, percent stenosis, lesion length, and calcification severity. Multiple lesions may be documented per procedure.

**ImplantedDevice**: A device placed during an interventional procedure (stent, valve prosthesis, closure device). Identified by manufacturer serial number. Attributes include device type, model, dimensions, implantation date, and anatomic location.

### Electrophysiology Context

**ArrhythmiaEpisode**: A documented occurrence of an abnormal heart rhythm. Identified by an episode ID. Attributes include arrhythmia type, onset time, duration, symptoms, hemodynamic impact, and termination method.

**CardiacImplantableDevice**: A pacemaker, ICD, or CRT device. Identified by device serial number. Attributes include manufacturer, model, implant date, battery status, programmed parameters, and lead configuration. This entity has a long lifecycle spanning years with regular interrogation updates.

**DeviceLead**: An electrode lead connected to a cardiac implantable device. Identified by lead serial number. Attributes include lead type (atrial, ventricular, coronary sinus), implant position, impedance, sensing threshold, and pacing threshold.

### Heart Failure Management Context

**HeartFailurePatient**: A patient entity enriched with heart failure-specific attributes. Identified by MRN. Attributes include heart failure phenotype (HFrEF, HFmrEF, HFpEF), ACC/AHA stage, NYHA functional class, and date of initial heart failure diagnosis.

**MedicationPrescription**: A prescribed GDMT medication. Identified by prescription ID. Attributes include drug class, specific agent, current dose, target dose, start date, titration history, and reason for any dose limitations.

### Cardiac Rehabilitation Context

**RehabilitationEnrollment**: A patient's enrollment in a cardiac rehabilitation program. Identified by enrollment ID. Attributes include qualifying diagnosis or procedure, enrollment date, insurance authorization, assigned phase, and program status.

**ExerciseSession**: A single supervised exercise session. Identified by session ID. Attributes include session date, exercise modality, duration, peak heart rate, peak blood pressure, symptoms, and session completion status.

## Entity Identity Strategies

Cardiology entities use various identity strategies:

- **Natural identifiers**: MRN for patients, NPI for providers, serial numbers for devices.
- **System-generated identifiers**: UUIDs or sequential IDs for encounters, procedures, and sessions.
- **Composite identifiers**: Lesion identification may combine procedure ID and vessel segment code.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 5 on entities.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 5 on entity modeling.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 5 on domain model building blocks.
- ACC/AHA Clinical Data Standards for Electrocardiography and Ambulatory Electrocardiography. *JACC*, 2004.
