# Value Objects for Mast Cell Activation Syndrome

## Overview

Value objects are immutable domain objects defined entirely by their attributes rather than by a unique identity. Two value objects with identical attributes are considered equal and interchangeable. In the MCAS domain, value objects capture measurements, scores, classifications, and other domain concepts that do not require individual tracking over time.

Value objects are preferred over entities when identity is not important. They simplify the domain model by eliminating identity management overhead and make the system easier to reason about because value objects cannot change after creation. Any modification produces a new value object rather than altering the existing one.

## Diagnostic Assessment Value Objects

### Tryptase Level

The Tryptase Level value object represents a single serum tryptase measurement. It contains the measured value in micrograms per liter, the collection timestamp, and the specimen type. Two Tryptase Level objects with the same value, timestamp, and specimen type are considered equal. The value object includes behavior to compare against a baseline level and determine whether the elevation meets consensus criteria thresholds, typically defined as the baseline value multiplied by 1.2 plus 2 ng/mL.

### Histamine Metabolite Level

The Histamine Metabolite Level value object represents a urine N-methylhistamine measurement. It contains the measured value, the collection period duration, and the units of measurement. The value object can evaluate whether the level exceeds the upper limit of the laboratory reference range.

### Prostaglandin D2 Level

The Prostaglandin D2 Level value object captures the urine 11-beta-prostaglandin F2-alpha measurement. It contains the measured value, collection period, and units. Like other mediator level value objects, it provides comparison behavior against reference ranges.

### Consensus Criteria Result

The Consensus Criteria Result value object encapsulates the outcome of evaluating a patient's diagnostic data against the MCAS consensus criteria. It contains boolean fields for each criterion element: symptomatic, mediator elevation documented, and treatment response demonstrated. The value object provides a method to determine whether all criteria are met.

### Mediator Elevation

The Mediator Elevation value object represents the calculated elevation of a mediator above baseline. It contains the acute level, the baseline level, and the calculated elevation percentage or absolute difference. This value object is used in criteria evaluation to determine clinical significance.

## Trigger Management Value Objects

### Trigger Classification

The Trigger Classification value object categorizes a trigger by type: environmental, dietary, pharmacological, physical, or emotional. It may also include a subtype for more granular categorization. The classification is immutable once assigned to a trigger entry.

### Confidence Level

The Confidence Level value object represents the certainty with which a trigger has been identified. It contains a level designation such as suspected, probable, or confirmed, along with the evidence basis. The confidence level is determined by the number and consistency of documented exposure-reaction correlations.

### Reaction Window

The Reaction Window value object captures the temporal relationship between trigger exposure and symptom onset. It contains the minimum and maximum observed latency periods and the typical onset time. This value object supports pattern analysis by quantifying how quickly symptoms follow exposure.

## Medication Protocol Value Objects

### Dosage

The Dosage value object represents the amount of medication prescribed for a single administration. It contains the numeric value, the unit of measurement, and the route of administration. Two identical dosages are interchangeable because the specific instance does not matter, only the prescription information.

### Titration Step

The Titration Step value object represents a single step in a titration plan. It contains the target dosage, the duration at that dosage before the next adjustment, and any monitoring requirements. A titration plan is composed of an ordered sequence of Titration Step value objects.

### Excipient Exclusion

The Excipient Exclusion value object identifies a specific inactive ingredient that must be excluded from a compounded medication. It contains the excipient name, the reason for exclusion, and the date the exclusion was established. The exclusion is immutable; if circumstances change, a new value object is created.

## Symptom Tracking Value Objects

### Severity Score

The Severity Score value object represents the intensity rating assigned to a symptom. It contains a numeric value on a defined scale, the scale definition, and the assessor type (patient-reported or clinician-assessed). Two identical severity scores are equal regardless of which symptom entry they are associated with.

### Organ System

The Organ System value object classifies a symptom by the affected body system. It contains the system name (dermatological, gastrointestinal, cardiovascular, neurological, respiratory, or musculoskeletal) and supports grouping symptoms for multi-system analysis.

### Symptom Duration

The Symptom Duration value object captures how long a symptom persisted. It contains the start time, end time, and calculated duration. The value object supports flare analysis by quantifying symptom episodes.

## Outcomes Measurement Value Objects

### Quality of Life Score

The Quality of Life Score value object represents a standardized assessment result. It contains the instrument name, the numeric score, the scoring date, and the interpretation category. The value object supports longitudinal comparison by providing a normalized scoring method.

### Functional Status Rating

The Functional Status Rating value object captures a patient's ability to perform daily activities. It contains ratings for physical function, social participation, work capacity, and self-care. The value object is immutable and represents a snapshot at a point in time.

## Design Principles

Value objects in the MCAS domain follow several design principles. They are always immutable. Equality is based on attribute comparison. They contain self-validation logic to ensure they are always in a valid state. They encapsulate domain-specific behavior such as threshold comparisons and score interpretations.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 5 on value objects.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 6 on value objects.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 4 on modeling with value objects.
