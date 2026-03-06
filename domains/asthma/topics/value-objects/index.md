# Value Objects - Asthma Domain

## Overview

Value objects are immutable domain objects defined entirely by their attributes rather than by a unique identity. Two value objects with the same attributes are considered equal and interchangeable. In the asthma management domain, value objects represent clinical measurements, classifications, scores, thresholds, and other concepts where the values themselves carry meaning without needing to be individually tracked over time.

Value objects are fundamental to modeling the asthma domain because clinical measurements, drug dosages, severity classifications, and quality scores are inherently value-based. An FEV1 reading of 2.5 liters is meaningful because of its numeric value and units, not because of a unique identifier assigned to that particular measurement.

## Value Object Design Principles

**Immutability.** Once created, a value object cannot be modified. If a different value is needed, a new value object is created. This prevents accidental corruption of clinical measurements and classifications.

**Self-Validation.** Value objects validate their own construction. An FEV1 value object rejects negative values. An ACTScore value object rejects values outside the 5-25 range. This pushes validation to the point of creation, preventing invalid data from propagating through the system.

**Side-Effect-Free Behavior.** Value objects can contain behavior (methods) but those methods must not modify state. They may compute derived values, compare with other value objects, or format for display.

**Equality by Attributes.** Two value objects are equal if all their attributes are equal. An FEV1 value object of 2.5L equals another FEV1 value object of 2.5L regardless of when or where they were created.

## Clinical Assessment Value Objects

### FEV1 (Forced Expiratory Volume in One Second)

**Attributes:** Value (liters), predicted value (liters), percent predicted (%).

**Validation:** Value must be positive. Percent predicted is calculated as (actual/predicted) * 100. Values below 20% or above 150% of predicted are flagged as potentially erroneous.

**Behavior:** Calculates percent predicted. Determines if value is below lower limit of normal. Supports comparison for bronchodilator reversibility calculation.

### FVC (Forced Vital Capacity)

**Attributes:** Value (liters), predicted value (liters), percent predicted (%).

**Validation:** Value must be positive. Must be greater than or equal to the corresponding FEV1 value from the same maneuver.

### FEV1FVCRatio

**Attributes:** Ratio value (decimal, 0.0 to 1.0), lower limit of normal (decimal).

**Validation:** Ratio must be between 0.0 and 1.0. Lower limit of normal is age-dependent.

**Behavior:** Determines if the ratio indicates obstructive pattern (below lower limit of normal). Distinguishes restrictive from obstructive patterns.

### PEFReading (Peak Expiratory Flow)

**Attributes:** Value (liters per minute), timestamp, measurement context (home, clinic).

**Validation:** Value must be between 60 and 900 L/min for adults. Pediatric ranges differ.

### FeNOReading (Fractional Exhaled Nitric Oxide)

**Attributes:** Value (parts per billion), interpretation (low, normal, intermediate, high).

**Validation:** Value must be non-negative. Interpretation follows ATS clinical practice guidelines: low (<25 ppb adults), intermediate (25-50 ppb), high (>50 ppb).

**Behavior:** Determines likelihood of eosinophilic inflammation. Guides ICS responsiveness prediction.

### GINAClassification

**Attributes:** Severity level (intermittent, mild persistent, moderate persistent, severe persistent).

**Validation:** Must be one of the four defined GINA severity levels.

**Behavior:** Determines initial treatment step recommendation. Supports comparison for severity progression tracking.

### ControlLevel

**Attributes:** Level (well-controlled, partly controlled, uncontrolled), assessment criteria met (daytime symptoms, night waking, reliever use, activity limitation).

**Validation:** Level must be one of three defined values. The number of criteria met determines the level (0 = well-controlled, 1-2 = partly controlled, 3-4 = uncontrolled).

### QualityGrade

**Attributes:** Grade (A, B, C, D, F), acceptability (acceptable, not acceptable), reproducibility (reproducible, not reproducible).

**Validation:** Grade must follow ATS/ERS spirometry quality grading system.

## Treatment Management Value Objects

### TherapyStep

**Attributes:** Step number (1-5), preferred controller, alternative controller, reliever therapy.

**Validation:** Step number must be between 1 and 5. Each step has defined preferred and alternative therapies per GINA guidelines.

### EligibilityCriteria

**Attributes:** Required biomarker thresholds (IgE range, eosinophil count, FeNO level), required prior therapy failures, phenotype requirements.

**Validation:** Threshold values must be within clinically meaningful ranges. Criteria are biologic-agent-specific.

### DosingSchedule

**Attributes:** Frequency (daily, twice daily, every 2 weeks, monthly), dose amount, dose units, administration route (inhaled, subcutaneous, intravenous).

**Validation:** Frequency must be a recognized dosing interval. Dose must be within approved range for the specific medication.

## Action Planning Value Objects

### ZoneThreshold

**Attributes:** Zone (green, yellow, red), upper PEF boundary (L/min or % personal best), lower PEF boundary, symptom criteria.

**Validation:** Green zone lower boundary must equal yellow zone upper boundary. Yellow zone lower boundary must equal red zone upper boundary. Boundaries must be within 0-100% of personal best.

### PersonalBest

**Attributes:** PEF value (L/min), date established, conditions (well-controlled, post-bronchodilator).

**Validation:** Value must be positive. Must be established during a period of well-controlled asthma or after maximal bronchodilator response.

## Medication Management Value Objects

### AdherenceRate

**Attributes:** Rate (percentage, 0-100), measurement period (days), measurement method (pharmacy refill, electronic monitor, self-report).

**Validation:** Rate must be between 0 and 100. Measurement period must be positive.

**Behavior:** Classifies adherence as good (>80%), partial (50-80%), or poor (<50%).

### RefillStatus

**Attributes:** Last refill date, next expected refill date, refills remaining, days supply remaining.

**Validation:** Refills remaining must be non-negative. Days supply must be positive.

## Outcomes Tracking Value Objects

### ACTScore

**Attributes:** Total score (5-25), individual question scores (1-5 each), assessment date.

**Validation:** Total score must be sum of five question scores. Each question score must be 1-5.

**Behavior:** Classifies control as well-controlled (>=20), not well-controlled (16-19), or very poorly controlled (<=15). Calculates minimal clinically important difference (3 points) from prior score.

### QualityOfLifeScore

**Attributes:** Overall score (1-7), domain scores (symptoms, activity limitation, emotional function, environmental stimuli), instrument (AQLQ, mini-AQLQ), assessment date.

**Validation:** Scores must be within 1-7 range. Domain scores must match the specified instrument's domains.

### ExacerbationSummary

**Attributes:** Count in period, period length, rate (exacerbations per year), severity distribution (mild, moderate, severe counts).

**Validation:** Count must be non-negative. Rate is calculated as (count / period) * 365.25.

## Value Object Composition

Value objects can compose other value objects. A SpirometryResult value object composes FEV1, FVC, FEV1FVCRatio, and QualityGrade value objects. An ActionPlanZone composes a ZoneThreshold value object with medication instructions. This composition creates rich, self-validating domain concepts without requiring entity identity.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 5 on value objects.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 6 on value objects.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 4 on value objects.
- American Thoracic Society / European Respiratory Society. Standardization of Spirometry, 2019.
