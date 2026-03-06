# Domain Events

Domain events in the heart health metrics domain represent significant occurrences within the cardiac monitoring system that are meaningful to domain experts and may trigger actions in other bounded contexts. They capture the fact that something clinically or operationally important happened at a specific point in time.

## Overview

Eric Evans and subsequent DDD practitioners emphasize domain events as a mechanism for capturing meaningful happenings in the domain. In the heart health metrics domain, domain events are essential because cardiac monitoring is inherently event-driven: a heart rhythm changes, a blood pressure threshold is exceeded, an ECG recording completes, or a risk score crosses into a higher category. Each of these occurrences has implications for downstream contexts.

Domain events enable loose coupling between bounded contexts. The Cardiac Analysis Context does not need to know about the Monitoring and Alerts Context; it simply publishes an "Arrhythmia Detected" event. Any interested context can subscribe and respond appropriately.

## Key Concepts

**Arrhythmia Detected Event.** Published by the Cardiac Analysis Context when rhythm analysis identifies an abnormal heart rhythm. The event payload includes the patient identifier, arrhythmia type (atrial fibrillation, ventricular tachycardia, bradycardia), onset timestamp, confidence level, and the supporting ECG segment reference. The Monitoring and Alerts Context subscribes to evaluate alert rules.

**Critical Blood Pressure Threshold Exceeded Event.** Published when a blood pressure reading exceeds a critical threshold (e.g., systolic above 180 mmHg or diastolic above 120 mmHg). The payload includes the patient identifier, the reading values, the threshold that was exceeded, and the severity classification. This event may trigger emergency notification workflows.

**ECG Recording Completed Event.** Published by the Data Collection Context when an ECG recording session finishes. The payload includes the session identifier, recording duration, lead configuration, and signal quality summary. The Cardiac Analysis Context subscribes to begin automated rhythm analysis.

**Risk Score Updated Event.** Published by the Risk Assessment Context when a patient's cardiovascular risk score is recalculated. The payload includes the patient identifier, the previous and current risk scores, the scoring model used, and whether the risk category changed. The Reporting Context subscribes to update dashboards and trend reports.

**Heart Rate Anomaly Detected Event.** Published when sustained heart rate values fall outside expected ranges for a patient's profile (e.g., sustained resting tachycardia above 120 bpm or bradycardia below 40 bpm). The payload includes the patient identifier, the anomalous values, duration, and baseline comparison.

**Device Calibration Expired Event.** Published by the Data Collection Context when a monitoring device's calibration period expires. This event triggers workflows for recalibration or device replacement and may pause data collection from the affected device.

## Domain Examples

A patient wearing a continuous ECG monitor develops atrial fibrillation at 2:30 AM. The Cardiac Analysis Context detects the rhythm change and publishes an "Arrhythmia Detected" event with type "atrial fibrillation" and high confidence. The Monitoring and Alerts Context receives the event, evaluates the patient's alert rules, and triggers an urgent notification to the on-call cardiologist. The Reporting Context records the event for inclusion in the patient's cardiac history timeline. The Clinical Integration Context translates the event into a FHIR Flag resource and posts it to the hospital EHR.

A home blood pressure monitoring series completes after 7 days. The Data Collection Context publishes a "Monitoring Period Completed" event. The Cardiac Analysis Context computes BP trends and variability. The Risk Assessment Context recalculates the patient's cardiovascular risk with the updated data and publishes a "Risk Score Updated" event.

## Relationships to Other Topics

- [Aggregates](../aggregates/index.md) - Aggregates emit domain events upon significant state changes.
- [Event-Driven Architecture](../event-driven-architecture/index.md) - Domain events are the building blocks of event-driven communication.
- [Bounded Contexts](../bounded-contexts/index.md) - Domain events cross bounded context boundaries.
- [Monitoring & Alerts Context](../monitoring-alerts-context/index.md) - Primary consumer of clinical domain events.
- [Domain Services](../domain-services/index.md) - Services may orchestrate event publication.
- [Integration Patterns](../integration-patterns/index.md) - Events are the basis for asynchronous integration.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 8 on domain events.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly, 2021. Chapter 11 on domain events.
- Brandolini, Alberto. "Introducing EventStorming." Leanpub, 2021.
- Stopford, Ben. *Designing Event-Driven Systems*. O'Reilly, 2018.
