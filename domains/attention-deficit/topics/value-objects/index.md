# Value Objects: Attention-Deficit Domain

Value objects are immutable domain objects defined entirely by their attributes rather than by a unique identity. In the ADHD management domain, value objects represent clinical measurements, diagnostic codes, dosage specifications, accommodation types, and other descriptive elements that are compared by value equality.

## Purpose

ADHD management relies heavily on standardized measurements, classifications, and descriptors. A Conners Rating Scale score of 72 is meaningful because of its value, not because of any unique identity. A DSM-5 diagnostic code of 314.01 carries meaning through its classification value. Value objects capture these concepts with immutability and value-based equality, ensuring that clinical measurements, diagnostic classifications, and treatment parameters are handled with precision and consistency.

## Clinical Assessment Value Objects

### DSM-5 Diagnostic Code

Represents a standardized diagnostic classification from the Diagnostic and Statistical Manual of Mental Disorders, Fifth Edition. Includes the numeric code (e.g., 314.01 for ADHD Combined Presentation), the ICD-10-CM mapping (e.g., F90.2), and the descriptive label. Two diagnostic codes are equal if they represent the same classification. Immutable because a diagnostic code's meaning does not change.

### ADHD Presentation Type

Represents the classification of ADHD into one of three presentation types: predominantly inattentive (314.00/F90.0), predominantly hyperactive-impulsive (314.01/F90.1), or combined (314.01/F90.2). Includes the presentation label and the corresponding diagnostic code. Two presentation types are equal if they represent the same classification.

### Rating Scale Score

Represents a scored result from a standardized ADHD rating scale. Includes the scale name (Conners, SNAP-IV, Vanderbilt, BRIEF), the raw score, the T-score or percentile (where applicable), the normative comparison group, the subscale name, and the clinical significance threshold. Two scores are equal if all attributes match.

### Symptom Severity Level

Represents a categorized severity classification (mild, moderate, severe) based on DSM-5 criteria. Includes the severity category, the number of symptoms meeting threshold, and the degree of functional impairment. Two severity levels are equal if they represent the same categorization.

### Comorbidity Profile

Represents the set of co-occurring conditions identified during assessment. Includes each comorbid diagnosis with its diagnostic code, confidence level, and clinical relevance to ADHD management. Two profiles are equal if they contain the same set of comorbidities.

## Treatment Management Value Objects

### Treatment Modality

Represents a category of therapeutic intervention: behavioral therapy, cognitive-behavioral therapy, parent training, social skills training, coaching, psychoeducation, or environmental modification. Two modalities are equal if they represent the same intervention category.

### Treatment Goal

Represents a specific, measurable objective for a treatment intervention. Includes the target behavior, the measurement method, the baseline level, the target level, and the timeframe. Two goals are equal if all attributes match. Immutable once established within a treatment episode.

### Session Frequency

Represents the scheduled cadence of treatment sessions. Includes the number of sessions per time period, the session duration, and the delivery modality (in-person, telehealth). Two frequencies are equal if they specify the same scheduling parameters.

## Behavioral Monitoring Value Objects

### Symptom Frequency Count

Represents the number of occurrences of a specific ADHD symptom within a defined observation period. Includes the symptom type, the count, the observation period duration, and the setting. Immutable once recorded.

### Attention Span Measurement

Represents a measured duration of sustained attention during a specific task or activity. Includes the duration, the task type, the setting, and the measurement method. Two measurements are equal if all attributes match.

### Behavior Duration

Represents the length of time a specific behavior was exhibited. Includes the behavior type, the start time, the end time, and the setting context. Immutable as a historical record.

## Educational Accommodation Value Objects

### Accommodation Type

Represents a specific category of educational accommodation: extended time, preferential seating, reduced assignment length, frequent breaks, visual schedule, organizational support, testing modification, or behavioral support. Two accommodation types are equal if they represent the same category.

### Eligibility Category

Represents the educational disability category under which a student qualifies for services. For ADHD, this is typically "Other Health Impairment" under IDEA or disability status under Section 504. Includes the legal framework, the category label, and the eligibility criteria met.

## Medication Management Value Objects

### Dosage Amount

Represents a specific medication dose. Includes the numeric value, the unit of measurement (mg), and the formulation type (immediate-release, extended-release). Two dosages are equal if they specify the same amount, unit, and formulation.

### Administration Schedule

Represents the timing pattern for medication administration. Includes the frequency (once daily, twice daily), the time of day, and whether the schedule is for school days only or daily. Two schedules are equal if they specify the same timing pattern.

### Side Effect Severity

Represents the classified severity of an observed medication side effect. Includes the side effect type (appetite suppression, insomnia, irritability, headache), the severity rating (mild, moderate, severe), and the functional impact description.

## Design Principles

**Immutability ensures reliability.** A Rating Scale Score, once recorded, never changes. If a score is corrected, a new value object is created. This prevents accidental modification of clinical data.

**Value equality simplifies comparison.** Two Dosage Amounts of "10 mg extended-release methylphenidate" are interchangeable regardless of when or where they were created. This simplifies medication history analysis and prescription comparison.

**Self-validation enforces domain rules.** Value objects validate their own construction. A Rating Scale Score rejects invalid raw scores, a Dosage Amount rejects negative values, and an ADHD Presentation Type rejects unrecognized classifications.

## References

- Evans, E. (2003). *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media.
