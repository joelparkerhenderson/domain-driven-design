# Neuroimaging Context - Neurology Domain

## Overview

The Neuroimaging Context is a core bounded context that manages the ordering, acquisition, interpretation, and reporting of neurological imaging and neurophysiological studies. This context serves as the diagnostic imaging hub for the neurology domain, integrating with DICOM standards for imaging data management and providing structured interpretation results to other bounded contexts.

Modern neurology depends heavily on neuroimaging for diagnosis, treatment planning, and disease monitoring. This context models the complete lifecycle of an imaging study from the initial clinical order through acquisition, interpretation, and final reporting.

## Scope and Responsibilities

### Structural Neuroimaging

**MRI Brain and Spine**
- Protocol management: standard brain MRI, epilepsy protocol, MS protocol, stroke protocol, brain with contrast, spine cervical/thoracic/lumbar.
- Sequence management: T1-weighted, T2-weighted, FLAIR, DWI/ADC, SWI, MRA, MRV, post-contrast sequences.
- Volumetric analysis: hippocampal volumetry for Alzheimer's evaluation, brain volume measurement for MS monitoring.
- Functional MRI (fMRI): pre-surgical motor and language mapping for epilepsy surgery candidates.

**CT Brain and Spine**
- Emergency imaging for acute stroke (CT head, CT angiography, CT perfusion).
- Hemorrhage detection and characterization.
- Fracture evaluation in trauma.
- CT myelography for spinal pathology when MRI is contraindicated.

**PET Imaging**
- Amyloid PET (florbetapir, florbetaben, flutemetamol) for Alzheimer's disease evaluation.
- Tau PET for neurodegenerative disease staging.
- FDG-PET for epilepsy surgical evaluation (interictal hypometabolism).
- DaT-SPECT for dopaminergic pathway assessment in parkinsonism.

### Neurophysiological Studies

**Electroencephalography (EEG)**
- Routine EEG: standard 20-minute recording with activation procedures.
- Ambulatory EEG: extended monitoring over 24-72 hours.
- Continuous EEG monitoring: ICU monitoring for seizures and encephalopathy.
- Video-EEG: epilepsy monitoring unit recordings for seizure characterization and surgical planning.
- EEG pattern classification: normal background, focal slowing, generalized slowing, epileptiform discharges (spikes, sharp waves), electrographic seizures, periodic patterns.

**Electromyography and Nerve Conduction Studies (EMG/NCS)**
- Motor nerve conduction studies: distal latency, amplitude, conduction velocity, F-wave latency.
- Sensory nerve conduction studies: amplitude, conduction velocity.
- Needle EMG: insertional activity, spontaneous activity (fibrillations, positive sharp waves, fasciculations), motor unit morphology, recruitment pattern.
- Repetitive nerve stimulation: decrement and increment testing for neuromuscular junction disorders.
- Single fiber EMG: jitter analysis for myasthenia gravis.

### Study Lifecycle Management

The imaging study lifecycle progresses through defined states:
1. **Ordered**: clinical indication documented, protocol selected, priority assigned.
2. **Scheduled**: appointment or slot allocated for acquisition.
3. **Acquired**: imaging data collected, quality assured, archived to PACS.
4. **Pending Interpretation**: study in the reading queue awaiting physician review.
5. **Interpreted**: preliminary findings documented by interpreting physician.
6. **Finalized**: report signed, results available for clinical consumption.
7. **Amended**: corrections or addenda applied to a finalized report.

### Reporting

Structured reporting captures:
- Clinical indication and relevant history.
- Technique and protocol used.
- Findings organized by anatomical region or system.
- Impression summarizing the key diagnostic conclusions.
- Recommendations for follow-up studies or clinical correlation.
- Critical findings requiring immediate communication.

## Aggregate Design

**ImagingStudy** is the primary aggregate root, enforcing the invariant that a study cannot be marked as interpreted without an associated structured report. The study contains StudyProtocol and AcquisitionParameters as value objects.

**NeurophysiologyStudy** is a separate aggregate root for EEG, EMG, and NCS studies, reflecting their distinct workflow and reporting requirements.

## DICOM Integration

This context natively supports DICOM standards:
- DICOM objects for image storage and retrieval.
- DICOM worklist for study scheduling and acquisition.
- DICOM structured reporting (SR) for machine-readable reports.
- Study Instance UID as a key identifier for cross-system reference.

## Domain Events

- ImagingStudyOrdered: initiates the imaging workflow.
- ImagingStudyAcquired: signals completion of data acquisition.
- ImagingStudyInterpreted: makes results available to consuming contexts.
- CriticalFindingIdentified: triggers urgent notification workflows.

## Integration Points

- Receives orders from Clinical Assessment Context (customer-supplier).
- Supplies results to Neurodegenerative Disease Context (conformist relationship).
- Supplies specialized imaging results to Epilepsy Management Context.
- Supplies electrodiagnostic results to Neuromuscular Disorders Context.
- Exposes DICOM services to external PACS systems (open host service).

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- DICOM Standards Committee. *DICOM Standard*. National Electrical Manufacturers Association (NEMA).
- Defined in ACR Appropriateness Criteria for Neurological Imaging.
