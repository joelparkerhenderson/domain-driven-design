# Entities for Body Health Metrics

## Overview

Entities in the body health metrics domain are objects defined by a unique identity that persists over time, even as their attributes change. Unlike value objects, which are defined by their attributes, entities maintain continuity of identity across state transitions. A person's body weight changes daily, but the person entity remains the same individual. A measurement device may be recalibrated, but it retains its device identity and measurement history.

## Key Entities

### Person

The Person entity represents an individual whose body health metrics are being tracked. This is the central identity around which all measurements, assessments, and scores are organized.

- **Identity**: PersonId (globally unique identifier)
- **Attributes**: date of birth, biological sex, demographic profile
- **Lifecycle**: Created when an individual begins health tracking; persists indefinitely as the anchor for all longitudinal health data
- **Significance**: A person's identity connects measurement sessions across time, enabling trend analysis and progress tracking. The same PersonId links data across all six bounded contexts.

### MeasurementSession

The MeasurementSession entity represents a discrete event during which body health metrics are recorded. Each session has a unique identity that connects the measurements taken together.

- **Identity**: MeasurementSessionId
- **Attributes**: timestamp, location, administering practitioner, measurement conditions
- **Lifecycle**: Created when a measurement event begins; completed when all planned measurements are recorded; immutable after completion
- **Significance**: Sessions group temporally related measurements. A morning weigh-in session differs from an afternoon clinical assessment session, even if they record the same metric types.

### MeasurementDevice

The MeasurementDevice entity represents a physical instrument used to capture body health data, such as a scale, stadiometer, blood pressure cuff, or bioimpedance analyzer.

- **Identity**: DeviceId
- **Attributes**: device type, manufacturer, model, calibration history, accuracy specifications
- **Lifecycle**: Registered when introduced to the system; maintained through calibration events; decommissioned when retired from use
- **Significance**: Device identity enables measurement traceability. When a measurement is questioned, the device's calibration history at the time of measurement can be reviewed.

### FitnessAssessment

The FitnessAssessment entity represents a specific evaluation event where an individual's physical fitness is tested using a standardized protocol.

- **Identity**: FitnessAssessmentId
- **Attributes**: assessment date, protocol used, administering assessor, environmental conditions, individual test results
- **Lifecycle**: Initiated when an assessment begins; completed when all protocol steps are finished and results are verified
- **Significance**: Assessment identity connects the protocol, conditions, and results of a specific evaluation, enabling comparison across assessments over time.

### HealthGoal

The HealthGoal entity represents a specific, measurable health improvement objective set by or for an individual.

- **Identity**: HealthGoalId
- **Attributes**: target metric, target value, start date, target date, current progress, milestone definitions
- **Lifecycle**: Created when a goal is established; updated as progress occurs; completed when the target is reached or the goal is abandoned
- **Significance**: Goal identity persists through progress updates, allowing the system to track the complete journey from goal setting to achievement or abandonment.

### ClinicalReport

The ClinicalReport entity represents a formatted compilation of health metrics prepared for clinical consumption.

- **Identity**: ClinicalReportId
- **Attributes**: report date, intended recipient, included observations, clinical coding, submission status
- **Lifecycle**: Created when report generation is requested; populated with clinical observations; submitted to external systems; status tracked through confirmation
- **Significance**: Report identity enables tracking of clinical data submissions, acknowledgment receipts, and potential resubmission needs.

## Entity Design Principles

**Identity stability**: Entity identifiers never change once assigned. A PersonId assigned at registration remains the same identifier throughout the individual's entire measurement history, even if their name or demographic attributes change.

**Attribute mutability**: Unlike value objects, entity attributes can change over time. A MeasurementDevice entity's calibration status changes with each calibration event. A HealthGoal entity's current progress updates with each new measurement.

**Identity equality**: Two entities are equal if and only if they share the same identity, regardless of attribute values. Two MeasurementSession entities with identical measurements but different MeasurementSessionIds are distinct sessions.

**Minimal entity scope**: Entities carry only the attributes necessary for their responsibilities. A Person entity in the body health metrics domain does not carry address, insurance, or billing information -- those attributes belong to other domains.

**Cross-context identity**: Some entity identities span bounded context boundaries. PersonId and MeasurementSessionId are shared across contexts through the shared kernel. However, each context may maintain different attributes for the same identity.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 5 on entities.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 5 on entity design.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 5 on tactical design patterns.
