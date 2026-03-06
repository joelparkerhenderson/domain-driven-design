# Value Objects in the Psychiatry Domain

## Overview

A value object is an immutable domain object defined entirely by its attributes rather than by a unique identity. Two value objects with the same attributes are considered equal and interchangeable. In the psychiatry domain, value objects represent clinical measurements, codes, scores, dosages, and other data that derives its meaning from what it is rather than which specific instance it is. Value objects are fundamental to modeling psychiatric data because much of clinical information consists of standardized codes, validated scores, and structured measurements.

## Characteristics

### Immutability

Once created, a value object cannot be modified. If a medication dosage needs to change, a new Dosage value object is created rather than modifying the existing one. This immutability simplifies reasoning about clinical data and prevents accidental corruption of historical records. A PHQ-9 score recorded on a specific date should never be altered; if a scoring error is discovered, a new corrected score is recorded alongside the original.

### Structural Equality

Value objects are compared by their attributes, not by identity. Two Diagnosis Code value objects representing "F32.1 - Major Depressive Disorder, Single Episode, Moderate" are equal regardless of when or where they were created. This equality semantics aligns with clinical reasoning: clinicians care about what the diagnosis is, not which instance of the code was used.

### Self-Validation

Value objects validate their own construction. A Dosage value object rejects creation with a negative quantity. A PHQ-9 Score value object rejects creation with a value outside the 0-27 range. This self-validation ensures that invalid clinical data cannot exist in the domain model.

## Key Value Objects by Bounded Context

### Diagnostic Assessment Context

**Diagnosis Code**
- Attributes: coding system (DSM-5-TR, ICD-10-CM, ICD-11), code value, display name
- Validation: Code must exist in the referenced coding system. DSM-5-TR codes must include required specifiers.
- Example: DSM-5-TR F32.1 "Major Depressive Disorder, Single Episode, Moderate"

**Diagnostic Specifier**
- Attributes: specifier type (severity, course, features), specifier value
- Validation: Specifier must be valid for the associated diagnosis code.
- Example: Severity specifier "Moderate" for Major Depressive Disorder

**Rating Scale Score**
- Attributes: instrument name, total score, subscale scores, severity interpretation, administration date
- Validation: Total score must be within instrument range. Subscale scores must sum correctly where applicable.
- Example: PHQ-9 total score 15 (moderately severe depression)

**Mental Status Finding**
- Attributes: domain (appearance, behavior, mood, affect, thought process, thought content, cognition, insight, judgment), finding description, clinical significance
- Validation: Domain must be a recognized mental status examination category.

### Medication Management Context

**Dosage**
- Attributes: quantity, unit (mg, mcg, mL), frequency, route of administration
- Validation: Quantity must be positive. Unit must be valid for the medication form. Frequency must be a recognized dosing schedule.
- Example: 20 mg oral once daily

**Drug Interaction**
- Attributes: interacting drug pair, interaction severity (contraindicated, major, moderate, minor), mechanism description, clinical recommendation
- Validation: Severity must be from the defined severity scale. Both drugs must be identifiable.

**Therapeutic Range**
- Attributes: medication name, lower bound, upper bound, unit, clinical context
- Validation: Lower bound must be less than upper bound. Unit must match the laboratory measurement standard.
- Example: Lithium therapeutic range 0.6-1.2 mEq/L for maintenance therapy

**Metabolizer Status**
- Attributes: gene, phenotype (poor, intermediate, normal, rapid, ultra-rapid), clinical implication
- Validation: Gene must be a recognized pharmacogenomic gene. Phenotype must be from the standardized classification.
- Example: CYP2D6 Poor Metabolizer

### Psychotherapy Context

**Session Duration**
- Attributes: minutes, billing category (16-37 min, 38-52 min, 53+ min)
- Validation: Duration must be positive. Billing category must match the duration range.

**Treatment Goal**
- Attributes: goal description, target behavior, baseline measure, target measure, target date
- Validation: Target measure must represent an improvement over baseline. Target date must be in the future at time of creation.

**Homework Assignment**
- Attributes: assignment description, therapeutic rationale, due date, modality alignment
- Validation: Due date must be before the next scheduled session.

### Crisis Management Context

**Risk Level**
- Attributes: risk category (imminent, acute, chronic, low), assessment basis, time horizon
- Validation: Risk category must be from the defined scale. Assessment basis must reference a structured assessment tool.

**Safety Plan Step**
- Attributes: step number, step category (warning signs, coping strategies, social contacts, professional contacts, means restriction), step content
- Validation: Step number must be sequential. Step category must be from the defined categories.

**Involuntary Hold Type**
- Attributes: hold statute (e.g., 5150, 5250), jurisdiction, maximum duration, required criteria
- Validation: Hold statute must be valid for the specified jurisdiction.

### Rehabilitation Services Context

**Functional Score**
- Attributes: assessment instrument, domain (social, occupational, self-care, cognitive), score, interpretation
- Validation: Score must be within instrument range. Domain must be recognized by the instrument.

**Service Category**
- Attributes: category name, description, evidence base reference
- Validation: Category must be from the defined rehabilitation service taxonomy.

### Outcomes Measurement Context

**Change Score**
- Attributes: instrument, baseline score, follow-up score, raw change, reliable change index, clinical significance classification (recovered, improved, unchanged, deteriorated)
- Validation: Clinical significance must be determined by the Jacobson-Truax method or specified alternative.

**Quality Indicator**
- Attributes: indicator name, numerator definition, denominator definition, benchmark value, reporting period
- Validation: Numerator must be a subset of denominator. Benchmark must be from an authoritative source.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 5 on value objects.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 6 on value objects.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 4 on value objects.
