# Entities - Asthma Domain

## Overview

Entities are domain objects defined by their unique identity rather than their attributes. An entity persists over time, and its identity remains stable even as its attributes change. In the asthma management domain, entities represent clinical objects that must be tracked, referenced, and updated throughout their lifecycle -- patients, assessment sessions, treatment plans, prescriptions, and exacerbation events.

The distinction between entities and value objects is fundamental to modeling the asthma domain correctly. A spirometry session is an entity because it has a unique identity (session ID), a lifecycle (scheduled, in-progress, completed, reviewed), and is referenced by other objects. An FEV1 measurement within that session is a value object because it is defined entirely by its attributes (value, units, quality grade).

## Entity Design Principles

**Identity.** Every entity has a unique identifier that distinguishes it from all other entities of the same type. In the asthma domain, identifiers are typically system-generated UUIDs or composite keys that include patient ID and timestamp components.

**Lifecycle.** Entities progress through defined states. A SpirometrySession moves from Scheduled to InProgress to Completed to Reviewed. A TreatmentPlan moves from Draft to Active to Superseded. These lifecycle transitions are governed by domain rules.

**Mutability.** Unlike value objects, entities can change their attributes over time. A TreatmentPlan entity changes its therapy step, a TriggerProfile entity acquires new allergen sensitivities, and an AdherenceRecord entity accumulates new dose tracking data.

**Equality.** Two entities are equal if and only if they have the same identity, regardless of their current attribute values. Two SpirometrySession entities with identical FEV1 values are still different entities if they have different session IDs.

## Clinical Assessment Context Entities

### PatientAssessment

**Identity:** AssessmentID (UUID).

**Attributes:** PatientID, assessment date, assessing clinician, GINA classification, control level, clinical notes, status (draft, finalized, amended).

**Lifecycle:** Created when a clinician initiates an assessment. Transitions to finalized when the clinician signs off. May be amended with documented rationale, creating a new version while preserving the original.

**Behavior:** Validates that classification criteria are met before allowing finalization. Enforces that amendments include a reason and clinician signature.

### SpirometryManeuver

**Identity:** ManeuverID within a SpirometrySession.

**Attributes:** Maneuver number, FEV1 value, FVC value, peak flow value, effort quality, acceptability flag, reproducibility flag.

**Lifecycle:** Created during a spirometry test session. Marked as acceptable or unacceptable based on ATS/ERS quality criteria. Cannot be modified after the session is finalized.

### AssessmentNote

**Identity:** NoteID within a PatientAssessment.

**Attributes:** Note text, author, timestamp, note type (clinical observation, patient-reported symptom, recommendation).

**Lifecycle:** Created by the assessing clinician. Immutable after creation. Additional notes can be appended but existing notes cannot be modified.

## Treatment Management Context Entities

### MedicationAssignment

**Identity:** AssignmentID within a TreatmentPlan.

**Attributes:** Medication name, medication class (ICS, LABA, SABA, LTRA, biologic), dose, frequency, device type, assignment date, discontinuation date, discontinuation reason.

**Lifecycle:** Created when a medication is added to a treatment plan. Active while the treatment plan is active and the medication has not been discontinued. Discontinued with a documented reason when therapy changes.

### StepChangeRecord

**Identity:** ChangeRecordID within a TreatmentPlan.

**Attributes:** Previous step, new step, change direction (step-up, step-down), change date, clinical rationale, triggering assessment ID, approving clinician.

**Lifecycle:** Created when a treatment plan step changes. Immutable after creation. Serves as an audit trail for treatment modifications.

### ResponseAssessment

**Identity:** ResponseAssessmentID within a BiologicTherapy.

**Attributes:** Assessment date, response classification (good response, partial response, non-response), biomarker values at assessment, clinician recommendation (continue, discontinue, switch).

**Lifecycle:** Created at scheduled assessment intervals (typically every 4 months). Immutable after finalization. Drives continuation or discontinuation of biologic therapy.

## Trigger Monitoring Context Entities

### AllergenSensitivity

**Identity:** SensitivityID within a TriggerProfile.

**Attributes:** Allergen code, allergen name, sensitivity level (mild, moderate, severe), confirmation method (skin prick test, specific IgE, clinical history), date confirmed, notes.

**Lifecycle:** Created when an allergen sensitivity is identified through testing or clinical history. May be updated if sensitivity level changes on retesting. Can be marked inactive if desensitization is achieved through immunotherapy.

### OccupationalAssessment

**Identity:** OccupationalAssessmentID.

**Attributes:** PatientID, workplace description, identified exposures, exposure frequency, exposure duration, protective measures in use, assessment date, assessing clinician, recommendations.

**Lifecycle:** Created during occupational exposure evaluation. Updated when workplace conditions change. Referenced by treatment plans to guide occupational asthma management.

## Action Planning Context Entities

### MedicationInstruction

**Identity:** InstructionID within an AsthmaActionPlan.

**Attributes:** Zone (green, yellow, red), medication name, dose, frequency, administration method, special instructions, sequence within zone.

**Lifecycle:** Created when an action plan is developed. Updated when the treatment plan changes necessitate action plan revision. Must be written in patient-understandable language.

## Medication Management Context Entities

### Prescription

**Identity:** PrescriptionID within a MedicationRegimen.

**Attributes:** Medication name, drug code (RxNorm, NDC), dose, frequency, quantity, refills authorized, prescribing clinician, pharmacy, status (active, discontinued, expired), start date, end date.

**Lifecycle:** Created when a clinician prescribes a medication. Active until discontinued, expired, or superseded. Refill tracking updates remaining refill count.

### MissedDoseEvent

**Identity:** EventID within an AdherenceRecord.

**Attributes:** Medication, scheduled dose time, detection method (electronic monitor, patient report, refill gap), reason reported (forgot, side effects, cost, felt well), follow-up action taken.

**Lifecycle:** Created when a missed dose is detected. Immutable. Aggregated for adherence rate calculation.

## Outcomes Tracking Context Entities

### LungFunctionTrend

**Identity:** TrendID within a PatientOutcomeRecord.

**Attributes:** Measurement type (FEV1, PEF), data points (time series of measurements), trend direction (improving, stable, declining), rate of change, clinical significance flag, analysis period.

**Lifecycle:** Created when sufficient data points exist for trend analysis (minimum three). Continuously updated as new measurements are recorded. Flagged for clinical review when decline exceeds threshold.

### ExacerbationEvent

**Identity:** ExacerbationID.

**Attributes:** PatientID, onset date, resolution date, severity (mild, moderate, severe), symptoms, trigger factors, treatment required (medication change, OCS burst, ED visit, hospitalization), recovery duration, follow-up actions.

**Lifecycle:** Created when an exacerbation is reported or detected. Updated with treatment details and resolution information. Closed when the patient returns to baseline. Referenced by outcomes tracking for frequency analysis.

## Entity vs. Value Object Decision Guide

In the asthma domain, the following heuristic guides entity vs. value object decisions:

- If the object must be tracked over time and referenced by other objects, it is an entity (SpirometrySession, Prescription, ExacerbationEvent).
- If the object is defined entirely by its attributes and can be replaced by another object with the same attributes, it is a value object (FEV1, ACTScore, AirQualityReading).
- If the object has a lifecycle with state transitions, it is an entity (TreatmentPlan, PatientAssessment).
- If the object is immutable once created, it is likely a value object (GINAClassification, ZoneThreshold).

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 5 on entities.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 5 on entity design.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 4 on entities and value objects.
