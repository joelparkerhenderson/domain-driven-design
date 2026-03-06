# Value Objects in the Sleep Quality Domain

## Overview

Value objects are immutable domain objects defined entirely by their attributes rather than by a unique identity. Two value objects with the same attributes are considered equal. In the sleep quality domain, value objects represent measurements, scores, time ranges, environmental readings, and other concepts that are naturally described by their values rather than tracked by identity.

## Sleep Metric Value Objects

### SleepEfficiency

SleepEfficiency is a value object representing the percentage of time in bed spent asleep. It contains a single decimal value between 0.0 and 100.0. The value object enforces the invariant that sleep efficiency cannot be negative or exceed 100 percent. It provides comparison operations for clinical thresholds: values above 85 percent are generally considered normal, values between 75 and 85 percent indicate moderate impairment, and values below 75 percent indicate significant impairment.

### SleepOnsetLatency

SleepOnsetLatency is a value object representing the time to fall asleep, measured in minutes. It contains a non-negative duration value. Clinical interpretation is embedded in the value object: latency under 15 minutes is considered normal, 15 to 30 minutes is mildly prolonged, and greater than 30 minutes is clinically significant.

### TotalSleepTime

TotalSleepTime is a value object representing the cumulative duration of sleep in a recording period. It contains a non-negative duration value and supports comparison against age-appropriate recommended sleep duration ranges.

### WakeAfterSleepOnset

WASO is a value object representing the total minutes of wakefulness after initial sleep onset. It contains a non-negative duration value with clinical threshold support.

## Instrument Score Value Objects

### PSQIResult

PSQIResult is a value object containing seven component scores (subjective sleep quality, sleep latency, sleep duration, habitual sleep efficiency, sleep disturbances, use of sleeping medication, daytime dysfunction) each ranging from 0 to 3, and a computed global score ranging from 0 to 21. The value object enforces that the global score equals the sum of component scores and that each component score is within valid range. A global score greater than 5 is classified as poor sleep quality.

### ESSResult

ESSResult is a value object containing eight situation scores (each 0 to 3) and a computed total score (0 to 24). The value object enforces scoring rules and classifies results: 0-10 normal, 11-14 mild excessive daytime sleepiness, 15-17 moderate, 18-24 severe.

### AHIValue

AHIValue is a value object representing the Apnea-Hypopnea Index as events per hour of sleep. It contains a non-negative decimal value and classifies severity: less than 5 is normal, 5-14 is mild, 15-29 is moderate, and 30 or greater is severe obstructive sleep apnea.

## Time and Duration Value Objects

### SleepPeriod

SleepPeriod is a value object representing the time window from bedtime to rise time. It contains a start timestamp and end timestamp and computes the total time in bed. The value object enforces that end is after start and that the duration does not exceed 24 hours.

### TimeInBedWindow

TimeInBedWindow is a value object used in sleep restriction therapy, representing the prescribed time window for being in bed. It contains a bedtime and a rise time, with the constraint that the window duration must be at least 5 hours (the minimum safe sleep restriction). It is used by the Behavioral Interventions Context to define and adjust prescribed sleep schedules.

## Environmental Reading Value Objects

### LightLevel

LightLevel is a value object representing ambient light measurement in lux. It classifies readings against sleep-appropriate thresholds: below 5 lux is suitable for sleep, 5-50 lux may impair melatonin production, and above 50 lux is likely to disrupt sleep.

### NoiseLevel

NoiseLevel is a value object representing ambient noise measurement in decibels. It classifies readings: below 30 dB is quiet, 30-50 dB may cause fragmentation in light sleepers, and above 50 dB is likely to disrupt sleep.

### TemperatureReading

TemperatureReading is a value object representing ambient temperature. It stores the value with explicit unit (Celsius or Fahrenheit) and supports conversion between units. Optimal sleep temperature is classified as 16-19 degrees Celsius (60-67 degrees Fahrenheit).

## Clinical Value Objects

### SeverityClassification

SeverityClassification is a value object representing disorder severity as an enumeration: mild, moderate, or severe. Different disorders define severity based on different metrics (AHI for OSA, ISI score for insomnia), but the classification itself is a standardized value object.

### DiagnosticCriteria

DiagnosticCriteria is a value object representing a set of clinical criteria for a disorder diagnosis, with indicators for which criteria have been met. The value object computes whether the minimum threshold for diagnosis is satisfied.

## Design Principles

Value objects in the sleep quality domain are always immutable. Once created, a value object cannot be modified; any change produces a new value object. This immutability simplifies reasoning about state, enables safe sharing between aggregates, and supports caching. Value objects encapsulate validation logic, ensuring that invalid states (negative sleep efficiency, AHI below zero) cannot be represented.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley. Chapter 5 on value objects.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley. Chapter 6 on value objects.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media. Chapter 10 on value objects.
- Buysse, D.J. et al. (1989). The Pittsburgh Sleep Quality Index. Psychiatry Research, 28(2), 193-213.
