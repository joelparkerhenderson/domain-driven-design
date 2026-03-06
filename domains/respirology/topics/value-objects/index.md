# Value Objects in the Respirology Domain

## Overview

A value object is an immutable domain object defined entirely by its attributes, with no concept of identity. Two value objects with the same attributes are considered equal. Value objects are the preferred modeling tool for domain concepts that represent measurements, classifications, quantities, ranges, and descriptive attributes. In the respirology domain, value objects capture the rich clinical data that characterizes respiratory assessments, diagnostic results, and treatment parameters.

Value objects enforce constraints at the point of creation, ensuring that invalid states cannot exist. A SpO2 reading of 150% cannot be constructed because the value object's creation logic rejects values outside the physiologically valid range.

## Value Object Characteristics

### Immutability

Once created, a value object cannot be changed. If a different value is needed, a new value object is created. This immutability simplifies reasoning about domain logic because value objects are inherently thread-safe and can be freely shared between aggregates.

### Equality by Value

Two SpO2Reading value objects with the same percentage and measurement timestamp are equal regardless of where they are stored or how they were created. This contrasts with entities, where two Patient entities with identical attributes are still different if they have different identifiers.

### Self-Validation

Value objects validate their inputs at construction time. A FEV1Measurement value object rejects negative volumes. A GOLDStage value object accepts only values I through IV. This validation pushes error detection to the earliest possible moment.

## Key Value Objects by Bounded Context

### Clinical Assessment Context

**SymptomScore**: Encapsulates a score from a standardized respiratory symptom instrument (mMRC grade 0-4, CAT score 0-40, or Borg scale 0-10) along with the instrument type and assessment date. Validates that the score falls within the instrument's defined range.

**AuscultationFinding**: Represents a single lung sound finding including the location (e.g., right lower lobe), sound type (e.g., wheeze, crackle, rhonchi, diminished), and characteristics (e.g., inspiratory, expiratory, bilateral). Ensures that location and sound type are valid clinical terms.

**SmokingHistory**: Captures smoking status (current, former, never), pack-years, and cessation date if applicable. Pack-years are calculated from packs per day multiplied by years smoked.

**VitalSigns**: Encapsulates a set of respiratory-relevant vital signs including respiratory rate, heart rate, blood pressure, temperature, and SpO2. Validates physiological ranges.

### Respiratory Diagnostics Context

**SpirometryResult**: Encapsulates FEV1 (liters), FVC (liters), FEV1/FVC ratio, percent predicted values for FEV1 and FVC, and the interpretation (normal, obstructive, restrictive, mixed). Validates that measurements are non-negative and that the ratio is mathematically consistent.

**ArterialBloodGas**: Encapsulates pH, PaO2 (mmHg), PaCO2 (mmHg), HCO3 (mmol/L), and base excess. Includes interpretation of acid-base status (respiratory acidosis, metabolic alkalosis, etc.). Validates physiological ranges.

**SpO2Reading**: Represents a pulse oximetry measurement with the saturation percentage (0-100), measurement site (finger, earlobe), and whether the patient was on supplemental oxygen at the time.

**DLCOResult**: Encapsulates the diffusion capacity of the lung for carbon monoxide, including the measured value, percent predicted, and interpretation. Used in the evaluation of interstitial lung disease and other conditions affecting gas exchange.

### Airway Management Context

**BronchodilatorDose**: Represents a specific dose of bronchodilator medication including the drug name, classification (SABA, LABA, SAMA, LAMA), dose amount, dose unit, and delivery method (MDI, nebulizer, DPI). Validates that the dose is within standard ranges.

**TubeSpecification**: Describes an endotracheal or tracheostomy tube including type, size (internal diameter in mm), cuff type (cuffed, uncuffed), and material. Validates standard tube sizes.

**AirwayClearanceParameters**: Encapsulates the parameters for an airway clearance session including technique type (percussion, vibration, oscillatory device, cough augmentation), duration, frequency, and positioning.

### Chronic Disease Management Context

**GOLDClassification**: Encapsulates the GOLD stage (I-IV based on spirometry), symptom group (A-D based on symptoms and exacerbation risk), and the date of classification. Validates stage-group combinations.

**ActionPlanZone**: Represents a single zone (green, yellow, or red) within a COPD or asthma action plan, including the symptom criteria for entering the zone and the corresponding medication and behavioral instructions.

**ExacerbationSeverity**: Classifies an exacerbation as mild (managed with increased bronchodilators), moderate (requiring corticosteroids or antibiotics), or severe (requiring hospitalization or emergency department visit).

### Ventilatory Support Context

**VentilationMode**: Represents the ventilator mode (e.g., volume control, pressure control, pressure support, SIMV, BiPAP, CPAP) with its defining characteristics. Validates that the mode is a recognized clinical ventilation mode.

**VentilatorParameters**: Encapsulates a set of ventilator settings including tidal volume, respiratory rate, PEEP, FiO2, pressure support level, and I:E ratio. Validates parameter ranges and clinically unsafe combinations.

**AlarmThreshold**: Represents a single ventilator alarm threshold including the parameter being monitored, the threshold value, the alarm priority (high, medium, low), and the response protocol reference.

### Pulmonary Rehabilitation Context

**SixMinuteWalkResult**: Encapsulates the distance walked (meters), pre- and post-exercise SpO2 and heart rate, Borg dyspnea and fatigue scores, and the number of rest stops. Validates that distance is non-negative and vital signs are within recordable ranges.

**ExerciseIntensity**: Represents the prescribed intensity for an exercise modality, defined by target heart rate range, target Borg score range, or percentage of peak workload. Validates that targets are within safe ranges for the patient's assessed capacity.

**FunctionalCapacityLevel**: Classifies the patient's functional capacity based on 6MWT results and symptom burden, used to determine appropriate exercise prescription intensity.

## References

- Evans, E. (2003). *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley. Chapter 5 on value objects.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley. Chapter 6 on value object design.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media. Chapter 9 on value objects and domain primitives.
