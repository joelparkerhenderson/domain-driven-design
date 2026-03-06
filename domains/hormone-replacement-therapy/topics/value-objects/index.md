# Value Objects in the Hormone Replacement Therapy Domain

Value objects are immutable domain objects defined entirely by their attributes rather than by a unique identity. Two value objects with the same attributes are considered equal. In the hormone replacement therapy (HRT) domain, value objects model the measurements, quantities, classifications, and descriptors that characterize clinical data without requiring persistent identity tracking.

## Purpose

HRT systems handle large volumes of clinical measurements, dosage specifications, and classification data that are meaningful for their content rather than their identity. A dosage of "2 mg estradiol daily" is the same regardless of when or where it was specified. A symptom score of 24 on the Menopause Rating Scale is identical to any other score of 24 on the same instrument. Value objects capture these concepts with immutability guarantees that simplify reasoning and prevent accidental mutation of shared clinical data.

## Clinical Assessment Context Value Objects

### SymptomScore

Represents the quantified result of a symptom assessment instrument. Attributes include the instrument name (such as Menopause Rating Scale or Kupperman Index), the composite score, the scoring date, and the severity classification derived from the score. The value object validates that the score falls within the valid range for the specified instrument.

### CandidacyStatus

Represents the determination of whether a patient is eligible for HRT. Possible values include approved, conditionally approved, deferred, and declined. Each status carries a rationale string. This is a value object because the status itself is a descriptive classification, not a tracked entity.

### RiskFactorProfile

Represents the collection of clinical risk factors relevant to HRT safety assessment. Attributes include individual risk factors such as thromboembolism history, breast cancer history, cardiovascular disease, liver disease, and undiagnosed vaginal bleeding. Each factor includes a presence indicator and relevant details. The profile is immutable; changes produce a new profile instance.

### HormoneLevel

Represents a single hormone measurement with its value, unit of measurement, reference range, and interpretation flag (low, normal, high). Used within hormone panel interpretation to convey individual test results.

## Hormone Protocol Context Value Objects

### Dosage

Represents a specific hormone dose specification. Attributes include the numeric amount, unit of measurement (mg, mcg, IU), frequency (daily, twice weekly, weekly), and any time-of-day instruction. The value object validates that the amount is positive and the unit is appropriate for the specified hormone.

### DeliveryMethod

Represents the route of hormone administration. Attributes include the method category (transdermal, oral, sublingual, injectable, vaginal, subcutaneous pellet), specific form factor (patch, gel, cream, tablet, injection, ring, pellet), and any method-specific parameters such as patch change frequency or injection site rotation.

### CyclingPattern

Represents the temporal pattern of hormone administration, particularly relevant for progesterone therapy. Attributes include cycle length in days, active days, and rest days. Common patterns include continuous (no cycling), sequential (12-14 days per month), and cyclic (3 weeks on, 1 week off).

### ProtocolStatus

Represents the current state of a treatment protocol. Possible values include draft, pending approval, active, suspended, completed, and discontinued. Each status carries a reason and effective date. Status transitions are validated against allowed transition rules.

### FormulationSpecification

Represents the detailed pharmaceutical specification of a hormone preparation. Attributes include active ingredient, concentration, inactive ingredients, manufacturer, NDC code, and regulatory approval status. Two formulations with identical attributes are interchangeable.

## Lab Monitoring Context Value Objects

### TherapeuticTargetRange

Represents the clinically defined acceptable range for a hormone measurement during active treatment. Attributes include the lower bound, upper bound, unit, and the clinical basis for the range. This differs from a standard reference range because it reflects treatment goals rather than population norms.

### LabResult

Represents a single laboratory test result. Attributes include the test code, result value, unit, reference range, collection timestamp, and reporting laboratory. The value object includes interpretation logic that compares the result against both reference ranges and therapeutic targets.

### TrendAnalysis

Represents the calculated trend across sequential laboratory measurements. Attributes include trend direction (increasing, decreasing, stable), rate of change, number of data points, and time span. This value object summarizes temporal patterns without maintaining the underlying data points.

### MonitoringFrequency

Represents the schedule frequency for laboratory monitoring. Attributes include interval type (days, weeks, months), interval count, and any conditional modifiers (such as increased frequency during dose titration).

## Side Effect Management Context Value Objects

### SeverityGrade

Represents the clinical severity classification of an adverse event. Follows standard grading scales such as CTCAE (Common Terminology Criteria for Adverse Events) with grades from 1 (mild) through 5 (death). Attributes include the grade number, grade description, and the classification system used.

### EventClassification

Represents the categorization of an adverse event by system organ class, preferred term, and relationship to treatment. Follows MedDRA terminology for standardized adverse event classification.

### ResolutionStatus

Represents the current resolution state of an adverse event. Possible values include ongoing, resolving, resolved, resolved with sequelae, and fatal. Carries a status date and any relevant notes.

## Treatment Optimization Context Value Objects

### DoseAdjustment

Represents a proposed change to a hormone dosage. Attributes include the current dose, proposed dose, adjustment direction (increase, decrease), adjustment magnitude, and clinical rationale for the change.

### TitrationDirection

Represents the direction of dose titration. Values include up-titrate, down-titrate, maintain, and discontinue. Carries a confidence level and supporting evidence count.

### EvidenceSummary

Represents a summary of clinical evidence supporting an optimization recommendation. Attributes include evidence sources (lab trends, side effect history, outcome data), evidence strength, and consistency assessment.

## Outcomes Tracking Context Value Objects

### QualityOfLifeScore

Represents a standardized quality of life measurement. Attributes include the instrument name, domain scores (physical, emotional, functional), composite score, and assessment date.

### SymptomResolutionRate

Represents the degree of symptom improvement as a percentage change from baseline. Attributes include the symptom category, baseline score, current score, and calculated resolution percentage.

### EfficacyRating

Represents an overall assessment of treatment effectiveness. Attributes include the rating category (highly effective, effective, partially effective, ineffective), the assessment basis, and the evaluation time point.

## Design Principles

Value objects in the HRT domain are always immutable. Any change produces a new instance. They include self-validation logic to ensure that their attributes are internally consistent. They implement equality based on attribute comparison. They are freely shareable because immutability guarantees thread safety and prevents unintended side effects.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 5 on value objects.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 6 on value objects.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 5 on implementing business logic.
