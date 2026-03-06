# Value Objects for Body Health Metrics

## Overview

Value objects in the body health metrics domain are immutable objects defined entirely by their attributes rather than by identity. Two value objects with the same attributes are interchangeable. Value objects are the natural modeling choice for measurements, units, ranges, scores, and other concepts where what something is matters more than which specific instance it is. In body health measurement, value objects carry the precision, units, and semantic richness that raw primitive types cannot express.

## Measurement Value Objects

### Weight

Represents a body weight measurement with value, unit, and precision.

- **Attributes**: numeric value, unit of measure (kg or lb), measurement precision (number of decimal places)
- **Validation**: Must be positive; must fall within physiological plausibility range (0.5 kg to 635 kg)
- **Behavior**: Supports unit conversion between kilograms and pounds; supports comparison and arithmetic operations that preserve unit consistency

### Height

Represents a body height measurement with value and unit.

- **Attributes**: numeric value, unit of measure (cm, m, or ft/in), measurement precision
- **Validation**: Must be positive; must fall within physiological plausibility range (50 cm to 280 cm)
- **Behavior**: Supports conversion between metric and imperial representations

### BodyMassIndex

Represents the calculated BMI value derived from weight and height.

- **Attributes**: numeric value (kg/m^2), classification category (underweight, normal, overweight, obese)
- **Validation**: Must be positive; typically between 10 and 80
- **Behavior**: Factory method calculates BMI from Weight and Height value objects; classification is derived automatically from the numeric value using WHO category thresholds

### WaistCircumference

Represents the measurement of waist circumference.

- **Attributes**: numeric value, unit of measure (cm or in), measurement site (narrowest point, umbilical level, iliac crest)
- **Validation**: Must be positive; typically between 40 cm and 200 cm
- **Behavior**: Supports waist-to-hip ratio calculation when paired with hip circumference

### BodyFatPercentage

Represents the proportion of body mass that is adipose tissue.

- **Attributes**: numeric value (percentage), measurement method (BIA, calipers, DEXA, hydrostatic)
- **Validation**: Must be between 2% and 60%
- **Behavior**: Classification into essential fat, athletic, fitness, acceptable, and obese categories based on age and sex

## Vital Sign Value Objects

### BloodPressure

Represents a blood pressure reading with systolic and diastolic components.

- **Attributes**: systolic value (mmHg), diastolic value (mmHg)
- **Validation**: Systolic must exceed diastolic; systolic between 60 and 300; diastolic between 30 and 200
- **Behavior**: Classification into normal, elevated, stage 1 hypertension, stage 2 hypertension, and hypertensive crisis categories

### HeartRate

Represents a heart rate measurement.

- **Attributes**: beats per minute, measurement context (resting, active, recovery)
- **Validation**: Must be between 20 and 220 bpm
- **Behavior**: Resting heart rate classification by fitness level; target heart rate zone calculation based on age

## Composition Value Objects

### FatMass

Represents the absolute weight of adipose tissue.

- **Attributes**: numeric value (kg), body region (total, trunk, appendicular, visceral)
- **Validation**: Must be non-negative; cannot exceed total body weight

### LeanMass

Represents the absolute weight of non-fat body tissue.

- **Attributes**: numeric value (kg), body region (total, appendicular, trunk)
- **Validation**: Must be positive; lean mass plus fat mass must equal total weight within tolerance

### BoneMineralDensity

Represents bone density measurement from a DEXA scan.

- **Attributes**: numeric value (g/cm^2), measurement site (lumbar spine, femoral neck, total hip), T-score, Z-score
- **Validation**: Density must be positive; T-score classification into normal, osteopenia, osteoporosis

## Score Value Objects

### CompositeScore

Represents a calculated health or fitness score.

- **Attributes**: numeric value, scale minimum, scale maximum, component weights, calculation timestamp
- **Validation**: Value must fall within the defined scale range
- **Behavior**: Normalization to different scales; percentile rank calculation

### NormativePercentile

Represents an individual's position relative to population norms.

- **Attributes**: percentile rank (0-100), reference population (age group, sex, ethnicity), sample size
- **Validation**: Percentile must be between 0 and 100
- **Behavior**: Classification into performance categories (below average, average, above average, excellent)

## Design Principles

**Immutability**: All value objects are immutable. A Weight of 75.3 kg cannot be changed to 74.8 kg; instead, a new Weight value object is created. This prevents accidental corruption of measurement data.

**Self-validation**: Each value object validates its own attributes upon creation. It is impossible to instantiate a BodyFatPercentage of 150% -- the constructor enforces the domain constraint.

**Rich behavior**: Value objects encapsulate domain logic related to their attributes. Unit conversion, category classification, and range checking are value object responsibilities, not external service responsibilities.

**No primitive obsession**: The domain model uses value objects instead of primitive types. A method signature of calculateBMI(Weight, Height) is self-documenting and type-safe, while calculateBMI(double, double) is ambiguous and error-prone.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 5 on value objects.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 6 on value objects.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 5 on value objects.
