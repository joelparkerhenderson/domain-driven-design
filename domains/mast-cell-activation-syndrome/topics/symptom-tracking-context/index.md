# Symptom Tracking Context for Mast Cell Activation Syndrome

## Overview

The Symptom Tracking Context is a supporting bounded context responsible for capturing the patient's symptom experience across multiple organ systems. MCAS is characterized by symptoms that span dermatological, gastrointestinal, cardiovascular, neurological, respiratory, and musculoskeletal systems, often simultaneously. This context manages symptom logging, flare documentation, severity scoring, and pattern analysis.

The Symptom Tracking Context is a prolific publisher of domain events, serving as a primary data source for the Trigger Management, Medication Protocol, and Outcomes Measurement contexts. Its ubiquitous language centers on terms such as symptom entry, flare record, severity score, organ system, baseline symptoms, and symptom pattern.

## Key Aggregates

### Symptom Log

The Symptom Log is the primary aggregate root. It represents a time-bounded collection of symptom entries for a patient. The log manages the addition of new entries, ensuring each contains the required severity score, organ system classification, and timestamp. The aggregate enforces chronological ordering and validates that entries use the domain's standardized symptom vocabulary.

Given the potentially high volume of symptom data, the Symptom Log aggregate uses time-bounded boundaries. Each log covers a defined period, and new logs are created as periods advance. This design prevents any single aggregate from growing unbounded while maintaining consistency within each period.

### Flare Record

The Flare Record is a separate aggregate documenting acute exacerbation episodes. Each flare record captures the onset time, duration, peak severity, affected organ systems, suspected triggers, interventions applied, and resolution details. The aggregate enforces the invariant that a flare must have an onset time and at least one documented symptom before finalization.

Flare records provide richer context than individual symptom entries. They capture the episode as a coherent clinical event, including the relationship between symptoms, the temporal progression, and the clinical response.

## Value Objects

The Severity Score value object represents the intensity rating assigned to a symptom on a defined scale. It contains the numeric value, the scale definition (such as 0-10 numeric rating or mild/moderate/severe categorical), and the assessor type (patient-reported or clinician-assessed). The value object validates that scores fall within the defined scale range.

The Organ System value object classifies symptoms by affected body system. The six primary categories are dermatological, gastrointestinal, cardiovascular, neurological, respiratory, and musculoskeletal. Each category encompasses specific symptom types relevant to MCAS presentation.

The Symptom Duration value object captures the temporal extent of a symptom episode, including start time, end time, and calculated duration. It supports comparison and aggregation operations for pattern analysis.

## Multi-System Symptom Categories

### Dermatological Symptoms

Skin manifestations include urticaria (hives), angioedema, flushing, pruritus (itching), dermatographism, and rashes. The context tracks the distribution, duration, and severity of skin symptoms. Flushing episodes are documented with location, duration, and associated systemic symptoms.

### Gastrointestinal Symptoms

GI symptoms include abdominal pain, nausea, vomiting, diarrhea, bloating, acid reflux, and food intolerances. The context tracks meal associations, stool patterns, and the relationship between GI symptoms and dietary triggers.

### Cardiovascular Symptoms

Cardiovascular manifestations include tachycardia, blood pressure fluctuations, presyncope, syncope, and chest pain. These symptoms require documentation of vital signs and temporal relationship to other mast cell activation symptoms.

### Neurological Symptoms

Neurological symptoms include headaches, brain fog, cognitive difficulties, dizziness, neuropathic pain, and anxiety. The context tracks cognitive function assessments and the impact on daily activities.

### Respiratory Symptoms

Respiratory symptoms include nasal congestion, throat tightness, wheezing, shortness of breath, and cough. The context documents the relationship between respiratory symptoms and environmental or exercise triggers.

### Musculoskeletal Symptoms

Musculoskeletal manifestations include joint pain, muscle pain, weakness, and fatigue. The context tracks the distribution and functional impact of these symptoms.

## Domain Events

SymptomRecorded is published for each new symptom entry. Given the high frequency of this event, downstream consumers must handle volume efficiently. FlareDocumented is published when a flare episode is recorded, carrying comprehensive episode data. FlareResolved is published when a flare episode ends, triggering duration and pattern analysis.

## Domain Services

The PatternAnalysisService examines longitudinal symptom data to identify temporal cycles, organ system clustering, and trend analysis. It produces pattern reports that inform trigger identification and treatment adjustment decisions.

The SeverityScoringService computes aggregate severity scores from individual entries, weighting symptoms by their frequency, intensity, duration, and functional impact.

## Patient-Reported Data

The Symptom Tracking Context supports both patient-reported and clinician-documented data. Patient-reported symptoms are captured through structured entries that use the domain's standardized vocabulary while allowing free-text notes for additional context. The context distinguishes between assessor types in its data model to support appropriate interpretation.

## Baseline Symptom Tracking

Many MCAS patients experience chronic low-level symptoms in addition to acute flares. The context supports baseline symptom documentation that captures the patient's typical symptom state between flares. This baseline data is essential for the Outcomes Measurement Context, which evaluates treatment effectiveness against the patient's personal baseline rather than symptom-free states.

## Longitudinal Analysis

The context maintains symptom data over extended periods, supporting longitudinal analysis of symptom patterns, treatment responses, and disease progression. Time-series data enables the identification of seasonal patterns, hormonal cycle correlations, and long-term trends that inform clinical decision-making.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- Afrin, L.B. Never Bet Against Occam: Mast Cell Activation Disease and the Modern Epidemics of Chronic Illness and Medical Complexity. Sisters Media, 2016.
