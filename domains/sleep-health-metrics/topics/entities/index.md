# Entities - Sleep Health Metrics

## Overview

Entities are domain objects defined by a unique identity that persists over time, even as their attributes change. In the sleep health metrics domain, entities represent things that have a lifecycle, undergo state transitions, and must be tracked individually. A patient is an entity because they persist across multiple sleep studies; a sleep episode is an entity because it has a distinct identity even though its scored stage may be revised.

## Entity Design Principles

Entities in this domain carry a stable identifier (UUID or domain-specific ID) that distinguishes them from all other instances of the same type. Two sleep episodes recorded at the same time on different channels are distinct entities. An entity's identity is assigned at creation and never changes, while its attributes may be modified through domain operations. Entities are always accessed through their containing aggregate root.

## Sleep Data Collection Context Entities

### Patient

The Patient entity represents an individual undergoing sleep health monitoring. It carries a unique PatientId, demographic attributes (date of birth, sex, BMI), and clinical identifiers (medical record number, insurance identifiers). The Patient entity persists across all studies, assessments, and interventions. Its attributes may change (weight, medications) but its identity remains stable.

### RecordingChannel

The RecordingChannel entity represents a single physiological signal stream within a sleep study. It has a unique ChannelId, a channel type (EEG, EOG, EMG, ECG, respiratory, SpO2), a derivation label (e.g., C3-M2, O1-M2), a sampling rate, and signal quality metadata. A PSG study typically contains 15-25 recording channels, each tracked as a distinct entity within the SleepStudy aggregate.

### SensorDevice

The SensorDevice entity represents a physical device used for data collection: a polysomnography system, an actigraph, or a consumer wearable. It carries a DeviceId, manufacturer, model, firmware version, and calibration status. The same device may be used across multiple studies, but within a single SleepStudy aggregate, it is referenced by identity.

## Sleep Stage Analysis Context Entities

### SleepEpisode

The SleepEpisode entity represents a continuous period of sleep within a study, from sleep onset to final awakening. It carries a unique EpisodeId, onset time, offset time, and references the scored epochs that compose it. A single study may contain multiple sleep episodes (e.g., in a multiple sleep latency test, MSLT, which involves four to five nap opportunities).

### RespiratoryEvent

The RespiratoryEvent entity represents a single apnea, hypopnea, or RERA detected during scoring. It carries an EventId, event type (obstructive apnea, central apnea, mixed apnea, hypopnea, RERA), start time, duration, associated oxygen desaturation depth, and the body position at occurrence. Each event is a distinct, identifiable occurrence within the RespiratoryEventSummary aggregate.

### Arousal

The Arousal entity represents a single cortical arousal detected during sleep. It carries an ArousalId, start time, duration, associated sleep stage, and cause classification (spontaneous, respiratory, movement-related). Arousals are individually identifiable events that contribute to the arousal index calculation.

## Circadian Rhythm Context Entities

### LightExposureSession

The LightExposureSession entity represents a continuous period of light exposure measurement. It carries a SessionId, start time, end time, average lux intensity, peak lux intensity, and spectral composition (blue light proportion). Multiple sessions compose a daily light exposure profile within the CircadianProfile aggregate.

## Intervention Tracking Context Entities

### InterventionModule

The InterventionModule entity represents a specific component of a treatment plan. It carries a ModuleId, intervention type (CBT-I stimulus control, CBT-I sleep restriction, CBT-I cognitive restructuring, medication, CPAP therapy, sleep hygiene), prescribed parameters, start date, target end date, and current status (planned, active, paused, completed, discontinued). It transitions through states as treatment progresses.

### TherapyDevice

The TherapyDevice entity represents a therapeutic device assigned to a patient, primarily CPAP/BiPAP machines and oral appliances. It carries a DeviceId, device type, manufacturer, model, assigned pressure settings (fixed or auto-titrating range), mask type, and assignment dates. The entity tracks the device through its assignment lifecycle.

### Clinician

The Clinician entity represents a healthcare provider involved in sleep care. It carries a ClinicianId, credentials (MD, DO, RPSGT, RRT), specialty (sleep medicine, pulmonology, neurology, psychiatry), and institutional affiliation. Clinicians are referenced by identity in treatment plans and clinical reports.

## Clinical Integration Context Entities

### ReferralRequest

The ReferralRequest entity represents a clinical referral for sleep evaluation or follow-up. It carries a ReferralId, referring provider, receiving provider, referral reason, urgency level, and status (pending, accepted, scheduled, completed, cancelled). The entity tracks the referral through its lifecycle.

### FHIRResourceMapping

The FHIRResourceMapping entity tracks the correspondence between internal domain objects and their FHIR R4 resource representations. It carries a MappingId, internal aggregate type and identifier, FHIR resource type, FHIR resource identifier, version, and last synchronization timestamp. This entity enables bidirectional traceability between internal and external representations.

## Entity Lifecycle

Entities in the sleep health metrics domain follow distinct lifecycles. A Patient entity is created at registration and persists indefinitely. A SleepEpisode entity is created during scoring and becomes immutable once the scoring session is finalized. An InterventionModule transitions through states (planned, active, completed) based on clinical decisions. Each entity's lifecycle is governed by its aggregate root.

## References

- Evans, E. (2003). *Domain-Driven Design*. Addison-Wesley. Chapter 5: A Model Expressed in Software -- Entities.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley. Chapter 5: Entities.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media. Chapter 9: Entities.
