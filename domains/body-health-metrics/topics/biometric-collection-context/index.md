# Biometric Collection Context

## Overview

The Biometric Collection Context is responsible for capturing, validating, and standardizing fundamental body health measurements. It serves as the primary data entry point for the body health metrics system, owning the measurement session lifecycle from initiation through completion. This context operates closest to the physical measurement instruments and enforces data quality at the point of capture, ensuring that all downstream contexts receive reliable, well-structured measurement data.

## Scope and Responsibilities

This context manages the following measurement categories:

- **Weight**: Body mass measured via scales, recorded in kilograms or pounds with defined precision
- **Height**: Standing height measured via stadiometer, recorded in centimeters, meters, or feet/inches
- **Body Mass Index (BMI)**: Calculated from weight and height; owned by this context as a derived biometric
- **Body Fat Percentage**: Estimated via bioelectrical impedance, skinfold calipers, or accepted from external DEXA results
- **Waist Circumference**: Measured at a specified anatomical landmark (narrowest point, umbilical level, or iliac crest)
- **Vital Signs**: Heart rate, blood pressure (systolic/diastolic), respiratory rate, body temperature

## Aggregate Design

### MeasurementSession (Aggregate Root)

The MeasurementSession aggregate groups all measurements captured during a single assessment event. It enforces temporal consistency (all measurements share the same session timestamp), prevents duplicate measurement types within a session, and validates each measurement against physiological plausibility ranges.

**Invariants**:
- A session must contain at least one measurement
- No duplicate measurement types within a single session
- All measurements must pass physiological plausibility validation
- A completed session cannot accept additional measurements
- Device calibration status must be recorded for each measurement

### Key Value Objects

- **Weight**: Value with unit (kg/lb), precision, and plausibility range (0.5-635 kg)
- **Height**: Value with unit (cm/m/ft-in), precision, and plausibility range (50-280 cm)
- **BloodPressure**: Systolic and diastolic values in mmHg with classification
- **HeartRate**: Beats per minute with measurement context (resting/active/recovery)
- **BodyTemperature**: Value with unit (Celsius/Fahrenheit) and measurement site (oral/tympanic/axillary)
- **MeasurementConditions**: Time of day, fasting status, clothing status, environmental conditions

## Domain Events Published

- **BiometricMeasurementRecorded**: Raised for each individual measurement captured, carrying the full measurement data with units and precision
- **MeasurementSessionCompleted**: Raised when a session is finalized, signaling to downstream contexts that a complete measurement set is available
- **PhysiologicalPlausibilityViolation**: Raised when a submitted value falls outside biologically plausible ranges

## Domain Services

- **BMICalculationService**: Calculates BMI from weight and height value objects, applies WHO classification thresholds
- **WaistToHipRatioService**: Computes waist-to-hip ratio and cardiometabolic risk classification from circumference measurements

## Integration Points

- **Upstream**: Receives externally sourced measurements from Clinical Integration Context via Open Host Service
- **Downstream**: Publishes measurement events consumed by Body Composition, Fitness Assessment, Health Scoring, and Progress Tracking contexts

## Data Quality Constraints

The Biometric Collection Context enforces multiple layers of data quality:

1. **Type safety**: Measurements are captured as strongly-typed value objects, not raw numbers
2. **Unit enforcement**: Every numeric value carries an explicit unit of measure
3. **Plausibility validation**: Values are checked against physiologically possible ranges
4. **Device attribution**: Each measurement records the device and its calibration status
5. **Temporal consistency**: Session-level timestamps ensure measurement synchronization
6. **Completeness tracking**: The session tracks which planned measurements have been captured versus which remain outstanding

## Ubiquitous Language (Context-Specific)

- **Measurement Session**: The act of recording one or more biometric measurements at a specific time
- **Raw Reading**: The unprocessed value directly from a measurement device before any calculation or normalization
- **Calibration Status**: Whether a device has been verified for accuracy within its certification window
- **Plausibility Range**: The biologically possible value range for a given measurement type
- **Assessment Conditions**: The environmental and physiological state factors that may influence measurement accuracy (fasting, time of day, hydration, clothing)

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- World Health Organization. *Physical Status: The Use and Interpretation of Anthropometry*. WHO Technical Report Series 854, 1995.
- American College of Sports Medicine. *ACSM's Guidelines for Exercise Testing and Prescription*. 11th ed. Wolters Kluwer, 2021.
