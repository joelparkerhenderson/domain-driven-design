# Ventilatory Support Context

## Overview

The Ventilatory Support Context manages the prescription, configuration, monitoring, and weaning of ventilatory support for patients with respiratory failure or chronic ventilatory insufficiency. It covers non-invasive ventilation (NIV) using BiPAP and CPAP, invasive mechanical ventilation through endotracheal tubes or tracheostomies, high-flow nasal therapy (HFNT), and home ventilation programs. This bounded context is classified as a supporting subdomain because it follows established clinical protocols, though the device integration and safety monitoring it provides are critical for patient outcomes.

This context receives airway establishment notifications from the Airway Management Context through a shared kernel and publishes weaning completion events that trigger rehabilitation referrals in the Pulmonary Rehabilitation Context.

## Ubiquitous Language

Key terms within this context include: non-invasive ventilation (NIV), BiPAP, CPAP, IPAP, EPAP, mechanical ventilation, volume control ventilation, pressure control ventilation, pressure support ventilation, SIMV, PEEP, FiO2, tidal volume, respiratory rate (set and actual), minute ventilation, I:E ratio, trigger sensitivity, rise time, cycling criteria, high-flow nasal therapy, flow rate, ventilator alarm, alarm threshold, spontaneous breathing trial (SBT), weaning protocol, weaning readiness, rapid shallow breathing index (RSBI), home ventilation, ventilator compliance, and device telemetry.

## Aggregate: Ventilator Configuration

The Ventilator Configuration is the aggregate root, representing the current and historical ventilatory support settings for a patient.

**Components**:
- Configuration ID (unique identifier)
- Patient ID (reference)
- Ventilation modality (NIV, invasive mechanical ventilation, HFNT)
- Ventilation mode (volume control, pressure control, pressure support, SIMV, BiPAP, CPAP)
- Parameter settings (tidal volume, rate, PEEP, FiO2, pressure support level, IPAP, EPAP, flow rate)
- Alarm thresholds (high pressure, low pressure, high rate, low rate, apnea, low tidal volume, high FiO2)
- Adjustment history (timestamped log of all parameter changes with clinician ID and rationale)
- Patient interface (mask type for NIV, ETT/tracheostomy for invasive, nasal cannula for HFNT)
- Prescribing clinician ID
- Configuration status (active, on hold, discontinued)
- Start date and time

**Invariants**:
- Tidal volume must not exceed 8 mL/kg of predicted body weight for lung-protective ventilation in ARDS.
- FiO2 and PEEP combinations must follow established tables (e.g., ARDS Network low-PEEP or high-PEEP tables) when applicable.
- All mandatory alarm thresholds must be set before ventilation can be activated.
- Parameter changes must be logged with the responsible clinician and clinical rationale.
- A configuration cannot be activated without a valid patient interface specification.

## Entities

**Weaning Protocol**: Tracks the structured weaning plan including readiness assessment criteria, spontaneous breathing trial parameters (T-piece, low pressure support, or CPAP method), trial duration, success criteria, failure criteria, progression schedule, and weaning outcome history. Each weaning attempt is documented with duration, vital signs during the trial, patient tolerance, and result.

**Home Ventilation Program**: Manages the ongoing home ventilation arrangement for patients requiring long-term ventilatory support. Includes the prescribed device and settings, mask or interface specifications, usage expectations, supply schedule, troubleshooting guidance, follow-up appointment schedule, and telemetry monitoring parameters.

**Alarm Event Log**: Records ventilator alarm events with timestamp, alarm type, triggering parameter values, response taken, and responding clinician. This entity provides an audit trail for alarm management.

## Value Objects

**VentilationMode**: Represents the operational mode with its defining characteristics (volume-targeted vs. pressure-targeted, mandatory vs. spontaneous vs. combined breaths).

**VentilatorParameters**: Captures the complete parameter set for the current mode. Self-validates parameter ranges and flags clinically unsafe combinations (e.g., high tidal volume with high plateau pressure).

**AlarmThreshold**: Specifies the monitored parameter, threshold value, alarm priority (high, medium, low), and the expected clinical response.

**WeaningReadinessScore**: Captures the assessment of weaning readiness including oxygenation adequacy (PaO2/FiO2 ratio), hemodynamic stability, neurological status, sedation level, respiratory drive, and rapid shallow breathing index (RSBI < 105).

**DeviceTelemetry**: Represents a snapshot of real-time ventilator data including delivered tidal volume, actual respiratory rate, minute ventilation, peak pressure, plateau pressure, compliance, and resistance.

## Domain Events

**VentilationStarted**: Published when ventilatory support is initiated. Carries the patient ID, modality, mode, and initial parameters.

**VentilatorParametersAdjusted**: Published when ventilator settings are changed. Carries the old and new parameter values, clinician ID, and rationale.

**VentilatorAlarmTriggered**: Published when an alarm condition occurs. Carries the alarm type, triggering values, severity, and timestamp. Consumed by clinical alerting and monitoring systems.

**WeaningTrialStarted**: Published when a spontaneous breathing trial begins. Carries the trial parameters and patient baseline status.

**WeaningTrialCompleted**: Published when an SBT concludes. Carries the outcome (pass/fail), trial duration, vital signs during the trial, and any complications.

**WeaningCompleted**: Published when the patient is successfully liberated from ventilatory support. Consumed by the Pulmonary Rehabilitation Context to initiate post-ventilation rehabilitation.

**HomeVentilationPrescribed**: Published when a patient is transitioned to home ventilation. Carries the device specifications, settings, and follow-up plan.

## Domain Services

**WeaningReadinessAssessmentService**: Evaluates multiple clinical criteria to determine whether a patient is ready to attempt a spontaneous breathing trial. Considers respiratory mechanics, gas exchange, hemodynamic stability, and neurological status.

**VentilatorParameterValidationService**: Validates proposed parameter changes for clinical safety, checking tidal volume limits, pressure limits, FiO2-PEEP consistency, and other safety constraints.

**DeviceNormalizationService**: Translates manufacturer-specific device data into the context's unified parameter model, enabling the system to work with ventilators from multiple manufacturers.

## Integration Points

- **Shared Kernel with Airway Management Context**: Common types for airway devices and patient interfaces.
- **Upstream**: Receives AirwayEstablished events from the Airway Management Context.
- **Downstream**: Publishes WeaningCompleted events consumed by the Pulmonary Rehabilitation Context.
- **External**: Interfaces with ventilator devices through manufacturer APIs via an anti-corruption layer and open host service.

## References

- Evans, E. (2003). *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media.
- Fan, E., et al. (2017). *An Official ATS/ESICM/SCCM Clinical Practice Guideline: Mechanical Ventilation in Adult Patients with ARDS*. American Journal of Respiratory and Critical Care Medicine.
- Boles, J. M., et al. (2007). *Weaning from Mechanical Ventilation*. European Respiratory Journal.
