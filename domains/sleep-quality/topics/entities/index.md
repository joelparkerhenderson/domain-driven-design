# Entities in the Sleep Quality Domain

## Overview

Entities are domain objects defined by a unique identity that persists over time, even as their attributes change. Unlike value objects, which are defined by their attributes, entities maintain continuity through an identity that remains stable across state transitions. In the sleep quality domain, entities represent the core clinical, behavioral, and technical objects that evolve throughout the patient's sleep quality journey.

## Sleep Assessment Entity

The SleepAssessment entity represents a single comprehensive evaluation of an individual's sleep quality at a point in time. It is identified by a unique assessment ID and tracks the assessment date, the administering clinician (if applicable), the set of instruments used, and the overall assessment status (scheduled, in-progress, completed, reviewed). The entity's attributes change as instruments are administered and scored, but its identity persists from scheduling through completion.

## Sleep Diary Entity

The SleepDiary entity represents an individual's ongoing sleep self-report record. It is identified by a unique diary ID linked to the individual. The diary accumulates entries over time, and its state evolves as new entries are added, entries are corrected, and aggregate statistics are recalculated. The entity maintains continuity across the entire duration of a sleep monitoring program, which may span weeks or months.

## Disorder Diagnosis Entity

The DisorderDiagnosis entity represents a clinical determination that an individual meets the criteria for a specific sleep disorder. It is identified by a unique diagnosis ID and tracks the disorder type (referencing the ICSD-3 classification), severity level, date of diagnosis, diagnosing clinician, criteria fulfillment status, and current clinical status (active, in remission, resolved). The entity evolves as the disorder progresses, treatment takes effect, or additional clinical information modifies the diagnosis.

## Intervention Protocol Entity

The InterventionProtocol entity represents a structured treatment plan assigned to an individual. It is identified by a unique protocol ID and tracks the intervention type (CBT-I, sleep restriction, stimulus control), start date, target duration, prescribed parameters (such as the initial time-in-bed window for sleep restriction), component list, and status (planned, active, paused, completed, discontinued). The entity evolves as sessions are completed, parameters are adjusted, and the protocol adapts to patient response.

## Intervention Session Entity

The InterventionSession entity represents a single treatment session within an intervention protocol. It is identified by a unique session ID and tracks the session number in sequence, scheduled date, actual date, session content topics, homework assigned, homework from the previous session reviewed, and completion status. Sessions evolve from scheduled to completed and may be rescheduled or cancelled.

## Device Feed Entity

The DeviceFeed entity represents the ongoing data relationship between an external device and the domain. It is identified by a unique feed ID and tracks the device type, manufacturer, model, firmware version, data format version, connection status (active, paused, disconnected), and last successful data ingestion timestamp. The entity evolves as the device connects, disconnects, updates firmware, and transmits data.

## Outcomes Record Entity

The OutcomesRecord entity represents the longitudinal collection of sleep quality outcome measurements for an individual. It is identified by a unique record ID linked to the individual. The entity accumulates measurements over time and maintains computed trend statistics. Its state changes with each new measurement, each recalculation of trends, and each clinical review milestone.

## Environment Assessment Entity

The EnvironmentAssessment entity represents an evaluation of a sleeping environment. It is identified by a unique assessment ID and tracks the location, assessment date, environmental readings, generated recommendations, and implementation status of those recommendations. The entity evolves as readings are updated, recommendations are generated, and follow-up assessments verify improvements.

## Patient Entity (Cross-Cutting)

While each bounded context maintains its own representation of the individual being served, the Patient entity concept appears across contexts. In the Assessment Context, it is the assessed individual. In the Disorders Context, it is the diagnosed patient. In the Interventions Context, it is the treatment participant. Each context maintains only the patient attributes relevant to its responsibilities, linked by a shared patient identifier.

## Identity Design

Entity identities in the sleep quality domain use universally unique identifiers (UUIDs) generated at creation time. These identifiers are opaque and carry no semantic meaning. External identifiers (medical record numbers, device serial numbers, insurance member IDs) are stored as attributes but never used as the entity's primary identity within the domain model.

## Lifecycle Management

Entities in the sleep quality domain have well-defined lifecycles. A SleepAssessment progresses from scheduled to in-progress to completed to reviewed. A DisorderDiagnosis moves from suspected to confirmed and eventually to resolved or chronic. An InterventionProtocol advances from planned to active to completed or discontinued. These lifecycle states are modeled explicitly and transitions are governed by domain rules.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley. Chapter 5 on entities.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley. Chapter 5 on entity design.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media. Chapter 10 on entities and value objects.
