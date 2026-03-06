# Value Objects in the Gynecology Domain

## Overview

A value object is an immutable domain object defined entirely by its attributes rather than by a unique identity. Two value objects with identical attributes are considered equal and interchangeable. In the gynecology domain, value objects represent clinical measurements, classifications, scores, and descriptive data that characterize entities and aggregates without requiring their own identity.

## Purpose

Gynecological care generates large amounts of descriptive clinical data: blood pressure readings, gestational age calculations, hormone levels, screening result classifications, and procedure type designations. These data elements are defined by their content, not by identity. A blood pressure reading of 120/80 mmHg is the same regardless of when or where it was recorded. Value objects model these data elements cleanly, ensuring immutability and structural equality.

## Key Value Objects by Bounded Context

### Clinical Assessment Context

- **BloodPressure**: Systolic and diastolic values in mmHg, with validation rules for physiologically plausible ranges.
- **BodyMassIndex**: Calculated from height and weight, with classification category (underweight, normal, overweight, obese).
- **PainScore**: Numeric rating scale value (0-10) with descriptive category.
- **SymptomDescription**: Structured description including location, duration, severity, and character.
- **DiagnosisCode**: ICD-10 code with display text, validated against the gynecology-relevant code set.

### Reproductive Health Context

- **MenstrualCycleSummary**: Cycle length, duration of flow, flow volume, and regularity classification.
- **HormonalLevel**: Hormone type (FSH, LH, estradiol, progesterone), measured value, unit, and reference range.
- **ContraceptiveMethod**: Method name, category (hormonal, barrier, intrauterine, permanent), and efficacy rating.
- **FertilityScore**: Composite assessment result incorporating ovarian reserve, age factor, and partner factors.
- **OvarianReserve**: AMH level, antral follicle count, and interpreted reserve category (normal, diminished, very low).

### Surgical Services Context

- **ProcedureType**: Procedure name, CPT code, and surgical approach classification (laparoscopic, vaginal, abdominal, robotic).
- **AnesthesiaClassification**: ASA physical status classification with documented risk factors.
- **SurgicalOutcome**: Outcome category (uncomplicated, minor complication, major complication), estimated blood loss, and operative time.
- **RecoveryMilestone**: Milestone name, expected timeframe, and achieved status.

### Prenatal Care Context

- **GestationalAge**: Weeks and days from last menstrual period or ultrasound dating, with method of determination.
- **FetalMeasurement**: Measurement type (crown-rump length, biparietal diameter, femur length), value, and gestational age percentile.
- **AmnioticFluidIndex**: Measured value in centimeters with normal/abnormal classification.
- **BishopScore**: Composite cervical readiness score (0-13) combining dilation, effacement, station, consistency, and position.
- **RiskCategory**: Risk level (low, moderate, high) with documented risk factors that contributed to the classification.

### Screening Programs Context

- **ScreeningTestType**: Test name, methodology (cytology, molecular, imaging), and target condition.
- **ScreeningResult**: Result classification (normal, abnormal, indeterminate), raw values where applicable, and reporting laboratory.
- **ScreeningInterval**: Recommended interval duration (months or years), based on risk category and guideline source.
- **BethesdaClassification**: Pap smear result classified according to the Bethesda System (NILM, ASC-US, LSIL, HSIL, AGC).

### Patient Education Context

- **LiteracyLevel**: Assessed health literacy category (limited, marginal, adequate) based on standardized assessment tools.
- **ContentTopic**: Topic identifier, clinical category, and recommended literacy level.
- **ComprehensionScore**: Numeric score from comprehension assessment with pass/fail threshold.

## Value Object Design Principles

1. Value objects are immutable. Once created, their attributes never change. Any modification produces a new value object.
2. Equality is based on attribute comparison, not identity.
3. Value objects encapsulate validation. A GestationalAge value object rejects negative values or values beyond 45 weeks.
4. Value objects may contain behavior. A GestationalAge can calculate the corresponding trimester. A BishopScore can determine cervical favorability.
5. Value objects simplify aggregate design by extracting descriptive complexity from entities.

## Clinical Standards Encoded as Value Objects

Many value objects in the gynecology domain encode clinical standards:

- BethesdaClassification encodes the Bethesda System for cervical cytology reporting.
- BishopScore encodes the Bishop scoring system for cervical readiness assessment.
- AnesthesiaClassification encodes the ASA physical status classification system.

These value objects ensure that clinical standards are consistently applied throughout the domain model.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 5 on value objects.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 6 on value object design.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 5 on value object patterns.
- Solomon, D. et al. "The 2001 Bethesda System: Terminology for Reporting Results of Cervical Cytology." JAMA, 2002.
