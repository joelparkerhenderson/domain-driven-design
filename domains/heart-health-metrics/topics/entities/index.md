# Entities

Entities in the heart health metrics domain are objects with a unique identity that persists over time, even as their attributes change. They represent the core actors and artifacts in cardiac health monitoring that must be tracked, referenced, and distinguished from one another across the system's lifetime.

## Overview

Eric Evans defines an entity as an object that is defined primarily by its identity rather than its attributes. In the heart health metrics domain, entities represent patients, devices, recording sessions, alert subscriptions, and other objects whose lifecycle and traceability matter. A patient entity maintains its identity as vital signs change, devices are replaced, and risk profiles evolve. A device entity persists through firmware updates, calibration adjustments, and reassignment between patients.

Entity identity in cardiac health monitoring carries clinical and regulatory significance. Patient identification must be unambiguous for clinical safety. Device identification must be traceable for FDA compliance. Recording session identification must be auditable for medicolegal purposes.

## Key Concepts

**Patient Entity.** Represents an individual receiving cardiac health monitoring. Identified by a unique patient identifier (which may align with a medical record number). The patient entity's attributes (age, weight, medications, clinical history) change over time, but its identity is constant. It serves as the anchor for all cardiac data, risk assessments, and clinical interactions.

**Device Entity.** Represents a cardiac monitoring device (heart rate sensor, blood pressure monitor, ECG recorder). Identified by a device serial number or unique device identifier (UDI). The device entity tracks calibration status, firmware version, assignment history, and operational status. Its identity persists through maintenance, firmware updates, and patient reassignment.

**Recording Session Entity.** Represents a discrete episode of cardiac data acquisition, such as a 12-lead ECG recording, a 24-hour Holter monitor session, or a home blood pressure monitoring period. Identified by a session identifier. It tracks start time, end time, device used, recording parameters, and quality assessment status.

**Alert Subscription Entity.** Represents an active monitoring subscription linking a patient to a set of alert rules and notification preferences. Identified by a subscription identifier. It tracks which metrics are monitored, threshold configurations, notification channels, and activation/deactivation history.

**Clinician Entity.** Represents a healthcare provider involved in cardiac monitoring, such as a cardiologist, nurse, or cardiac technician. Identified by a provider identifier. It tracks role, specialization, alert routing preferences, and authorization scope.

## Domain Examples

When a patient is enrolled in a remote cardiac monitoring program, a Patient entity is created with a unique identifier. Over the following months, the patient's blood pressure readings change, medications are adjusted, and risk scores are updated. Through all these changes, the Patient entity maintains its identity, allowing longitudinal tracking of cardiac health trends.

When a hospital purchases a new 12-lead ECG machine, a Device entity is created with the manufacturer's unique device identifier. The device is calibrated, assigned to the cardiology department, used for hundreds of recordings, undergoes firmware updates, and is eventually decommissioned. Its identity links all ECG recordings made with that device for quality assurance and regulatory traceability.

## Relationships to Other Topics

- [Aggregates](../aggregates/index.md) - Entities serve as aggregate roots or internal members.
- [Value Objects](../value-objects/index.md) - Entities contain value objects as their attributes.
- [Repositories](../repositories/index.md) - Repositories store and retrieve entities by identity.
- [Domain Events](../domain-events/index.md) - Entity state changes produce domain events.
- [Ubiquitous Language](../ubiquitous-language/index.md) - Entity names form key terms in the ubiquitous language.
- [Bounded Contexts](../bounded-contexts/index.md) - Entities are scoped within their bounded context.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 5 on entities.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 5 on entity design.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly, 2021. Chapter 8 on entity modeling.
- FDA. "Unique Device Identification System." 21 CFR Part 830. U.S. Food and Drug Administration.
- HIMSS. "Patient Identity Integrity." Healthcare Information and Management Systems Society.
