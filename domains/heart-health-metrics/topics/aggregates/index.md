# Aggregates

Aggregates in the heart health metrics domain are clusters of related cardiac health objects treated as a single unit for data changes and consistency enforcement. Each aggregate has a root entity that controls access to its internal objects and enforces invariants across all cardiac measurements and clinical findings within its boundary.

## Overview

Eric Evans defines an aggregate as a cluster of associated objects that are treated as a unit for the purpose of data changes. In the heart health metrics domain, aggregates ensure that cardiac data maintains clinical consistency. A blood pressure series aggregate, for example, ensures that all readings within a monitoring session share the same patient context, device calibration, and measurement protocol. Changes to any reading within the series must go through the aggregate root.

Aggregate design in cardiac health is particularly important because clinical decisions depend on internally consistent data sets. A risk assessment cannot mix blood pressure readings from different patients or measurement protocols. An ECG recording session must maintain the integrity of all its lead channels and timing synchronization.

## Key Concepts

**Patient Cardiac Profile Aggregate.** The central aggregate that represents a patient's complete cardiac health identity. It is the root for demographic information, clinical history relevant to cardiac health, active monitoring subscriptions, and baseline vital sign ranges. All cardiac data ultimately references a Patient Cardiac Profile.

**ECG Recording Session Aggregate.** Encapsulates a complete electrocardiogram recording event, including lead configurations, signal duration, sampling parameters, and the raw waveform data across all channels. The aggregate root ensures that all leads within a session are temporally synchronized and that the recording meets minimum quality thresholds before being accepted.

**Blood Pressure Series Aggregate.** Groups a sequence of blood pressure readings taken under a defined monitoring protocol (e.g., ambulatory 24-hour monitoring, home self-monitoring series). The aggregate root enforces protocol rules such as minimum number of readings, required time intervals, and measurement position consistency.

**Risk Assessment Report Aggregate.** Encapsulates a complete cardiovascular risk evaluation, including the input parameters (demographics, clinical values, biomarkers), the scoring model applied, the computed risk scores, and the risk stratification result. The aggregate root ensures that all inputs are present and valid before computing the score.

**Alert Subscription Aggregate.** Represents a patient's or clinician's monitoring subscription, including the metrics being watched, threshold configurations, notification preferences, and escalation rules. The aggregate root ensures that alert rules are internally consistent (e.g., critical threshold is more extreme than warning threshold).

## Domain Examples

When a nurse initiates a 24-hour ambulatory blood pressure monitoring session, the system creates a Blood Pressure Series aggregate with the monitoring protocol parameters. Each reading added to the series is validated by the aggregate root: correct patient identity, device calibration status, and protocol compliance. The aggregate rejects readings that violate protocol rules (e.g., readings taken less than 15 minutes apart when the protocol requires 30-minute intervals).

When a cardiologist reviews an ECG, the ECG Recording Session aggregate ensures that all 12 leads are present, the recording duration meets the minimum requirement, and signal quality metrics are within acceptable bounds. The aggregate root exposes methods for marking the recording as reviewed, annotated, or flagged for follow-up.

## Relationships to Other Topics

- [Entities](../entities/index.md) - Aggregate roots are entities with unique identity.
- [Value Objects](../value-objects/index.md) - Aggregates contain value objects representing measurements.
- [Repositories](../repositories/index.md) - Repositories store and retrieve aggregate roots.
- [Domain Events](../domain-events/index.md) - Aggregates emit domain events when significant state changes occur.
- [Bounded Contexts](../bounded-contexts/index.md) - Each bounded context defines its own aggregates.
- [Domain Services](../domain-services/index.md) - Services coordinate operations across aggregates.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 6 on aggregates.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 10 on aggregate design rules.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly, 2021. Chapter 10 on aggregate boundaries.
- Vernon, Vaughn. "Effective Aggregate Design." DDD Community, 2011. Three-part series on aggregate sizing.
- Task Force of the European Society of Cardiology. "Heart Rate Variability: Standards of Measurement." *Circulation*, 93(5), 1043-1065, 1996.
