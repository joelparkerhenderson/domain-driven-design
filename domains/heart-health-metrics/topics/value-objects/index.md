# Value Objects

Value objects in the heart health metrics domain are immutable objects defined entirely by their attributes, with no concept of identity. They represent cardiac measurements, clinical parameters, and physiological quantities that are equal when their values are equal, regardless of which instance is referenced.

## Overview

Eric Evans defines a value object as an object that describes a characteristic of a thing but carries no concept of identity. In the heart health metrics domain, value objects are the natural representation for cardiac measurements, vital sign readings, waveform components, and clinical thresholds. A blood pressure reading of 120/80 mmHg is a value object: any two readings with the same systolic, diastolic, and unit values are interchangeable. There is no need to track which specific instance of "120/80 mmHg" is being referenced.

Value objects are central to cardiac health modeling because the domain is measurement-intensive. Heart rate readings, ECG waveform features, HRV metrics, risk scores, and clinical thresholds are all naturally expressed as immutable values with equality based on their attributes.

## Key Concepts

**Heart Rate Reading.** An immutable value representing a single heart rate measurement. Attributes include beats per minute (bpm), measurement timestamp, measurement method (PPG, ECG-derived, palpation), and signal quality indicator. Two readings with identical attributes are considered equal.

**Blood Pressure Reading.** An immutable value representing a single blood pressure measurement. Attributes include systolic pressure, diastolic pressure, mean arterial pressure, unit (mmHg), measurement position (sitting, standing, supine), cuff site (left arm, right arm), and device type. Includes validation rules enforcing that systolic exceeds diastolic.

**QRS Complex.** An immutable value representing the ventricular depolarization waveform extracted from an ECG. Attributes include duration (milliseconds), amplitude (millivolts), morphology classification (normal, left bundle branch block, right bundle branch block), and axis deviation. Used by the Cardiac Analysis Context for rhythm classification.

**R-R Interval.** An immutable value representing the time between consecutive R peaks in the ECG. Attributes include duration (milliseconds) and the indices of the bounding R peaks. Collections of R-R Intervals form the basis for HRV computation.

**HRV Metrics.** An immutable value object containing computed heart rate variability statistics: SDNN (standard deviation of N-N intervals), RMSSD (root mean square of successive differences), pNN50 (percentage of successive intervals differing by more than 50 ms), and frequency-domain measures (LF power, HF power, LF/HF ratio).

**Risk Score.** An immutable value representing a computed cardiovascular risk assessment. Attributes include the scoring model identifier (Framingham, ASCVD), the computed risk percentage, the risk category (low, borderline, intermediate, high), and the input parameter snapshot used for the calculation.

**Clinical Threshold.** An immutable value defining a boundary for normal, elevated, or critical classification of a cardiac metric. Attributes include the metric type, the threshold value, the comparison operator, and the severity level.

## Domain Examples

When a wearable device reports a heart rate of 72 bpm measured via PPG at a given timestamp, the system creates a Heart Rate Reading value object. If another device reports the same values, the two value objects are equal. Neither has an identity separate from its attribute values.

When the Cardiac Analysis Context computes HRV from a series of R-R Intervals, it produces an HRV Metrics value object. This value object is passed to the Risk Assessment Context as an input parameter for cardiovascular risk scoring, and to the Reporting Context for inclusion in trend analysis. The value object is freely shared because it is immutable and has no side effects.

## Relationships to Other Topics

- [Entities](../entities/index.md) - Entities contain value objects as attributes.
- [Aggregates](../aggregates/index.md) - Aggregates enforce consistency of value objects within their boundary.
- [Domain Events](../domain-events/index.md) - Domain events carry value objects as payload data.
- [Ubiquitous Language](../ubiquitous-language/index.md) - Value object names reflect ubiquitous language terms.
- [Heart Health Metrics Standards](../heart-health-metrics-standards/index.md) - Standards define units and ranges for value objects.
- [Cardiac Analysis Context](../cardiac-analysis-context/index.md) - Primary producer of derived cardiac value objects.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 5 on value objects.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 6 on value objects.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly, 2021. Chapter 8 on value objects.
- Task Force of the European Society of Cardiology. "Heart Rate Variability: Standards of Measurement." *Circulation*, 93(5), 1043-1065, 1996.
- Whelton, P.K. et al. "2017 ACC/AHA Guideline for the Prevention, Detection, Evaluation, and Management of High Blood Pressure." *Hypertension*, 71(6), e13-e115, 2018.
