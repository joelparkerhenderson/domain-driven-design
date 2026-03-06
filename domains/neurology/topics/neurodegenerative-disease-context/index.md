# Neurodegenerative Disease Context - Neurology Domain

## Overview

The Neurodegenerative Disease Context is a core bounded context that models the longitudinal management of progressive neurological conditions. Unlike the Clinical Assessment Context, which captures point-in-time evaluations, this context tracks disease trajectories over months and years, managing treatment protocols, monitoring biomarkers, and evaluating clinical trial eligibility.

Neurodegenerative diseases are defined by the progressive loss of neuronal structure or function. The domain model must capture the slow, relentless progression of these conditions, the therapeutic interventions that aim to slow that progression, and the clinical milestones that mark significant changes in a patient's disease course.

## Scope and Responsibilities

### Alzheimer's Disease Management

- Cognitive decline tracking using serial MMSE, MoCA, and CDR (Clinical Dementia Rating) scores.
- Biomarker monitoring: amyloid PET status, CSF amyloid-beta 42/40 ratio, CSF phosphorylated tau, plasma neurofilament light chain.
- Disease staging: preclinical (biomarker-positive, cognitively normal), mild cognitive impairment (MCI), mild dementia, moderate dementia, severe dementia.
- Treatment management: cholinesterase inhibitors (donepezil, rivastigmine, galantamine), NMDA receptor antagonist (memantine), anti-amyloid therapies (aducanumab, lecanemab, donanemab).
- ARIA (Amyloid-Related Imaging Abnormalities) monitoring for anti-amyloid therapies.
- Caregiver support coordination and advance care planning.

### Parkinson's Disease Management

- Motor symptom tracking using UPDRS (Unified Parkinson's Disease Rating Scale) Parts I-IV.
- Hoehn and Yahr staging for disease severity (Stage 1-5).
- Medication management: levodopa/carbidopa, dopamine agonists (pramipexole, ropinirole, rotigotine), MAO-B inhibitors (rasagiline, selegiline, safinamide), COMT inhibitors (entacapone, opicapone).
- Motor fluctuation tracking: wearing-off, on-off phenomenon, dyskinesias.
- Non-motor symptom management: REM sleep behavior disorder, autonomic dysfunction, cognitive impairment, depression.
- Deep brain stimulation (DBS) candidacy evaluation and programming.
- DaT-SPECT imaging for dopaminergic pathway assessment.

### Amyotrophic Lateral Sclerosis (ALS) Management

- Functional assessment using ALSFRS-R (ALS Functional Rating Scale - Revised).
- Respiratory monitoring: forced vital capacity (FVC), maximal inspiratory pressure (MIP).
- Nutritional status tracking: weight, swallowing function, PEG tube management.
- Treatment management: riluzole, edaravone, AMX0035 (sodium phenylbutyrate/taurursodiol).
- Assistive device coordination: communication devices, wheelchair, ventilator.
- Multidisciplinary ALS clinic coordination.
- Genetic testing for familial ALS (SOD1, C9orf72, FUS, TARDBP).

### Multiple Sclerosis (MS) Management

- Relapse tracking: date, symptoms, severity, recovery, treatment (corticosteroids, PLEX).
- Disability tracking using EDSS (Expanded Disability Status Scale).
- MRI monitoring: new T2 lesions, enhancing lesions, brain atrophy, spinal cord lesions.
- Disease-modifying therapy management: injectable (interferons, glatiramer), oral (dimethyl fumarate, fingolimod, teriflunomide, siponimod, cladribine), infusion (natalizumab, ocrelizumab, ofatumumab, alemtuzumab).
- JCV antibody monitoring for progressive multifocal leukoencephalopathy (PML) risk stratification.
- MS subtype classification: relapsing-remitting (RRMS), secondary progressive (SPMS), primary progressive (PPMS).
- Pregnancy planning and DMT management considerations.

## Aggregate Design

**DiseaseProgression** is the primary aggregate root, tracking the longitudinal disease trajectory for a specific condition and patient. It enforces the invariant that progression milestones must be chronologically ordered and that each milestone references a validated assessment.

**TreatmentProtocol** is a separate aggregate root managing disease-modifying therapy protocols. It enforces the invariant that monitoring schedules comply with medication-specific safety protocols (e.g., ARIA monitoring for anti-amyloid therapies, JCV testing for natalizumab).

## Domain Events

- DiseaseProgressionRecorded: signals measurable change in disease status.
- TreatmentProtocolModified: notifies pharmacy and monitoring systems of treatment changes.
- ClinicalMilestoneReached: triggers care plan adjustments and potentially rehabilitation referrals.
- BiomarkerResultReceived: updates disease staging and treatment response evaluation.

## Integration Points

- Shares cognitive assessment data with Clinical Assessment Context via shared kernel.
- Conforms to Neuroimaging Context's model for imaging biomarkers.
- Publishes progression events to Neurorehabilitation Context for rehabilitation planning.
- Receives laboratory biomarker results through anti-corruption layer.
- Partners with Neurorehabilitation Context for ongoing functional management.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- Jack, Clifford R., et al. "NIA-AA Research Framework for Alzheimer's Disease." *Alzheimer's & Dementia*, 2018.
- Postuma, Ronald B., et al. "MDS Clinical Diagnostic Criteria for Parkinson's Disease." *Movement Disorders*, 2015.
- Thompson, Alan J., et al. "Diagnosis of Multiple Sclerosis: 2017 Revisions of the McDonald Criteria." *Lancet Neurology*, 2018.
