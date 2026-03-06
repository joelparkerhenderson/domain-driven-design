# Value Objects - Otolaryngology Domain

## Overview

Value objects are immutable domain objects defined entirely by their attributes rather than by identity. Two value objects with the same attributes are considered equal and interchangeable. In the otolaryngology domain, value objects represent clinical measurements, scores, classifications, and other concepts where the data itself, not any persistent identity, determines meaning and equality.

## Value Object Characteristics in Otolaryngology

Clinical measurements and scores are natural value objects. An audiometric threshold of 40 dB HL at 1000 Hz in the right ear is fully described by those attributes. There is no meaningful "identity" beyond the measurement values themselves. Replacing one such measurement with another having identical values changes nothing in the domain model.

Value objects in the otolaryngology domain also provide type safety and validation. A "frequency" is not just a number; it is a value object that validates the value falls within the audiometric frequency range. An "AHI score" validates that it is non-negative and classifies severity according to established criteria.

## Key Value Objects by Bounded Context

### Clinical Assessment Context

**Anatomical Site**: Represents a location on the head and neck anatomy (e.g., right tympanic membrane, left nasal cavity, posterior pharyngeal wall). Attributes: region, laterality, specific structure.

**Endoscopic Finding Detail**: Describes a specific observation during endoscopy. Attributes: finding type (erythema, edema, mass, polyp), severity (mild, moderate, severe), extent description.

**Vital Signs**: A set of measurements taken at a clinical encounter. Attributes: blood pressure, heart rate, respiratory rate, temperature, oxygen saturation.

**ICD-10 Diagnosis Code**: An immutable reference to a diagnosis classification. Attributes: code, description, version. Ensures valid coding throughout the domain.

### Surgical Management Context

**CPT Procedure Code**: An immutable reference to a procedure classification. Attributes: code, description, relative value units, modifier applicability.

**Surgical Risk Score**: A composite assessment of surgical risk. Attributes: ASA classification, airway assessment score, bleeding risk category.

**Specimen Description**: Describes a tissue specimen submitted for pathology. Attributes: specimen type, anatomical source, laterality, size, fixation method.

**Anesthesia Type**: Classifies the anesthesia approach. Attributes: type (general, local, local with sedation, topical), intubation method if applicable.

### Voice Disorders Context

**Voice Handicap Index Score**: A patient-reported outcome score. Attributes: total score, functional subscale, physical subscale, emotional subscale, severity classification (mild, moderate, severe).

**Acoustic Measurement**: A set of objective voice measurements. Attributes: fundamental frequency (F0), jitter percentage, shimmer percentage, noise-to-harmonics ratio.

**GRBAS Scale Rating**: A perceptual voice quality rating. Attributes: grade, roughness, breathiness, asthenia, strain, each rated 0-3.

**Stroboscopic Finding**: Describes vocal fold vibratory characteristics. Attributes: mucosal wave (present, diminished, absent), glottic closure pattern, phase symmetry, amplitude.

### Sleep Disorders Context

**Apnea-Hypopnea Index**: The primary severity measure for obstructive sleep apnea. Attributes: total AHI, REM AHI, supine AHI, non-supine AHI, severity classification (normal <5, mild 5-15, moderate 15-30, severe >30).

**Epworth Sleepiness Scale Score**: A daytime sleepiness measure. Attributes: total score (0-24), severity classification (normal 0-10, mild 11-12, moderate 13-15, severe 16-24).

**Oxygen Desaturation Index**: Measures nocturnal oxygen instability. Attributes: ODI value, nadir SpO2, percentage time below 90% SpO2.

**CPAP Settings**: Prescribed pressure parameters. Attributes: mode (fixed, auto), minimum pressure, maximum pressure, pressure relief setting, ramp time.

### Allergy Management Context

**Skin Test Result**: The outcome of a single allergen skin test. Attributes: allergen name, allergen category, wheal diameter (mm), flare diameter (mm), interpretation (negative, positive 1+ to 4+).

**Serum IgE Level**: A laboratory measurement of allergen-specific IgE. Attributes: allergen, concentration (kU/L), class (0-6), interpretation.

**Immunotherapy Dose**: A single dose in an immunotherapy protocol. Attributes: allergen extract, concentration, volume, injection site, observed reaction.

**Rhinitis Severity Score**: A composite measure of rhinitis symptoms. Attributes: congestion score, rhinorrhea score, sneezing score, itching score, total score, classification.

### Hearing Services Context

**Audiometric Threshold**: A single hearing threshold measurement. Attributes: frequency (Hz), hearing level (dB HL), ear (left, right), conduction type (air, bone), masking (masked, unmasked).

**Speech Recognition Score**: Speech understanding performance. Attributes: word list used, presentation level (dB HL), score (percentage correct), ear tested.

**Tympanometric Result**: Middle ear function measurement. Attributes: peak compliance, peak pressure, ear canal volume, tympanogram type (A, As, Ad, B, C).

**Hearing Loss Classification**: Categorical description of hearing status. Attributes: type (conductive, sensorineural, mixed), degree (normal, mild, moderate, moderately severe, severe, profound), configuration (flat, sloping, rising, notched).

## Value Object Benefits

Value objects provide significant benefits in the otolaryngology domain:

- **Clinical precision**: A Hearing Loss Classification value object enforces valid combinations (e.g., preventing "conductive profound" which is clinically implausible).
- **Self-validation**: An Apnea-Hypopnea Index value object automatically classifies severity based on the numeric value.
- **Comparability**: Value objects enable meaningful comparison of clinical measurements over time without identity-based tracking.
- **Immutability**: Once recorded, a clinical measurement does not change. New measurements create new value objects.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 5 on value objects.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 6 on value object design.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 5 on value objects.
