# Action Planning Context - Asthma Domain

## Overview

The Action Planning Context is responsible for creating, managing, and maintaining personalized asthma action plans that guide patient self-management. This bounded context owns the models for the zone system (green, yellow, red), zone threshold definitions, self-management instructions, emergency protocols, and shared decision-making documentation. It is classified as a supporting subdomain.

Asthma action plans are the primary tool for translating clinical decisions into patient-understandable, actionable instructions. Research consistently demonstrates that written personalized action plans, when used in conjunction with regular clinical review, reduce emergency department visits, hospitalizations, and asthma-related mortality. The Action Planning Context bridges the professional clinical domain and the patient self-management domain.

## Key Concepts

**Asthma Action Plans** are personalized written documents developed collaboratively between the clinician and patient. Each plan contains three zones with specific instructions for daily management, symptom worsening response, and emergency actions. Plans include the patient's daily medications, rescue medication dosing, triggers to avoid, peak flow zone boundaries, and emergency contact information. Plans are reviewed and updated at every clinical visit or when treatment changes occur.

**Zone System** uses a traffic-light metaphor that patients can easily understand and act upon:

- **Green Zone (All Clear):** The patient is well-controlled. Symptoms are minimal, peak flow is at or above 80% of personal best, sleep is uninterrupted by asthma, and normal activities are possible. Instructions specify daily controller medication use, trigger avoidance measures, and criteria for recognizing transition to yellow zone.

- **Yellow Zone (Caution):** The patient is experiencing increased symptoms. Peak flow is between 50% and 80% of personal best, symptoms are increasing, nighttime awakening occurs, or rescue inhaler use increases. Instructions specify additional medication steps (increased ICS, addition of oral corticosteroids), monitoring frequency, and criteria for escalating to red zone or seeking medical contact.

- **Red Zone (Medical Alert):** The patient is in a medical emergency. Peak flow is below 50% of personal best, severe shortness of breath, rescue inhaler provides no relief, difficulty speaking in full sentences, or symptoms continue to worsen despite yellow zone interventions. Instructions specify emergency medication dosing (high-dose SABA, systemic corticosteroids), immediate emergency contact notification, and criteria for calling emergency services or going to an emergency department.

**Emergency Protocols** define the specific response pathway when a patient enters the red zone. The protocol includes sequential medication administration steps, time-based escalation triggers (for example, if no improvement after 15 minutes of rescue inhaler use), emergency contact list with phone numbers, and hospital emergency department preference. Emergency protocols are tailored to the patient's age, comorbidities, and geographic access to emergency care.

**Shared Decision-Making** documents the collaborative process by which clinicians and patients determine action plan parameters. Patients may have preferences regarding medication types, device preferences, comfort with self-escalation, and willingness to initiate oral corticosteroids without medical consultation. These preferences, along with the clinical rationale, are documented as part of the action plan.

## Aggregates

### AsthmaActionPlan

The central aggregate containing all three zone definitions, zone thresholds, medication instructions, emergency contacts, and review history. Invariants enforce that all three zones are defined with non-overlapping thresholds, that the plan has at least one emergency contact, and that medication instructions align with the current treatment plan.

### EmergencyProtocol

Encapsulates the emergency response pathway including sequential medication steps, time-based escalation triggers, emergency contacts, and hospital preference. Invariants ensure that every protocol includes instructions to seek emergency care and that contact information is complete.

## Entities

- **MedicationInstruction:** Zone-specific medication instructions written in patient-understandable language. Includes medication name, dose, frequency, administration method, and special instructions. Linked to a specific zone within the action plan.
- **SharedDecisionRecord:** Documentation of clinician-patient discussions regarding action plan choices, patient preferences, and clinical rationale for specific instructions.
- **PlanReviewRecord:** Documentation of periodic action plan reviews including whether modifications were made and the reasons.

## Value Objects

