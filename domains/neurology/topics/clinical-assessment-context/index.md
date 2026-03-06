# Clinical Assessment Context - Neurology Domain

## Overview

The Clinical Assessment Context is a core bounded context in the neurology domain that manages the structured evaluation of patients presenting with neurological symptoms. This context encompasses the entire neurological examination workflow, from history taking through physical examination to clinical impression and disposition.

The neurological examination is the foundation of neurological practice. Before any imaging is ordered or treatment is initiated, the clinical assessment localizes the problem within the nervous system, generates a differential diagnosis, and determines the urgency of further evaluation. This context captures that clinical reasoning process.

## Scope and Responsibilities

### History Taking

The clinical assessment begins with a detailed neurological history. The context manages structured data collection for:
- Chief complaint and history of present illness with temporal profile (acute, subacute, chronic, progressive, relapsing-remitting).
- Neurological review of systems covering headache, seizures, weakness, numbness, visual changes, speech difficulties, gait disturbance, and cognitive concerns.
- Past neurological history including prior diagnoses, imaging, and treatments.
- Family history of neurological disease (hereditary neuropathies, epilepsy, neurodegenerative disease).
- Social history relevant to neurological risk (alcohol use, occupational exposures, travel history).

### Mental Status Examination

The context manages formal cognitive screening using validated instruments:
- Mini-Mental State Examination (MMSE): 30-point screening covering orientation, registration, attention, recall, and language.
- Montreal Cognitive Assessment (MoCA): 30-point screening with greater sensitivity for mild cognitive impairment, covering visuospatial/executive, naming, memory, attention, language, abstraction, and orientation.
- Glasgow Coma Scale (GCS): 3-15 scale for acute consciousness assessment.

### Cranial Nerve Examination

Systematic evaluation of all twelve cranial nerves:
- CN I (Olfactory): smell identification testing.
- CN II (Optic): visual acuity, visual fields, fundoscopy.
- CN III, IV, VI (Oculomotor, Trochlear, Abducens): extraocular movements, pupillary responses.
- CN V (Trigeminal): facial sensation, jaw strength, corneal reflex.
- CN VII (Facial): facial symmetry, strength of facial muscles.
- CN VIII (Vestibulocochlear): hearing, vestibular function.
- CN IX, X (Glossopharyngeal, Vagus): palate elevation, gag reflex, voice quality.
- CN XI (Accessory): sternocleidomastoid and trapezius strength.
- CN XII (Hypoglossal): tongue protrusion and movement.

### Motor Examination

Assessment of the motor system including:
- Muscle bulk and tone (normal, increased, decreased, spasticity, rigidity).
- Muscle power grading on the MRC 0-5 scale for major muscle groups.
- Pronator drift and upper motor neuron signs.
- Involuntary movements (tremor, chorea, dystonia, myoclonus, fasciculations).

### Sensory Examination

Evaluation of sensory modalities mapped to dermatomal distributions:
- Light touch and pinprick (spinothalamic tract).
- Vibration and proprioception (dorsal column).
- Temperature sensation.
- Cortical sensory functions (stereognosis, graphesthesia, two-point discrimination).

### Reflex Testing

Deep tendon reflex grading across standard sites:
- Biceps (C5-C6), Triceps (C7-C8), Brachioradialis (C5-C6).
- Patellar (L3-L4), Achilles (S1-S2).
- Graded on the 0-4+ scale: 0 (absent), 1+ (diminished), 2+ (normal), 3+ (brisk), 4+ (clonus).
- Pathological reflexes: Babinski sign, Hoffmann sign.

### Coordination and Gait

Assessment of cerebellar function and ambulatory status:
- Finger-to-nose and heel-to-shin testing.
- Rapid alternating movements (dysdiadochokinesia).
- Romberg test.
- Gait observation: normal, spastic, ataxic, steppage, waddling, antalgic.
- Tandem gait testing.

## Aggregate Design

The primary aggregate root is the **NeurologicalExamination**, which enforces the invariant that an examination must be associated with a valid patient, examiner, and encounter before it can be persisted. The examination contains component value objects for each examination section.

A separate **CognitiveScreening** aggregate manages formal cognitive assessments, allowing cognitive scores to be tracked independently from the full neurological examination.

## Domain Events

Key domain events published by this context:
- ExaminationCompleted: triggers downstream specialist evaluations and imaging orders.
- AbnormalFindingDetected: alerts specialist contexts to clinically significant abnormalities.
- CognitiveScreeningScored: notifies the Neurodegenerative Disease Context of cognitive results.

## Integration Points

- Publishes examination results to Neuroimaging Context for imaging orders.
- Shares cognitive screening scores with Neurodegenerative Disease Context via shared kernel.
- Provides baseline assessment data to Neurorehabilitation Context.
- Refers seizure presentations to Epilepsy Management Context.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- Campbell, William W. *DeJong's The Neurologic Examination*. 8th Edition. Lippincott Williams & Wilkins, 2019.
- American Academy of Neurology. Clinical Practice Guidelines for Neurological Evaluation.
