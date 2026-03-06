# Sleep Environment Context

## Overview

The Sleep Environment Context models the physical conditions of the sleeping space and provides evidence-based optimization guidance. Environmental factors including light, noise, temperature, humidity, air quality, and bedding characteristics significantly influence sleep quality. This context captures environmental assessments, evaluates conditions against research-based thresholds, and generates prioritized recommendations for improvement. It bridges the gap between measurable environmental conditions and actionable changes that improve sleep outcomes.

## Core Responsibilities

This context owns the domain logic for environmental factor assessment and optimization. It manages the lifecycle of environment assessments from initial measurement through recommendation generation and follow-up verification. It maintains evidence-based thresholds for optimal sleep conditions. It classifies environmental conditions by their impact severity. It generates prioritized, actionable recommendations. It publishes assessment results for integration with behavioral sleep hygiene interventions.

## Environmental Factors

### Light Exposure

Light is the most potent environmental cue for the circadian system. The context models light levels in lux across different times of day and night. Nighttime bedroom light should ideally be below 5 lux to avoid suppressing melatonin production. The context distinguishes between ambient light (room illumination), direct light (light sources visible from the bed), and screen light (electronic device emissions). It models light spectrum characteristics, as blue-enriched light (460-480 nm) has the greatest impact on melatonin suppression.

### Ambient Noise

Noise disrupts sleep by causing arousals, stage shifts, and increased sleep fragmentation, even when it does not cause full awakenings. The context models noise levels in decibels (dB) and distinguishes between continuous background noise and intermittent noise events. Background noise below 30 dB is ideal for sleep. The context models noise type (traffic, aircraft, household, environmental) and temporal patterns (steady, intermittent, rhythmic), as these characteristics affect the degree of sleep disruption.

### Temperature

Room temperature directly affects core body temperature regulation, which is essential for sleep initiation and maintenance. The context models ambient temperature in both Celsius and Fahrenheit. The optimal range for sleep is 16-19 degrees Celsius (60-67 degrees Fahrenheit). The context also considers bedding thermal properties, personal thermal preferences, and seasonal variations. Humidity interacts with temperature to affect thermal comfort; the context models relative humidity with an optimal range of 30-60 percent.

### Air Quality

Indoor air quality affects breathing comfort and can exacerbate respiratory-related sleep disorders. The context models particulate matter levels, volatile organic compound (VOC) concentrations, carbon dioxide levels, and allergen presence. These factors are particularly relevant for patients with OSA or allergic rhinitis that affects nasal breathing during sleep.

### Bedding Characteristics

Mattress, pillow, and bedding characteristics affect sleep through their impact on spinal alignment, pressure distribution, and thermal regulation. The context models mattress type, firmness level, age, and condition. It models pillow type and loft relative to sleeping position. It captures bedding material and thermal properties. While individual preferences vary significantly, the context maintains evidence-based guidelines for bedding selection based on body type, sleeping position, and clinical conditions.

## Key Aggregates

### Environment Assessment

The primary aggregate groups environmental readings, analysis results, and recommendations for a specific sleeping space. The root entity tracks the location, assessment date, assessor (self-report or professional), and overall environmental quality classification. It contains EnvironmentalReading value objects for each measured factor and Recommendation value objects with priority rankings and implementation guidance.

## Value Objects

EnvironmentalReading captures a single measurement with its value, unit, timestamp, measurement method (sensor, estimate, professional measurement), and classification against evidence-based thresholds (optimal, acceptable, suboptimal, poor). Recommendation captures an actionable suggestion with its target factor, expected impact, implementation difficulty, cost estimate, and verification method.

## Domain Events Published

The context publishes EnvironmentAssessmentCompleted when an assessment produces findings and recommendations. This event carries the assessment summary and key findings, enabling the Behavioral Interventions Context to incorporate environmental recommendations into sleep hygiene education components.

## Relationships with Other Contexts

The Sleep Environment Context consumes normalized sensor data from the Device Integration Context in a conformist relationship for ambient sensor readings (light, noise, temperature, humidity). It shares a small kernel with the Behavioral Interventions Context around sleep hygiene concepts. It supplies environmental assessment results to the Outcomes Tracking Context for longitudinal tracking of environmental improvements and their correlation with sleep quality changes.

## Evidence-Based Thresholds

All environmental thresholds used by this context are derived from peer-reviewed research. Light thresholds reference Gooley et al. (2011) on melatonin suppression. Noise thresholds reference WHO Night Noise Guidelines for Europe (2009). Temperature thresholds reference Okamoto-Mizuno and Mizuno (2012) on thermal environment and sleep. These thresholds are modeled as configurable parameters to accommodate updates as new research emerges.

## Seasonal and Geographic Considerations

Environmental conditions vary with season, geography, and housing type. The context accounts for seasonal light variations (longer daylight hours in summer), geographic noise patterns (urban versus rural), and climate-related temperature and humidity variations. Assessment recommendations are contextualized for the patient's specific environment.

## Implementation Tracking

The context tracks whether generated recommendations have been implemented and provides follow-up assessment to verify improvement. This creates a feedback loop: initial assessment produces recommendations, implementation is tracked, and follow-up assessment measures the impact. This tracking supports the Outcomes Tracking Context in correlating environmental changes with sleep quality improvements.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media.
- Gooley, J.J. et al. (2011). Exposure to Room Light Before Bedtime Suppresses Melatonin Onset. Journal of Clinical Endocrinology and Metabolism, 96(3), E463-E472.
- Okamoto-Mizuno, K. and Mizuno, K. (2012). Effects of Thermal Environment on Sleep and Circadian Rhythm. Journal of Physiological Anthropology, 31(1), 14.
- World Health Organization. (2009). Night Noise Guidelines for Europe.
