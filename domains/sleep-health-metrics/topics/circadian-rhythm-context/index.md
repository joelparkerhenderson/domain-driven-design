# Circadian Rhythm Context - Sleep Health Metrics

## Overview

The Circadian Rhythm Context models the approximately 24-hour biological clock that governs sleep-wake timing, hormone secretion, body temperature regulation, and alertness fluctuations. This context tracks circadian phase markers, assesses chronotype, monitors light exposure patterns, and quantifies the impact of circadian misalignment on sleep quality. It operates as a partner context to the Sleep Quality Assessment Context, enriching quality metrics with circadian phase information.

## Scope and Boundaries

This context owns the modeling of circadian biology as it relates to sleep health. It encompasses chronotype assessment, dim light melatonin onset (DLMO) tracking, light exposure dosimetry, core body temperature rhythm estimation, social jet lag quantification, and shift work schedule impact analysis. It does not own sleep staging (Sleep Stage Analysis Context), quality metric computation (Sleep Quality Assessment Context), or therapeutic intervention management (Intervention Tracking Context), though it provides data that informs all of these.

## Circadian Phase Markers

### Dim Light Melatonin Onset (DLMO)

DLMO is the most reliable and widely used marker of circadian phase. It represents the clock time at which melatonin concentration rises above a defined threshold (typically 3 pg/mL in saliva or 10 pg/mL in plasma) under dim light conditions (less than 30 lux). DLMO typically occurs 2-3 hours before habitual sleep onset in entrained individuals. The context models DLMO measurements including collection method (saliva or plasma), sampling protocol (interval and duration), threshold applied, and computed onset time.

### Core Body Temperature Minimum

The nadir of the core body temperature rhythm occurs approximately 2-3 hours before habitual wake time in normally entrained individuals. While less practical to measure than DLMO, it provides a complementary circadian phase marker. The context can estimate temperature nadir from wearable skin temperature data using validated algorithms.

## Chronotype Assessment

Chronotype represents an individual's innate preference for sleep-wake timing. The context models chronotype using validated instruments:

- **Morningness-Eveningness Questionnaire (MEQ)**: A 19-item questionnaire yielding scores from 16 to 86, classified as definitely evening type (16-30), moderately evening type (31-41), intermediate type (42-58), moderately morning type (59-69), or definitely morning type (70-86).
- **Munich Chronotype Questionnaire (MCTQ)**: Assesses chronotype based on midpoint of sleep on free days (corrected for sleep debt), providing a continuous measure of circadian preference.

Chronotype informs optimal treatment timing for interventions and helps explain discrepancies between prescribed and actual sleep schedules.

## Light Exposure Tracking

Light is the primary zeitgeber (time-giver) for the human circadian system. The context tracks light exposure patterns that influence circadian entrainment:

- **Exposure timing**: Morning light advances circadian phase; evening light delays it.
- **Intensity**: Brighter light produces stronger phase-shifting effects, with melanopic sensitivity peaking around 480 nm (blue light).
- **Duration**: Longer exposure produces greater phase shifts, with diminishing returns beyond certain durations.
- **Spectral composition**: Melanopsin-containing intrinsically photosensitive retinal ganglion cells (ipRGCs) are maximally sensitive to short-wavelength blue light.

The context models light exposure sessions with timestamped lux measurements and, where available, melanopic equivalent daylight illuminance (melanopic EDI) for spectral characterization.

## Social Jet Lag

Social jet lag quantifies the misalignment between an individual's biological clock and socially imposed schedule. It is computed as the absolute difference between midpoint of sleep on work days and midpoint of sleep on free days. Social jet lag exceeding 2 hours is associated with increased cardiometabolic risk, mood disturbance, and reduced cognitive performance.

## Shift Work Impact

The context models the circadian disruption associated with shift work schedules. Rotating and night shift schedules force sleep at times that conflict with the circadian alerting signal, leading to circadian misalignment, reduced sleep quality, and increased health risks. The context tracks shift schedules, estimates circadian phase under shift conditions, and quantifies the degree of misalignment between work schedule and biological night.

## Key Aggregates

The **CircadianProfile** aggregate captures an individual's circadian characteristics: chronotype classification, DLMO measurements, light exposure history, circadian phase estimates, and social jet lag calculations.

## Domain Events Published

- **CircadianPhaseAssessed**: Emitted when a circadian phase assessment is completed.
- **CircadianMisalignmentDetected**: Emitted when significant misalignment between circadian phase and sleep timing is identified.

## Partnership with Sleep Quality Assessment

The Circadian Rhythm Context and Sleep Quality Assessment Context operate as partners. Circadian phase data contextualizes quality metrics: poor sleep efficiency may be explained by attempted sleep at an adverse circadian phase. Conversely, sleep timing data from quality assessments informs circadian phase estimation. This bidirectional data exchange is mediated through domain events.

## References

- Evans, E. (2003). *Domain-Driven Design*. Addison-Wesley.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media.
- Lewy, A. J. et al. (1999). The Human Phase Response Curve to Melatonin Is About 12 Hours Out of Phase with the PRC to Light. *Chronobiology International*, 15(1), 71-83.
- Roenneberg, T. et al. (2004). A Marker for the End of Adolescence. *Current Biology*, 14(24), R1038-R1039.
- Wittmann, M. et al. (2006). Social Jetlag: Misalignment of Biological and Social Time. *Chronobiology International*, 23(1-2), 497-509.
