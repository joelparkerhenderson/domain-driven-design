# Domain Events - Asthma Domain

## Overview

Domain events represent significant occurrences within the domain that domain experts care about. They capture something that happened in the past and can trigger reactions in other parts of the system. In the asthma management domain, domain events connect the six bounded contexts by communicating clinical state changes, treatment decisions, environmental alerts, and outcome measurements across context boundaries.

Domain events are named in the past tense using ubiquitous language: AssessmentFinalized, TreatmentPlanSteppedUp, ExacerbationRecorded. They carry the information necessary for consuming contexts to react without needing to query the originating context.

## Domain Event Principles

**Immutability.** Domain events are immutable records of something that has already occurred. Once published, an event cannot be modified or retracted. If an error is discovered, a corrective event is published (for example, AssessmentAmended following an erroneous AssessmentFinalized).

**Past Tense Naming.** Events describe completed actions: SpirometrySessionCompleted, not CompleteSpirometrySession. This convention distinguishes events (things that happened) from commands (requests to do something).

**Self-Contained Payload.** Events carry sufficient data for consumers to react without requiring callbacks to the originating context. An AssessmentFinalized event includes the patient ID, assessment ID, GINA classification, control level, and key spirometry values.

**Temporal Ordering.** Events carry timestamps and can be ordered chronologically. This is critical in the asthma domain where the sequence of clinical events (assessment before treatment change, treatment change before action plan update) carries clinical significance.

## Clinical Assessment Context Events

### AssessmentFinalized

**Trigger:** A clinician completes and signs off on a patient assessment.

**Payload:** PatientID, AssessmentID, AssessmentDate, GINAClassification, ControlLevel, FEV1PercentPredicted, PEFValue, FeNOValue, ClinicalNotes.

**Consumers:** Treatment Management Context (evaluates step change need), Action Planning Context (updates zone thresholds), Outcomes Tracking Context (records assessment data point).

### SpirometrySessionCompleted

**Trigger:** A spirometry testing session is finalized with quality-graded results.

**Payload:** PatientID, SessionID, BestFEV1, BestFVC, FEV1FVCRatio, QualityGrade, BronchodilatorResponse.

**Consumers:** Clinical Assessment Context (internal, feeds into assessment), Outcomes Tracking Context (adds to lung function trend).

### SeverityReclassified

**Trigger:** A patient's GINA severity classification changes from a previous assessment.

**Payload:** PatientID, AssessmentID, PreviousClassification, NewClassification, ReclassificationRationale.

**Consumers:** Treatment Management Context (triggers treatment plan review), Action Planning Context (triggers action plan revision).

## Treatment Management Context Events

### TreatmentPlanSteppedUp

**Trigger:** A clinician escalates the treatment plan to a higher GINA step.

**Payload:** PatientID, TreatmentPlanID, PreviousStep, NewStep, AddedMedications, ClinicalRationale, TriggeringAssessmentID.

**Consumers:** Medication Management Context (creates new prescriptions), Action Planning Context (updates medication instructions).

### TreatmentPlanSteppedDown

**Trigger:** A clinician de-escalates the treatment plan to a lower GINA step.

**Payload:** PatientID, TreatmentPlanID, PreviousStep, NewStep, DiscontinuedMedications, ClinicalRationale.

**Consumers:** Medication Management Context (discontinues prescriptions), Action Planning Context (updates medication instructions).

### BiologicTherapyInitiated

**Trigger:** A patient begins biologic agent therapy after meeting eligibility criteria.

**Payload:** PatientID, TherapyID, BiologicAgent, DosingSchedule, EligibilityCriteria, ExpectedReviewDate.

**Consumers:** Medication Management Context (creates biologic prescription), Outcomes Tracking Context (begins biologic response tracking).

### BiologicResponseAssessed

**Trigger:** A scheduled biologic therapy response assessment is completed.

**Payload:** PatientID, TherapyID, ResponseClassification, BiomarkerChanges, ClinicalRecommendation.

**Consumers:** Outcomes Tracking Context (records therapy response), Medication Management Context (continues or discontinues biologic).

## Trigger Monitoring Context Events

