# Bounded Contexts - Neurology Domain

## Overview

Bounded contexts define logical boundaries within which a specific domain model and its terminology remain consistent. In the neurology domain, six bounded contexts have been identified, each representing a distinct area of neurological practice with its own internal model, invariants, and ubiquitous language.

Each bounded context encapsulates its own aggregates, entities, value objects, domain events, repositories, and domain services. Communication between bounded contexts occurs through well-defined integration points using domain events and published languages.

## The Six Bounded Contexts

### 1. Clinical Assessment Context

The Clinical Assessment Context encompasses the initial and ongoing evaluation of patients with neurological complaints. This context manages the structured neurological examination, which systematically evaluates mental status, cranial nerves, motor function, sensory function, reflexes, coordination, and gait.

Key capabilities include:
- History taking with structured neurological review of systems
- Cognitive screening using validated instruments (MMSE, MoCA)
- Cranial nerve examination documentation
- Motor and sensory examination with dermatomal and myotomal mapping
- Deep tendon reflex grading (0 to 4+ scale)
- Coordination and gait assessment
- Glasgow Coma Scale scoring for acute presentations

### 2. Neuroimaging Context

The Neuroimaging Context manages the ordering, acquisition, interpretation, and reporting of neurological imaging and neurophysiological studies. This context operates at the intersection of clinical neurology and neuroradiology/neurophysiology.

Key capabilities include:
- MRI brain and spine study management (structural and functional sequences)
- CT imaging for acute neurological emergencies
- PET imaging for amyloid, tau, and dopaminergic studies
- EEG recording, interpretation, and reporting
- EMG and nerve conduction study management
- DICOM-compliant image storage and retrieval
- Structured radiology and neurophysiology reporting

### 3. Neurodegenerative Disease Context

The Neurodegenerative Disease Context models the longitudinal management of progressive neurological conditions. Unlike acute clinical assessment, this context tracks disease trajectories over months and years, managing treatment protocols and monitoring for disease milestones.

Key capabilities include:
- Alzheimer's disease staging and cognitive decline tracking
- Parkinson's disease motor and non-motor symptom management (UPDRS)
- ALS functional assessment and respiratory monitoring
- Multiple sclerosis relapse tracking and DMT management
- Biomarker monitoring (amyloid PET, CSF analysis, neurofilament levels)
- Clinical trial eligibility screening and enrollment tracking

### 4. Epilepsy Management Context

The Epilepsy Management Context covers the specialized management of epilepsy from initial seizure presentation through long-term treatment optimization. This context uses the ILAE classification system as its foundational framework.

Key capabilities include:
- Seizure classification by onset type (focal, generalized, unknown)
- Seizure diary management and frequency tracking
- AED selection, titration, and therapeutic drug monitoring
- Epilepsy monitoring unit (EMU) admission and video-EEG management
- Surgical candidacy evaluation (Phase I and Phase II)
- Vagus nerve stimulator (VNS) programming and outcome tracking
- Ketogenic diet therapy management

### 5. Neuromuscular Disorders Context

The Neuromuscular Disorders Context addresses the diagnostic workup and management of disorders affecting peripheral nerves, neuromuscular junctions, and skeletal muscles. This context coordinates complex diagnostic pathways including electrodiagnostic testing and genetic analysis.

Key capabilities include:
- Myopathy evaluation (inflammatory, metabolic, genetic)
- Peripheral neuropathy classification and workup
- Neuromuscular junction disorder management (myasthenia gravis, Lambert-Eaton)
- Electrodiagnostic study coordination (EMG/NCS)
- Genetic testing ordering, interpretation, and counseling
- Immunotherapy and biologic treatment management
- Pulmonary function monitoring for neuromuscular respiratory compromise

### 6. Neurorehabilitation Context

The Neurorehabilitation Context manages recovery and rehabilitation programs following neurological injury or disease. This context tracks functional outcomes over rehabilitation episodes using standardized assessment tools.

Key capabilities include:
- Stroke recovery trajectory modeling (acute, subacute, chronic phases)
- Traumatic brain injury (TBI) rehabilitation planning
- Functional outcome measurement (FIM, mRS, Barthel Index)
- Cognitive rehabilitation protocol management
- Speech and language therapy coordination
- Physical and occupational therapy goal tracking
- Adaptive technology assessment and prescription
- Community reintegration planning

## Context Boundaries and Language

Each bounded context maintains its own interpretation of shared terms. For example:
- "Study" in Neuroimaging refers to an imaging examination; in Neurodegenerative Disease it may refer to a clinical trial.
- "Episode" in Epilepsy Management refers to a seizure event; in Neurorehabilitation it refers to a rehabilitation episode of care.
- "Score" in Clinical Assessment refers to an examination finding (GCS, reflex grade); in Neurorehabilitation it refers to a functional outcome measure (FIM score).

These linguistic boundaries reinforce the need for explicit translation when concepts cross context boundaries.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 14 on bounded contexts.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 2 on domains and bounded contexts.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 7 on modeling boundaries.