- **ZoneThreshold:** PEF boundaries (as percentage of personal best) and symptom criteria defining each zone.
- **PersonalBest:** The patient's personal best PEF value, date established, and conditions under which it was determined.
- **EmergencyContact:** Name, relationship, phone number, and priority order for emergency notification.
- **ZoneDefinition:** Complete specification of a zone including threshold criteria, medication instructions, and monitoring instructions.

## Domain Events Published

- **ActionPlanActivated:** Published when a new or revised action plan is activated for a patient. Consumed by Medication Management (verifies prescription alignment) and Outcomes Tracking (records plan activation date for effectiveness analysis).
- **ZoneEscalationDetected:** Published when patient-reported symptoms or PEF measurements indicate zone transition. Consumed by Treatment Management (may trigger urgent review) and Outcomes Tracking (records escalation event).
- **EmergencyProtocolActivated:** Published when a patient enters the red zone and the emergency protocol is initiated. Consumed by Outcomes Tracking (records emergency event) and Treatment Management (triggers post-emergency treatment review).

## Domain Events Consumed

- **AssessmentFinalized (from Clinical Assessment):** Updated PEF personal best or control level changes may require zone threshold revision.
- **SeverityReclassified (from Clinical Assessment):** Severity changes require action plan revision.
- **TreatmentPlanSteppedUp/Down (from Treatment Management):** Medication changes must be reflected in action plan medication instructions.
- **HighRiskExposureDetected (from Trigger Monitoring):** Environmental alerts may trigger precautionary zone awareness messaging.
- **TriggerProfileUpdated (from Trigger Monitoring):** New trigger identifications should be reflected in avoidance recommendations.

## Domain Services

- **ActionPlanGenerationService:** Generates zone thresholds from personal best PEF, translates treatment plan medications into patient-facing instructions, and validates plan consistency with current clinical data.
- **PlanValidationService:** Validates that an action plan is current with the latest treatment plan, assessment results, and trigger profile. Detects outdated medication names, inconsistent zone thresholds, and missing components.

## Integration Points

- **Conformist to Clinical Assessment:** Zone thresholds are derived directly from clinical assessment data (personal best PEF, control level). The Action Planning Context adopts the Clinical Assessment model for these concepts without translation.
- **Published Language from Treatment Management:** Receives standardized treatment instructions that are translated into patient-facing action plan language.
- **Downstream from Trigger Monitoring:** Receives environmental alerts for precautionary patient notification. Customer-supplier relationship.
- **Upstream to Outcomes Tracking:** Plan activation and zone escalation events feed into outcome effectiveness analysis.

## Health Literacy Considerations

Action plans must be written at an appropriate health literacy level. The Action Planning Context applies the following principles:

- Medication instructions use plain language rather than clinical terminology.
- Zone descriptions use concrete, observable symptoms rather than clinical measures alone.
- Visual indicators (green, yellow, red colors) supplement textual instructions.
- Instructions are action-oriented: "Take 2 puffs of your blue inhaler" rather than "Administer 200mcg salbutamol via MDI."
- Emergency instructions are unambiguous: "Call 911" rather than "Seek emergency medical attention."

## Business Rules

1. All three zones must be defined before an action plan can be activated.
2. Zone thresholds must be based on the patient's personal best PEF, not predicted PEF.
3. Zone boundaries must be non-overlapping and contiguous (green lower = yellow upper, yellow lower = red upper).
4. Red zone instructions must always include "seek emergency care" as an action item.
5. Action plans must be reviewed at minimum every 12 months or whenever treatment changes occur.
6. Emergency protocols must include at least one emergency contact with a verified phone number.
7. Medication instructions in the action plan must match current active prescriptions.
8. Shared decision-making must be documented when patients decline recommended action plan components.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- Global Initiative for Asthma (GINA). Global Strategy for Asthma Management and Prevention, 2023. Chapter on self-management education.
- Gibson et al. Written Action Plans for Asthma. Cochrane Database of Systematic Reviews, 2004.
- National Asthma Education and Prevention Program (NAEPP). Expert Panel Report 4, 2020. Self-management education section.
