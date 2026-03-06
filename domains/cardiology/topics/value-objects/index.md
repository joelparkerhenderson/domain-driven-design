# Value Objects in Cardiology

## Overview

Value objects are immutable domain objects defined by their attributes rather than by identity. In the cardiology domain, value objects represent measurements, classifications, scores, and clinical parameters that are characterized by what they are rather than who they are. Two troponin measurements with the same value, units, and reference range are interchangeable -- there is no need to distinguish between them by identity.

## Value Object Characteristics in Cardiology

Cardiology value objects exhibit specific characteristics:

- **Immutability**: Once created, a value object does not change. A blood pressure reading of 140/90 mmHg remains 140/90 mmHg. If the measurement changes, a new value object is created.
- **Equality by attributes**: Two value objects are equal if all their attributes are equal. Two LVEF measurements of 35% with the same method and date are considered equal.
- **Self-validation**: Value objects validate their own invariants on creation. A heart rate value object rejects negative values or values exceeding physiological limits.
- **Side-effect free behavior**: Value objects may contain calculation methods, but these return new value objects rather than modifying existing state.

## Key Value Objects by Bounded Context

### Clinical Assessment Context

**BloodPressure**: Systolic and diastolic values in mmHg with position (seated, supine, standing) and arm (left, right). Self-validates that systolic exceeds diastolic and both are within physiological range.

**HeartRate**: Beats per minute with rhythm notation (regular, irregular). Validates range of 20-300 bpm.

**CardiacBiomarkerResult**: Analyte type (troponin, BNP, NT-proBNP, CK-MB), numerical value, units, reference range, and clinical interpretation (normal, elevated, critically elevated). Validates unit consistency.

**RiskScore**: Score type (Framingham, ASCVD, HEART, TIMI, GRACE), calculated value, risk category (low, intermediate, high), and input parameter snapshot. Ensures score value falls within the valid range for the score type.

**BodyMassIndex**: Weight in kilograms, height in meters, calculated BMI value, and WHO classification (underweight, normal, overweight, obese). Self-calculates from weight and height.

### Diagnostic Imaging Context

**EjectionFraction**: Percentage value (0-100), measurement method (biplane Simpson, Teichholz, visual estimate, 3D), and classification (normal >=55%, mildly reduced 45-54%, moderately reduced 30-44%, severely reduced <30%).

**ChamberDimension**: Measurement value, unit (mm or cm), chamber identifier (LV, RV, LA, RA), dimension type (end-diastolic, end-systolic, diameter, volume), and reference range based on body surface area indexing.

**ValveGradient**: Peak and mean gradient values in mmHg, valve identifier (aortic, mitral, tricuspid, pulmonic), and severity classification (mild, moderate, severe) based on guideline thresholds.

**WallMotionScore**: Segment identifier (using 16- or 17-segment model), motion grade (normal, hypokinetic, akinetic, dyskinetic, aneurysmal), and wall motion score index calculation.

**CalciumScore**: Agatston score value, percentile for age and sex, and CAC risk category (0, 1-99, 100-299, >=300).

### Interventional Procedures Context

**StenosisPercentage**: Percentage value (0-100) representing the degree of vessel narrowing, with severity classification (non-significant <50%, significant 50-69%, severe 70-99%, total occlusion 100%).

**SYNTAXScore**: Calculated score value, complexity category (low <=22, intermediate 23-32, high >=33), and contributing lesion scores.

**ContrastVolume**: Volume in milliliters with contrast agent type. Used for tracking contrast exposure and calculating contrast-to-creatinine clearance ratio.

**FluoroscopyDose**: Dose area product and fluoroscopy time, used for radiation exposure monitoring and reporting.

### Electrophysiology Context

**HeartRhythm**: Rhythm classification (sinus, atrial fibrillation, atrial flutter, ventricular tachycardia, etc.), rate, and regularity. Based on standard arrhythmia classification schemes.

**ConductionInterval**: Interval type (PR, QRS, QT, QTc), duration in milliseconds, and normal/abnormal classification. QTc self-calculates using Bazett or Fridericia formula from QT and heart rate.

**DeviceThreshold**: Threshold type (sensing, pacing, impedance), measured value with units (mV for sensing, V/ms for pacing, ohms for impedance), and acceptable range determination.

**CHA2DS2VAScScore**: Individual component scores and total score (0-9) for stroke risk stratification in atrial fibrillation, with annual stroke risk percentage and anticoagulation recommendation.

### Heart Failure Management Context

**NYHAFunctionalClass**: Class value (I, II, III, IV) with descriptive criteria and assessment date. Supports comparison operations for tracking functional status changes.

**ACCAHAStage**: Stage value (A, B, C, D) with stage criteria and assessment date. Stage can only advance (A to B to C to D), never regress, per ACC/AHA guidelines.

**MedicationDose**: Drug name, dose amount, dose unit, frequency, and percentage of target dose achieved. Supports titration increment calculations.

**FluidBalance**: Intake volume, output volume, net balance in milliliters, and time period. Used for volume status monitoring.

### Cardiac Rehabilitation Context

**METCapacity**: Measured or estimated metabolic equivalent capacity, assessment method (exercise test, estimation formula), and fitness classification for age and sex.

**ExercisePrescription**: Exercise mode (treadmill, bicycle, arm ergometer), target heart rate range, target RPE (Rating of Perceived Exertion), duration in minutes, and frequency per week.

**LipidPanel**: Total cholesterol, LDL, HDL, and triglycerides in mg/dL with ATP classification and goal attainment status.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 5 on value objects.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 6 on value object implementation.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 5 on value objects.
- ASE Guidelines for Cardiac Chamber Quantification. *JASE*, 2015.
