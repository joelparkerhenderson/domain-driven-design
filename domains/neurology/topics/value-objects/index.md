# Value Objects - Neurology Domain

## Overview

Value objects are immutable domain objects defined entirely by their attributes rather than by a unique identity. Two value objects with the same attributes are considered equal. In the neurology domain, value objects capture the clinical measurements, scores, classifications, and descriptive data that characterize neurological findings without requiring individual tracking.

Value objects are fundamental to neurology modeling because much of clinical neurology involves recording precise measurements and classifications. A reflex grade of 2+ is a value object: it describes a finding, but there is no need to track one instance of "2+" as distinct from another instance of "2+" with the same attributes.

## Value Object Design Principles

### Immutability

Value objects cannot be modified after creation. If a clinical finding needs to change, a new value object replaces the existing one. This immutability ensures that historical clinical data remains intact and that value objects can be safely shared across aggregates.

### Equality by Attributes

Two value objects are equal if all their attributes are equal. A GCS score of 15 (E4, V5, M6) is equal to any other GCS score of 15 (E4, V5, M6), regardless of which patient or encounter produced it.

### Self-Validation

Value objects validate their own attributes upon creation. A Reflex Grade value object rejects a value of 5 because the valid range is 0 to 4+. A MoCA Score rejects a value of 35 because the maximum is 30. This embeds clinical business rules directly in the value object.

## Value Objects by Bounded Context

### Clinical Assessment Context

**GlasgowComaScale**
- Attributes: eyeOpening (1-4), verbalResponse (1-5), motorResponse (1-6), totalScore (3-15).
- Validation: totalScore must equal the sum of the three components.
- Clinical significance: quantifies consciousness level for acute neurological presentations.

**ReflexGrade**
- Attributes: grade (0, 1+, 2+, 3+, 4+), location (e.g., biceps, triceps, patellar, Achilles).
- Validation: grade must be within the standardized 0-4+ scale.
- Clinical significance: indicates upper or lower motor neuron pathology.

**CognitiveScore**
- Attributes: instrument (MMSE or MoCA), totalScore, domainSubscores, assessmentDate.
- Validation: total must fall within instrument range (MMSE: 0-30, MoCA: 0-30).
- Clinical significance: screens for cognitive impairment and tracks cognitive decline.

**DermatomalFinding**
- Attributes: dermatome (e.g., C5, T10, L4), modality (light touch, pinprick, vibration, proprioception), finding (normal, decreased, absent, increased).
- Validation: dermatome must be a valid spinal nerve root level.
- Clinical significance: localizes sensory deficits to specific spinal cord levels.

**MusclePowerGrade**
- Attributes: grade (0-5 on MRC scale), muscle, side (left, right).
- Validation: grade must be within 0-5 MRC scale.
- Clinical significance: quantifies motor weakness for neurological localization.

### Neuroimaging Context

**ImagingModality**
- Attributes: type (MRI, CT, PET, SPECT), subtype (e.g., MRI with contrast, CT angiography).
- Validation: type must be a recognized neuroimaging modality.
- Clinical significance: determines the imaging technique applied.

**ImagingFinding**
- Attributes: location (anatomical region), description, severity, laterality.
- Validation: location must be a valid neuroanatomical structure.
- Clinical significance: captures a specific abnormality identified on imaging.

**EEGPattern**
- Attributes: patternType (normal, focal slowing, generalized slowing, epileptiform discharge, seizure), location, frequency, morphology.
- Validation: patternType must be a recognized EEG classification.
- Clinical significance: characterizes the electrical activity pattern on EEG.

### Neurodegenerative Disease Context

**UPDRSScore**
- Attributes: partI (non-motor), partII (motor experiences of daily living), partIII (motor examination), partIV (motor complications), totalScore.
- Validation: each part score within defined ranges.
- Clinical significance: standardized measure of Parkinson's disease severity.

**BiomarkerMeasurement**
- Attributes: biomarkerType (amyloid-beta, tau, neurofilament light chain, alpha-synuclein), value, unit, referenceRange, collectionMethod.
- Validation: value must be numeric and positive.
- Clinical significance: objective measure of disease state or progression.

**DiseaseStage**
- Attributes: disease, stagingSystem, stage, criteria met.
- Validation: stage must be valid for the specified staging system.
- Clinical significance: classifies disease severity for treatment and prognosis.

### Epilepsy Management Context

**SeizureClassification**
- Attributes: onsetType (focal, generalized, unknown), awarenessLevel (aware, impaired awareness), motorFeatures (motor, non-motor), specific descriptors.
- Validation: must conform to the ILAE 2017 classification hierarchy.
- Clinical significance: determines treatment approach and prognosis.

**AEDDosage**
- Attributes: medication, dose, unit, frequency, route.
- Validation: dose must not exceed maximum recommended daily dose.
- Clinical significance: specifies the medication regimen for seizure control.

**SeizureFrequency**
- Attributes: count, period (daily, weekly, monthly), seizureType.
- Validation: count must be non-negative.
- Clinical significance: tracks treatment efficacy over time.

### Neuromuscular Disorders Context

**NerveConduction**
- Attributes: nerve, segment, latency, amplitude, conductionVelocity, fWaveLatency.
- Validation: values must be numeric and non-negative.
- Clinical significance: distinguishes axonal from demyelinating pathology.

**GeneticVariant**
- Attributes: gene, variant, zygosity, pathogenicityClassification (benign, likely benign, VUS, likely pathogenic, pathogenic).
- Validation: pathogenicity must follow ACMG classification.
- Clinical significance: determines genetic diagnosis and counseling.

### Neurorehabilitation Context

**FIMScore**
- Attributes: motorSubscore (13-91), cognitiveSubscore (5-35), totalScore (18-126), individualItemScores.
- Validation: total must equal sum of motor and cognitive subscores.
- Clinical significance: measures functional independence level.

**ModifiedRankinScore**
- Attributes: grade (0-6), description.
- Validation: grade must be integer 0-6.
- Clinical significance: quantifies post-stroke disability.

**NIHStrokeScaleScore**
- Attributes: totalScore (0-42), individualItemScores (15 items).
- Validation: total must equal sum of individual item scores.
- Clinical significance: quantifies acute ischemic stroke severity.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 5 on value objects.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 6 on value object design.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 4 on value objects.
- Medical Research Council (MRC). Muscle power grading scale.
- International League Against Epilepsy (ILAE). 2017 Seizure Classification.
