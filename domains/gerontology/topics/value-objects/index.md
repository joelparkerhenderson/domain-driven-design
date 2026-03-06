# Value Objects in the Gerontology Domain

## Overview

Value objects are immutable domain objects defined entirely by their attributes rather than by a unique identity. Two value objects with the same attributes are considered equal. In the gerontology domain, value objects represent clinical measurements, assessment scores, classifications, and other concepts where the value itself matters more than any notion of identity. Value objects are fundamental to modeling the many standardized clinical instruments and scoring systems used in geriatric care.

Evans (2003) recommends using value objects wherever possible because their immutability simplifies reasoning, eliminates side effects, and makes the domain model more expressive. In gerontology, many clinical concepts are naturally value objects: a blood pressure reading, a frailty score, a cognitive test result, or a medication dosage.

## Clinical Measurement Value Objects

### BloodPressureReading

Represents a single blood pressure measurement with systolic and diastolic values in mmHg. Immutable once recorded. Used in geriatric assessments to evaluate cardiovascular risk and orthostatic hypotension, which contributes to fall risk in older adults.

### GripStrengthMeasurement

Represents a hand grip strength test result in kilograms. Used in frailty screening as one of the Fried Frailty Phenotype criteria. The measurement includes the hand tested and the value, enabling comparison against age- and sex-adjusted norms.

### GaitSpeedMeasurement

Represents a timed walking test result, typically measured as meters per second over a defined distance. Gait speed is a key predictor of adverse outcomes in older adults and is used in frailty screening and functional assessment.

### BodyMassIndex

Represents the calculated BMI from height and weight measurements. Used in nutritional assessment to identify malnutrition or obesity risk in older adults, both of which contribute to adverse health outcomes.

## Assessment Score Value Objects

### FrailtyScore

Represents the result of a frailty assessment instrument. Contains the instrument name (e.g., Clinical Frailty Scale, Fried Phenotype), the numeric score, and the classification (robust, pre-frail, frail). The score is immutable once calculated and serves as input to care planning decisions.

### FallRiskScore

Represents the calculated fall risk from a standardized instrument such as the Morse Fall Scale or Tinetti Assessment Tool. Contains the instrument name, the numeric score, and the risk classification (low, moderate, high). Informs fall prevention interventions and home modification recommendations.

### CognitiveTestScore

Represents the result of a cognitive screening instrument. Contains the instrument name (MMSE, MoCA, Clock Drawing Test), the numeric score, the maximum possible score, and the interpretation (normal, mild impairment, moderate impairment, severe impairment). Different instruments produce different score ranges, so the value object encapsulates the instrument-specific interpretation logic.

### ADLScore

Represents the total score from an Activities of Daily Living assessment using the Katz Index. Contains scores for each of the six ADL domains (bathing, dressing, toileting, transferring, continence, feeding) and the total score. The total score ranges from 0 (fully dependent) to 6 (fully independent).

### IADLScore

Represents the total score from an Instrumental Activities of Daily Living assessment using the Lawton Scale. Contains scores for each IADL domain (telephone use, shopping, food preparation, housekeeping, laundry, transportation, medication management, finances) and the total score.

## Classification Value Objects

### DementiaStage

Represents the classified severity of dementia using a standardized staging system. Contains the staging instrument (Global Deterioration Scale, Clinical Dementia Rating), the stage number, and the stage description (mild, moderate, severe). Used to guide memory care planning and resource allocation.

### FrailtyClassification

Represents the categorical classification of frailty status. Contains the classification level (robust, pre-frail, frail, severely frail) and the instrument used to determine it. This classification drives care intensity decisions and prognostic discussions.

### MedicationAppropriatenessClassification

Represents the classification of a medication's appropriateness for an older adult based on the Beers criteria. Contains the Beers category (avoid, use with caution, avoid in certain conditions), the quality of evidence, and the strength of recommendation.

## Care Planning Value Objects

### CareGoalStatus

Represents the current status of a care goal at a point in time. Contains the status (active, achieved, modified, discontinued), the date of the status change, and the reason for any status change. Immutable once created; a new status object is created for each change.

### TreatmentPreference

Represents a patient's stated preference regarding a specific treatment decision. Contains the treatment type (CPR, mechanical ventilation, artificial nutrition, hospitalization), the preference (accept, decline, undecided), and the date the preference was expressed.

### MedicationDosage

Represents the dosage specification for a prescribed medication. Contains the dose amount, unit of measure, route of administration, and frequency. Immutability ensures that dosage changes are tracked as distinct events rather than in-place modifications.

## Address and Contact Value Objects

### CareSettingLocation

Represents a care delivery location. Contains the setting type (home, assisted living, skilled nursing facility, hospital, hospice), address, and contact information. Used to track where a patient receives care and to facilitate care transitions.

### CaregiverContact

Represents contact information for an informal caregiver. Contains the caregiver's name, relationship to the patient, phone number, email, and availability. Used in caregiver support planning and emergency contact protocols.

## Design Principles

Value objects in the gerontology domain follow several design principles. They are immutable: once a frailty score is recorded, it does not change. They implement equality based on attributes: two ADLScores with the same domain scores are equal. They are self-validating: a CognitiveTestScore with an MMSE value above 30 is rejected at creation. They encapsulate domain logic: a FrailtyScore knows its classification based on its numeric value and instrument.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media.
- Katz, S. et al. (1963). Studies of Illness in the Aged: The Index of ADL. JAMA.
