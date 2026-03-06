# Aggregates in the Audiology Domain

## Overview

Aggregates are clusters of related domain objects treated as a single unit for the purpose of data consistency. Each aggregate has a root entity that controls access to all objects within the aggregate and enforces business invariants. In the audiology domain, aggregates represent the fundamental transactional boundaries within each bounded context, ensuring that clinical data maintains its integrity across operations.

## Aggregate Design Principles

Aggregates in the audiology domain follow four core principles. First, each aggregate enforces its own invariants: no external object can put an aggregate into an invalid state. Second, aggregates are the unit of persistence: repositories store and retrieve complete aggregates. Third, aggregates communicate with other aggregates only through their root entities and domain events. Fourth, aggregates are kept as small as possible while still maintaining consistency.

## Hearing Assessment Context Aggregates

### Audiometric Evaluation Aggregate

The Audiometric Evaluation is the primary aggregate in the Hearing Assessment Context. The aggregate root is the AudiometricEvaluation entity, which contains a collection of ThresholdMeasurement value objects (each specifying frequency, intensity, ear, and conduction type), a TestConfiguration value object (specifying masking parameters, transducer type, and calibration reference), and a resulting Audiogram value object. The aggregate enforces invariants such as: thresholds must fall within valid dB HL ranges, masking must be applied when interaural attenuation rules require it, and the audiogram must be internally consistent with the measured thresholds.

### Speech Recognition Evaluation Aggregate

The Speech Recognition Evaluation aggregate contains the SpeechRecognitionEvaluation root entity, SpeechRecognitionThreshold value objects, and WordRecognitionScore value objects. This aggregate enforces that presentation levels are appropriate for the patient's hearing thresholds and that word lists are scored correctly.

## Device Management Context Aggregates

### Device Fitting Aggregate

The Device Fitting is the primary aggregate in the Device Management Context. The aggregate root is the DeviceFitting entity, which contains a FittingPrescription value object (calculated target gain at each frequency), DeviceConfiguration value objects (current programming parameters), and RealEarMeasurement value objects (verification data). The aggregate enforces that the device configuration does not exceed maximum power output safety limits and that verification measurements are compared against prescriptive targets.

### Device Aggregate

The Device aggregate tracks the lifecycle of a hearing device. The aggregate root is the Device entity, which contains DeviceSpecification value objects (manufacturer, model, serial number, technology level), MaintenanceRecord value objects, and WarrantyStatus. The aggregate enforces device-specific constraints such as valid programming ranges and maintenance intervals.

## Rehabilitation Context Aggregates

### Rehabilitation Plan Aggregate

The Rehabilitation Plan is the primary aggregate in the Rehabilitation Context. The aggregate root is the RehabilitationPlan entity, which contains RehabilitationGoal value objects, InterventionActivity entities, and OutcomeMeasurement value objects. The aggregate enforces that goals are measurable, interventions align with stated goals, and outcome measurements use validated instruments.

## Vestibular Services Context Aggregates

### Vestibular Evaluation Aggregate

The Vestibular Evaluation is the primary aggregate. The aggregate root is the VestibularEvaluation entity, containing VNGResult value objects, CaloricTestResult value objects, PositionalTestResult value objects, and a VestibularDiagnosis value object. The aggregate enforces that test results are complete before a diagnosis is assigned and that conflicting findings are flagged for clinical review.

## Pediatric Audiology Context Aggregates

### Pediatric Patient Aggregate

The Pediatric Patient aggregate extends the standard patient model with developmental context. The aggregate root is the PediatricPatient entity, containing DevelopmentalMilestone value objects, ScreeningResult value objects, and FamilyEngagementPlan entities. The aggregate enforces age-appropriate testing method selection and milestone tracking intervals.

## Clinical Documentation Context Aggregates

### Clinical Report Aggregate

The Clinical Report is the primary aggregate. The aggregate root is the ClinicalReport entity, containing ReportSection value objects, ClinicalImpression value objects, and Recommendation value objects. The aggregate enforces that reports include all required sections per documentation standards and that clinical impressions are supported by referenced test data.

## Aggregate Boundaries

Aggregate boundaries in the audiology domain are drawn to minimize cross-aggregate transactions. An audiometric evaluation is a self-contained clinical event that does not need to reference device fitting data within the same transaction. If a device fitting depends on assessment results, this dependency is resolved through domain events rather than direct aggregate references. This design ensures that each aggregate can be modified, validated, and persisted independently.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley. Chapter 6 on aggregates.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley. Chapter 10 on aggregate design.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media. Chapter 7 on aggregate patterns.
