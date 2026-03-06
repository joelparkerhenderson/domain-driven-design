# Trigger Management Context for Mast Cell Activation Syndrome

## Overview

The Trigger Management Context is a supporting bounded context responsible for identifying, categorizing, and mitigating factors that provoke mast cell degranulation. Trigger management is a continuous process that extends throughout a patient's lifetime with MCAS. This context owns the trigger identification workflow, avoidance strategy design, environmental control planning, and dietary management protocols.

The context has its own ubiquitous language centered on exposure-reaction relationships. Terms such as trigger profile, avoidance plan, reaction window, environmental control, and dietary restriction carry specific meanings within this boundary that may differ from their use in other clinical contexts.

## Key Aggregates

### Trigger Profile

The Trigger Profile is the primary aggregate root. It represents the comprehensive collection of identified triggers for an individual patient. Each patient has exactly one Trigger Profile that evolves over time as new triggers are discovered and existing triggers are reclassified.

The Trigger Profile contains Trigger Entry entities, each representing a single identified trigger with its classification, evidence basis, confidence level, and associated avoidance strategy. The aggregate enforces the invariant that each trigger must have documented exposure-reaction correlation evidence before being classified as confirmed.

### Avoidance Plan

The Avoidance Plan is a separate aggregate that translates trigger identification into actionable mitigation strategies. It groups environmental controls and dietary restrictions into a coherent management plan. The aggregate enforces the invariant that avoidance recommendations must not conflict with one another and that the overall plan addresses all confirmed triggers.

## Value Objects

The Trigger Classification value object categorizes triggers by type: environmental (fragrances, chemicals, temperature extremes, humidity, air quality), dietary (high-histamine foods, alcohol, fermented foods, additives, preservatives), pharmacological (NSAIDs, opioids, contrast dyes, certain antibiotics), physical (exercise, vibration, pressure, heat, cold), and emotional (acute stress, anxiety, sleep deprivation).

The Confidence Level value object represents the certainty of trigger identification. It progresses from suspected (single exposure-reaction correlation) through probable (multiple consistent correlations) to confirmed (reproducible correlation with elimination and rechallenge evidence).

The Reaction Window value object captures the temporal relationship between exposure and symptom onset, including minimum latency, maximum latency, and typical onset time. Different trigger types have characteristic reaction windows that inform the correlation analysis.

## Domain Events

TriggerIdentified is published when a new trigger reaches confirmed status. This event informs the Medication Protocol Context, which may adjust prophylactic medication, and the Specialist Coordination Context, which updates the shared care plan.

AvoidancePlanUpdated is published when the avoidance plan is modified. The Specialist Coordination Context updates the shared care plan to reflect new avoidance recommendations, and the Outcomes Measurement Context records the plan change as a management milestone.

## Domain Services

The TriggerCorrelationService analyzes the temporal relationship between exposure events and symptom flares. It accepts symptom data from the Symptom Tracking Context and exposure records from the Trigger Management Context, then evaluates whether the timing, frequency, and consistency of correlations meet the threshold for trigger identification.

The AvoidancePlanValidationService reviews proposed avoidance plans for internal consistency, completeness, and nutritional adequacy of dietary restrictions.

## Trigger Categories

### Environmental Triggers

Environmental triggers include airborne chemicals, fragrances, cleaning products, volatile organic compounds, tobacco smoke, and air pollution. Temperature extremes, both heat and cold, are common triggers. Humidity changes, barometric pressure shifts, and altitude changes may also provoke mast cell activation. The context model tracks environmental conditions associated with each trigger event.

### Dietary Triggers

Dietary management in MCAS involves identifying foods that provoke symptoms. High-histamine foods including aged cheeses, fermented foods, cured meats, and leftover proteins are common triggers. Histamine-releasing foods such as citrus, strawberries, tomatoes, and shellfish may also provoke reactions. Alcohol, food additives, artificial colorings, and preservatives are additional categories. The context supports both elimination diet tracking and individual food challenge documentation.

### Pharmacological Triggers

Certain medications can trigger mast cell degranulation. NSAIDs, opioid analgesics, muscle relaxants, and contrast dyes are well-documented triggers. The context maintains a pharmacological trigger database and cross-references it with the Medication Protocol Context to prevent prescribing known triggers.

### Physical and Emotional Triggers

Physical triggers include exercise (particularly sustained aerobic activity), vibration, mechanical pressure, and temperature changes. Emotional triggers include acute stress responses and anxiety. The context tracks these triggers with their specific circumstances and intensities.

## Integration with Symptom Tracking

The Trigger Management Context is a downstream consumer of the Symptom Tracking Context. It subscribes to SymptomRecorded and FlareDocumented events to correlate symptom occurrences with documented exposures. This integration is governed by a published language that defines standardized symptom and exposure event formats.

## Personalization

MCAS trigger profiles are highly individualized. What provokes a reaction in one patient may be well-tolerated by another. The context model supports this variability through patient-specific trigger profiles rather than population-level trigger databases. Avoidance plans are tailored to each patient's confirmed triggers, lifestyle, and tolerance thresholds.

## Longitudinal Management

Trigger management is not static. New triggers may emerge over time, while tolerance to previously identified triggers may improve with treatment. The context model supports temporal tracking of trigger status, allowing triggers to be reclassified as tolerance changes. The Trigger Profile maintains a complete history of trigger identification and status changes.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- Afrin, L.B. Never Bet Against Occam: Mast Cell Activation Disease and the Modern Epidemics of Chronic Illness and Medical Complexity. Sisters Media, 2016.
- Weinstock, L.B. et al. "Mast Cell Activation Syndrome: A Primer for the Gastroenterologist." Digestive Diseases and Sciences, 2021.
