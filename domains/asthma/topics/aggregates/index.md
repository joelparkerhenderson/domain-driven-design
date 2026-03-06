# Aggregates - Asthma Domain

## Overview

Aggregates are clusters of related entities and value objects treated as a single unit for data consistency. Each aggregate has a root entity that controls access to all objects within the aggregate and enforces invariants (business rules that must always be true). In the asthma management domain, aggregates ensure that clinical data integrity, treatment plan consistency, and medication safety rules are maintained within transactional boundaries.

Aggregate design in the asthma domain follows the principle of small, focused aggregates that protect specific invariants. Large aggregates that span multiple clinical concerns would create unnecessary contention and coupling.

## Clinical Assessment Context Aggregates

### PatientAssessment Aggregate

**Root Entity:** PatientAssessment

**Contained Objects:** GINAClassification (value object), ControlLevel (value object), AssessmentNote (entity), RiskFactorList (value object).

**Invariants:**
- A PatientAssessment must have at least one diagnostic data source (spirometry session, peak flow record, or FeNO reading) before a GINA classification can be assigned.
- The ControlLevel must be consistent with the most recent spirometry and symptom data.
- AssessmentNotes are immutable once finalized by the reviewing clinician.

### SpirometrySession Aggregate

**Root Entity:** SpirometrySession

**Contained Objects:** SpirometryManeuver (entity), FEV1 (value object), FVC (value object), FEV1FVCRatio (value object), PredictedValues (value object), QualityGrade (value object).

**Invariants:**
- A SpirometrySession must contain at least three acceptable maneuvers with at least two reproducible results per ATS/ERS standards.
- The best FEV1 and best FVC are selected from acceptable maneuvers (not necessarily the same maneuver).
- QualityGrade must be assigned before the session is finalized.

### PeakFlowRecord Aggregate

**Root Entity:** PeakFlowRecord

**Contained Objects:** PEFReading (value object), PersonalBest (value object), PercentPersonalBest (value object).

**Invariants:**
- Each PEFReading must be within physiologically plausible range (60-900 L/min for adults).
- PersonalBest is updated only when a new reading exceeds the current personal best under well-controlled conditions.

## Treatment Management Context Aggregates

### TreatmentPlan Aggregate

**Root Entity:** TreatmentPlan

**Contained Objects:** TherapyStep (value object), MedicationAssignment (entity), StepChangeRecord (entity), TreatmentGoal (value object).

**Invariants:**
- A TreatmentPlan must be at a valid GINA step (1 through 5).
- Step changes (up or down) must be supported by documented clinical rationale.
- LABA medications cannot be prescribed without a concurrent ICS (safety rule).
- Only one active TreatmentPlan per patient at any time.

### BiologicTherapy Aggregate

**Root Entity:** BiologicTherapy

**Contained Objects:** EligibilityCriteria (value object), DosingSchedule (value object), ResponseAssessment (entity).

**Invariants:**
- Biologic therapy requires documented failure of Step 4 or Step 5 conventional therapy.
- Eligibility criteria must be met (IgE level, eosinophil count, or FeNO thresholds per agent).
- ResponseAssessment must be performed at 4-month intervals to justify continuation.

## Trigger Monitoring Context Aggregates

### TriggerProfile Aggregate

**Root Entity:** TriggerProfile

**Contained Objects:** AllergenSensitivity (entity), TriggerCategory (value object), SensitivityLevel (value object).

**Invariants:**
- A TriggerProfile belongs to exactly one patient.
- AllergenSensitivities must reference validated allergen codes (standardized allergen taxonomy).
- Duplicate allergen entries are not permitted within the same profile.

### EnvironmentalExposure Aggregate

**Root Entity:** EnvironmentalExposure

**Contained Objects:** AirQualityReading (value object), PollenReading (value object), ExposureDuration (value object), GeographicLocation (value object).

**Invariants:**
- Environmental readings must have a valid timestamp and geographic location.
- AQI values must be within the standard range (0-500).

## Action Planning Context Aggregates

### AsthmaActionPlan Aggregate

**Root Entity:** AsthmaActionPlan

**Contained Objects:** GreenZone (value object), YellowZone (value object), RedZone (value object), ZoneThreshold (value object), MedicationInstruction (entity), EmergencyContact (value object).

**Invariants:**
- An action plan must define all three zones (green, yellow, red) with non-overlapping thresholds.
- Zone thresholds must be derived from the patient's personal best PEF.
- Green zone lower threshold must equal yellow zone upper threshold.
- Red zone must always include instructions to seek emergency care.
- An action plan must have at least one emergency contact.

## Medication Management Context Aggregates

### MedicationRegimen Aggregate

**Root Entity:** MedicationRegimen

**Contained Objects:** Prescription (entity), DosingSchedule (value object), RefillStatus (value object).

**Invariants:**
- A regimen cannot contain duplicate active prescriptions for the same medication.
- Controller medications must have a defined daily dosing schedule.
- Prescription discontinuation must record a reason.

### AdherenceRecord Aggregate

**Root Entity:** AdherenceRecord

**Contained Objects:** AdherenceRate (value object), MissedDoseEvent (entity), RefillGap (value object).

**Invariants:**
- AdherenceRate is calculated over a defined measurement period (typically 30 days).
- Adherence rate must be between 0% and 100%.

## Outcomes Tracking Context Aggregates

### PatientOutcomeRecord Aggregate

**Root Entity:** PatientOutcomeRecord

**Contained Objects:** ACTScore (value object), LungFunctionTrend (entity), ExacerbationSummary (value object).

**Invariants:**
- ACTScore must be between 5 and 25 (five questions, each scored 1-5).
- LungFunctionTrend requires at least three data points for trend calculation.

### ExacerbationEvent Aggregate

**Root Entity:** ExacerbationEvent

**Contained Objects:** SeverityLevel (value object), TreatmentRequired (value object), ContributingFactors (value object), RecoveryDuration (value object).

**Invariants:**
- An exacerbation must have a severity classification (mild, moderate, severe).
- TreatmentRequired must document interventions (OCS burst, ED visit, hospitalization).

## Design Principles

- **Small Aggregates.** Each aggregate protects a focused set of invariants. The SpirometrySession aggregate does not include treatment plan data, even though treatment decisions depend on spirometry results.
- **Reference by Identity.** Aggregates in different bounded contexts reference each other by identity (patient ID, assessment ID) rather than direct object references.
- **Eventual Consistency.** Cross-aggregate consistency is achieved through domain events rather than transactions spanning multiple aggregates.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 6 on aggregates.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 10 on aggregate design.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 5 on aggregate patterns.