### HighRiskExposureDetected

**Trigger:** Air quality, pollen levels, or occupational exposure exceeds risk thresholds for a patient.

**Payload:** PatientID, TriggerType (allergen, AQI, occupational), TriggerDetails, SeverityLevel, GeographicLocation, DetectionTimestamp.

**Consumers:** Action Planning Context (activates precautionary measures), Outcomes Tracking Context (records exposure for correlation analysis).

### TriggerProfileUpdated

**Trigger:** A patient's trigger profile is modified with new allergen sensitivities or removed sensitivities.

**Payload:** PatientID, TriggerProfileID, AddedTriggers, RemovedTriggers, UpdateReason.

**Consumers:** Action Planning Context (updates avoidance recommendations), Treatment Management Context (may influence immunotherapy decisions).

## Action Planning Context Events

### ActionPlanActivated

**Trigger:** A new or revised asthma action plan is activated for a patient.

**Payload:** PatientID, ActionPlanID, GreenZoneThresholds, YellowZoneThresholds, RedZoneThresholds, ActivationDate.

**Consumers:** Medication Management Context (verifies medication alignment), Outcomes Tracking Context (records plan activation).

### ZoneEscalationDetected

**Trigger:** A patient's symptoms or PEF readings indicate transition from green to yellow or yellow to red zone.

**Payload:** PatientID, ActionPlanID, PreviousZone, NewZone, TriggeringMeasurement, EscalationTimestamp.

**Consumers:** Treatment Management Context (may trigger urgent review), Outcomes Tracking Context (records zone escalation event).

## Medication Management Context Events

### AdherenceAlertRaised

**Trigger:** A patient's medication adherence falls below the acceptable threshold or rescue inhaler use exceeds baseline.

**Payload:** PatientID, MedicationID, AdherenceRate, MeasurementPeriod, AlertType (low adherence, excessive rescue use), AlertThreshold.

**Consumers:** Treatment Management Context (considers treatment adjustment), Outcomes Tracking Context (records adherence alert), Action Planning Context (may trigger patient outreach).

### PrescriptionFilled

**Trigger:** A pharmacy confirms dispensing of a prescribed medication.

**Payload:** PatientID, PrescriptionID, MedicationName, QuantityDispensed, DaysSupply, PharmacyID, FillDate.

**Consumers:** Medication Management Context (internal, updates refill status), Outcomes Tracking Context (tracks fill patterns).

## Outcomes Tracking Context Events

### ACTScoreRecorded

**Trigger:** A patient completes an Asthma Control Test questionnaire.

**Payload:** PatientID, ACTScore, ControlClassification, PreviousACTScore, ChangeFromPrevious, AssessmentDate.

**Consumers:** Treatment Management Context (declining scores may trigger review), Action Planning Context (may adjust zone thresholds).

### ExacerbationRecorded

**Trigger:** A clinician documents an asthma exacerbation event.

**Payload:** PatientID, ExacerbationID, SeverityLevel, OnsetDate, TreatmentRequired, IdentifiedTriggers, ResolutionDate.

**Consumers:** Treatment Management Context (evaluates step-up need), Trigger Monitoring Context (updates trigger correlation data).

### OutcomeTrendAlertRaised

**Trigger:** Longitudinal analysis detects a concerning trend (declining FEV1, increasing exacerbation frequency, worsening ACT scores).

**Payload:** PatientID, TrendType, TrendDirection, TrendMagnitude, AnalysisPeriod, ClinicalSignificance.

**Consumers:** Treatment Management Context (triggers treatment plan review).

## Event Flow Example

A typical clinical workflow generates a chain of domain events:

1. SpirometrySessionCompleted (Clinical Assessment)
2. AssessmentFinalized with new ControlLevel (Clinical Assessment)
3. TreatmentPlanSteppedUp (Treatment Management, reacting to assessment)
4. ActionPlanActivated with updated medication instructions (Action Planning)
5. PrescriptionFilled for new controller medication (Medication Management)
6. ACTScoreRecorded at follow-up visit (Outcomes Tracking)

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 8 on domain events.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 8 on domain events.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 6 on domain events.
