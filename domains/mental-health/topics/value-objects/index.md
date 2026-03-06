# Value Objects in the Mental Health Domain

## Overview

A value object is an immutable domain object defined entirely by its
attributes rather than by a unique identity. Two value objects with the
same attributes are considered equal and interchangeable. Value objects
capture descriptive aspects of the domain that do not require individual
tracking over time. In the mental health domain, value objects represent
clinical measurements, diagnostic codes, dosage specifications, and other
concepts where the value itself is what matters, not which particular
instance it is.

Value objects are a cornerstone of tactical DDD design. They reduce
complexity by eliminating unnecessary identity management and improve
model expressiveness by creating self-validating, semantically rich types
that replace primitive obsession.

## Value Objects by Bounded Context

### Clinical Assessment Context

**DiagnosticCode**: Encapsulates a DSM-5 or ICD-11 diagnosis code along
with its description and any specifiers (e.g., severity, course). Two
DiagnosticCode objects with the same code and specifiers are equal.
Validation ensures the code conforms to the coding system's format.

**AssessmentScore**: Represents the result of scoring a standardized
instrument. Contains the instrument identifier, raw score, and severity
interpretation (e.g., PHQ-9 score of 15 = "moderately severe depression").
The score is immutable once calculated; a new assessment produces a new
score.

**SeverityRating**: A graduated clinical severity level (minimal, mild,
moderate, moderately severe, severe) derived from assessment scoring.
Used across contexts to communicate symptom severity without carrying
the full assessment details.

### Treatment Planning Context

**CareLevel**: Represents the intensity of care (outpatient, intensive
outpatient, partial hospitalization, residential, inpatient). Includes
the clinical criteria that justify the level. Care levels follow the
American Society of Addiction Medicine (ASAM) or similar frameworks.

**ModalitySelection**: Describes a selected therapeutic modality (CBT,
DBT, EMDR, psychodynamic therapy) along with its evidence base for the
patient's diagnosis. Two identical modality selections for the same
diagnosis are interchangeable.

**GoalCriteria**: Defines the measurable criteria for a treatment goal,
including the target metric, baseline value, target value, and
measurement instrument. Goal criteria are immutable; changing them
creates a new GoalCriteria value object.

### Therapy Management Context

**SessionDuration**: Encapsulates the length of a therapy session
(typically 30, 45, 50, 60, or 90 minutes) along with the billing unit
type. Validates that the duration falls within acceptable clinical
ranges.

**TherapeuticIntervention**: Describes a specific technique used during
a session (e.g., cognitive restructuring, exposure exercise, mindfulness
practice). Contains the intervention name and the modality it belongs to.

**AppointmentSlot**: Represents a specific date, time, and duration
available for scheduling. Two slots with the same attributes are
identical. Slots do not carry identity because they represent
availability, not booked sessions.

### Crisis Intervention Context

**RiskLevel**: A structured assessment of risk (low, moderate, high,
imminent) with the factors that support the determination. Risk levels
are immutable snapshots of risk at a moment in time; they do not change
because risk is reassessed by creating a new RiskLevel.

**ProtectiveFactor**: Represents a factor that reduces risk, such as
social support, engagement in treatment, or reasons for living. Contains
the factor description and its assessed strength.

**RiskFactor**: Represents a factor that increases risk, such as prior
attempts, substance use, access to means, or recent losses. Contains
the factor description and its assessed relevance.

### Medication Management Context

**Dosage**: Encapsulates the amount of medication, the unit of measure,
the route of administration, and the frequency. A Dosage of "50 mg oral
daily" is a value object; changing the dose creates a new Dosage rather
than mutating the existing one.

**DrugInteraction**: Describes a clinically significant interaction
between two medications, including the severity (minor, moderate, major,
contraindicated), the mechanism, and the clinical significance.

**MedicationIdentifier**: Wraps a medication's name, generic name,
drug class, and formulary status into a single value object. Two
references to the same medication with the same attributes are equal.

### Outcomes Measurement Context

**InstrumentScore**: Represents a single measurement from a validated
instrument at a point in time. Contains the instrument name, raw score,
percentile (if applicable), and clinical interpretation band.

**ChangeMetric**: Represents the calculated change between two
measurement points, including the absolute change, percentage change,
and whether the change meets the threshold for clinically significant
improvement or reliable change.

**FunctionalDomain**: Identifies a specific area of functioning being
assessed (work, social relationships, self-care, daily activities) along
with the assessment method used.

## Design Principles

Value objects in the mental health domain follow these principles:

- Immutability: once created, a value object cannot be changed. Any
  modification produces a new instance.
- Self-validation: value objects validate their own attributes at
  construction time. An invalid PHQ-9 score (outside 0-27) cannot exist.
- Side-effect-free behavior: methods on value objects return new values
  rather than modifying state.
- Equality by attributes: two DiagnosticCode objects with the same code
  and specifiers are equal regardless of how they were created.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 5 on value objects.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 6 on value objects.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 5 on value objects.
