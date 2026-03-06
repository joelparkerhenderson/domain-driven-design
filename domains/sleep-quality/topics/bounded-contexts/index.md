# Bounded Contexts in the Sleep Quality Domain

## Overview

Bounded contexts define the logical boundaries within the sleep quality domain where specific models and terminology maintain internal consistency. Each bounded context has its own domain model, its own ubiquitous language refinements, and clear interfaces for communicating with other contexts. The sleep quality domain contains six bounded contexts, each reflecting a distinct area of clinical, behavioral, or technological responsibility.

## Sleep Assessment Context

This context owns the model for evaluating sleep quality through standardized instruments and clinical measurements. It manages sleep diary entries, questionnaire administration and scoring (PSQI, ESS, ISI), actigraphy data interpretation, and polysomnography study coordination. The primary aggregates are the Sleep Assessment (grouping multiple measurement instruments for a single evaluation period) and the Sleep Diary (a longitudinal self-report record). Assessment results are published as domain events consumed by downstream contexts.

## Sleep Disorders Context

This context manages the clinical classification, diagnosis, and tracking of sleep disorders according to the ICSD-3 taxonomy. It models insomnia disorder (with subtypes for sleep onset, sleep maintenance, and early morning awakening), obstructive sleep apnea (with severity classifications based on AHI), restless legs syndrome, narcolepsy (types 1 and 2), and parasomnias (NREM and REM subtypes). The primary aggregate is the Disorder Diagnosis, which maintains diagnostic criteria fulfillment and clinical status.

## Sleep Environment Context

This context models the physical environment in which sleep occurs and provides optimization guidance. It captures bedroom assessments covering light levels (lux measurements), ambient noise (decibel readings), temperature (degrees Celsius/Fahrenheit), humidity, air quality, and bedding characteristics. The primary aggregate is the Environment Assessment, which groups environmental readings with evidence-based recommendations for improvement.

## Behavioral Interventions Context

This context captures structured treatment protocols for sleep disorders, with emphasis on evidence-based behavioral approaches. It models CBT-I programs (typically 6-8 sessions), sleep restriction therapy protocols, stimulus control instruction sets, sleep hygiene education plans, and relaxation technique sequences. The primary aggregate is the Intervention Protocol, which tracks treatment components, session scheduling, homework assignments, and adherence metrics.

## Device Integration Context

This context handles data ingestion and normalization from external sleep-related devices. It manages device registrations, data feed configurations, raw data ingestion, and translation into domain-standard formats. Supported device types include consumer wearables (fitness trackers, smart watches), smart mattresses and pillows, CPAP/BiPAP therapy machines, and ambient environment sensors. The primary aggregate is the Device Feed, which maintains the mapping between device-specific data formats and canonical domain representations.

## Outcomes Tracking Context

This context tracks longitudinal sleep quality outcomes and supports reporting for clinical decision-making. It manages outcome records that aggregate sleep efficiency trends, composite quality scores, daytime functioning assessments (including cognitive performance, mood, and energy levels), and patient satisfaction measures. The primary aggregate is the Outcomes Record, which maintains a temporal series of quality measurements for an individual and computes trend analytics.

## Boundary Enforcement

Each bounded context enforces its boundaries through well-defined interfaces. Contexts do not share database tables, aggregate instances, or internal domain objects. Communication between contexts occurs exclusively through domain events, published language contracts, or explicit integration APIs. This boundary enforcement ensures that changes within one context do not create ripple effects in others.

## Language Variations

While the ubiquitous language provides a shared foundation, certain terms carry context-specific meanings. "Score" in the Assessment Context refers to a validated instrument result (e.g., PSQI global score), while in the Outcomes Tracking Context it refers to a composite metric computed from multiple sources. "Session" in the Behavioral Interventions Context refers to a therapy appointment, while in the Device Integration Context it refers to a data collection period.

## Ownership and Teams

Each bounded context should ideally be owned by a team with the relevant domain expertise. The Assessment and Disorders contexts require clinical sleep medicine knowledge. The Behavioral Interventions context requires behavioral health expertise. The Device Integration context requires embedded systems and data engineering skills. The Environment and Outcomes contexts benefit from interdisciplinary collaboration.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley. Chapter 14 on maintaining model integrity.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley. Chapter 2 on bounded contexts.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media. Chapter 6 on bounded contexts.
- Buysse, D.J. et al. (1989). The Pittsburgh Sleep Quality Index. Psychiatry Research, 28(2), 193-213.
