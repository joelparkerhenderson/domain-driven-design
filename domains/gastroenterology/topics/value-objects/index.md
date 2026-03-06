# Value Objects in Gastroenterology

## Overview

A value object is an immutable domain object defined by its attributes rather than a
unique identity. Two value objects with identical attributes are considered equal and
interchangeable. In gastroenterology, value objects represent clinical measurements,
scoring results, diagnostic codes, and other concepts where the value itself matters
more than the identity of the specific instance.

## Value Object Characteristics

Value objects in the gastroenterology domain follow core DDD principles:

1. Immutability: once created, a value object's attributes never change. A new value
   object is created to represent a changed state.
2. Equality by value: two value objects are equal if all their attributes are equal.
3. Self-validation: a value object validates its own attributes upon creation and
   rejects invalid states.
4. Side-effect-free behavior: value object methods return new value objects or computed
   results without modifying internal state.

## Clinical Scoring Value Objects

### MELD Score

The MELD Score value object encapsulates the Model for End-Stage Liver Disease
calculation. It contains bilirubin, creatinine, INR, and sodium values along with the
computed score and mortality prediction. The value object validates that all components
are within physiologically plausible ranges and applies the standard MELD formula.
Creating a new MELD Score from updated lab values produces a new value object instance.

### Child-Pugh Score

The Child-Pugh Score value object contains five components (bilirubin, albumin, INR,
ascites grade, encephalopathy grade) and the resulting classification (A, B, or C).
Each component is scored 1-3 points. The value object enforces that the total score
correctly maps to the assigned class.

### Mayo Score

The Mayo Score value object quantifies ulcerative colitis disease activity using four
components: stool frequency, rectal bleeding, endoscopic findings, and physician global
assessment. Each component scores 0-3, yielding a total of 0-12. The value object
validates component ranges and computes the correct total.

### Harvey-Bradshaw Index

The Harvey-Bradshaw Index value object measures Crohn's disease activity. It contains
general well-being, abdominal pain, number of liquid stools, abdominal mass assessment,
and complication count. The value object enforces valid scoring and classifies activity
as remission, mild, moderate, or severe.

## Diagnostic Code Value Objects

### ICD-10 Code

The ICD-10 Code value object represents a standardized diagnosis code. It validates
the code format, stores the code value and description, and provides clinical category
classification. This value object is used across bounded contexts for diagnostic
documentation and billing compliance.

### CPT Code

The CPT Code value object represents a procedure code used in the Endoscopic Procedures
Context. It validates the code format and stores the procedure description, enabling
accurate procedural documentation and reimbursement tracking.

## Clinical Measurement Value Objects

### LabResult

The LabResult value object contains a LOINC code, numeric value, unit of measurement,
reference range, and abnormality flag. It validates that the value and unit are
compatible and that the abnormality flag correctly reflects the reference range
comparison. Lab results are immutable records of measurements at a point in time.

### VitalSigns

The VitalSigns value object captures blood pressure, heart rate, respiratory rate,
temperature, oxygen saturation, and weight recorded during a clinical encounter.
It validates physiologically plausible ranges for all measurements.

### PressureMeasurement

The PressureMeasurement value object is used in the Motility Disorders Context to
represent manometric pressure readings. It contains the pressure value in mmHg, the
measurement location (anatomical site), and the measurement type (resting, swallow,
transient). It validates measurement ranges appropriate for the anatomical location.

## Time and Duration Value Objects

### DateRange

The DateRange value object represents a clinical time period, such as the duration of
a flare episode or the monitoring period for a pH study. It contains start and end
dates, validates that the end date does not precede the start date, and provides
duration calculation.

### EncounterDate

The EncounterDate value object represents the date and time of a clinical encounter
with associated timezone information. It enforces that encounter dates are not in the
future and provides formatting for clinical documentation.

## Classification Value Objects

### MontrealClassification

The Montreal Classification value object categorizes IBD by age at diagnosis, disease
location, and disease behavior for Crohn's disease, or by extent of colonic involvement
for ulcerative colitis. It enforces valid classification combinations.

### ChicagoClassification

The Chicago Classification value object categorizes esophageal motility disorders based
on manometry findings. It validates that the classification is consistent with the
provided manometric parameters and identifies the specific motility disorder subtype.

## Design Rationale

Value objects are used extensively in the gastroenterology domain because clinical
medicine involves many concepts that are defined by their values: a lab result is
its measured value, a score is its calculated total, a diagnosis code is its assigned
classification. Making these value objects immutable ensures that historical clinical
records are never inadvertently altered.

## References

- Evans, E. (2003). *Domain-Driven Design*. Addison-Wesley. Chapter 5: Value Objects.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley. Chapter 6: Value Objects.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media. Chapter 8: Value Objects.
