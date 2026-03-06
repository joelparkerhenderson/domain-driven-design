# Emergency Services Context in Hospital Management

## Overview

The Emergency Services Context manages the emergency department (ED) operations: triage, rapid assessment, trauma activation, bed management, capacity tracking, and surge protocols. This context operates under extreme time pressure and must support fast, reliable workflows where delays directly impact patient outcomes.

## Core Responsibilities

- **Triage**: Assessing and prioritizing patients using the Emergency Severity Index (ESI)
- **Trauma Management**: Activating trauma protocols, coordinating trauma teams, managing resuscitation bays
- **Bed and Capacity Management**: Tracking ED bed availability, managing patient flow, monitoring boarding times
- **Rapid Assessment**: Supporting fast clinical evaluation with immediate access to critical patient data
- **Surge Management**: Activating disaster and mass casualty protocols when ED capacity is exceeded
- **ED-to-Inpatient Transition**: Managing the handoff when ED patients are admitted to inpatient care

## Aggregates

### EmergencyEncounter Aggregate

- **Root**: EmergencyEncounter (identified by ED EncounterID)
- **Internal entities**: TriageAssessment
- **Value objects**: ESILevel, ChiefComplaint, ArrivalMode (ambulance, walk-in, transfer), AcuityScore, DispositionDecision
- **States**: Arrived → Triaged → InTreatment → PendingDisposition → Admitted / Discharged / Transferred
- **Invariants**:
  - All patients must be triaged within defined time thresholds
  - ESI-1 patients must be seen immediately
  - Disposition decision must be documented by attending ED physician
  - Patients cannot be discharged without documented reassessment

### TraumaActivation Aggregate

- **Root**: TraumaActivation (identified by TraumaID)
- **Value objects**: TraumaLevel (Level I, Level II), ActivationCriteria, TeamComposition, ETA
- **States**: Activated → TeamAssembled → PatientArrived → Assessment → Stabilized → Disposition
- **Invariants**:
  - Trauma level determines required team composition
  - Resuscitation bay must be confirmed available before patient arrival
  - All required team members must acknowledge activation

## Domain Events

| Event | Trigger | Consumers |
|-------|---------|-----------|
| PatientArrivedAtED | Patient presents to ED | Triage queue |
| TriageCompleted | Triage assessment done | Bed assignment, treatment queue |
| TraumaActivated | Trauma criteria met | Trauma team, OR, blood bank |
| BedAssigned | ED bed allocated | Patient tracking board |
| PatientAdmittedFromED | Disposition: admit | Clinical/EMR (inpatient encounter), Billing, Bed Management |
| PatientDischargedFromED | Disposition: discharge | Patient (instructions), Clinical |
| EDCapacityAlert | Occupancy exceeds threshold | Administration, diversion protocols |
| SurgeProtocolActivated | Mass casualty or extreme volume | All departments, administration |

## Emergency Severity Index (ESI)

The ESI is a five-level triage algorithm used to prioritize patients:

| ESI Level | Category | Description | Response Time |
|-----------|----------|-------------|---------------|
| 1 | Resuscitation | Immediate life-threatening condition | Immediate |
| 2 | Emergent | High risk, confused/lethargic, severe pain | Minutes |
| 3 | Urgent | Multiple resources needed, stable vitals | 30 minutes |
| 4 | Less Urgent | One resource needed | 60 minutes |
| 5 | Non-Urgent | No resources needed | 120 minutes |

## Domain Services

### TriageAssessmentService

Evaluates presenting symptoms, vital signs, and medical history to assign an ESI level. Flags patients meeting trauma activation criteria or requiring immediate intervention.

### BedAssignmentService (ED-specific)

Assigns ED beds based on acuity, available resources, and current occupancy. Prioritizes resuscitation bays for ESI-1, monitors beds for ESI-2, and fast-track areas for ESI-4/5.

### SurgeCapacityManager

Monitors ED occupancy against thresholds. When capacity is exceeded, activates surge protocols: opening overflow areas, diverting ambulances, calling in additional staff, and suspending elective admissions.

## Integration with Other Contexts

### Patient → Emergency (Shared Kernel)

Emergency needs immediate access to life-critical patient data without event propagation delays. A minimal shared kernel provides real-time read access to allergies, blood type, and active medications by MRN.

### Emergency → Clinical/EMR (Partnership)

When an ED patient is admitted, the EmergencyEncounter transitions to an inpatient Encounter in the Clinical context. Both teams coordinate on:

- Clinical data handoff (triage assessment, orders placed in ED, results)
- Attending physician assignment
- Continuity of orders and medications

### Emergency → Billing (Indirect via Clinical)

ED charges flow through the Clinical context to Billing. Emergency-specific charges (trauma activation fees, facility fees based on ESI level) are captured as part of the encounter.

## Design Considerations

### Real-Time Requirements

The Emergency context has the strictest performance requirements:

- Triage data must be available within seconds
- Bed availability must be accurate in real-time
- Trauma activation notifications must reach the team within seconds
- Patient tracking board must reflect current state continuously

### Eventual Consistency Limitations

Unlike other contexts where eventual consistency is acceptable, certain Emergency operations require strong consistency:

- Bed assignment must be atomic (two patients cannot be assigned the same bed)
- Trauma activation must have confirmed acknowledgments
- ESI-1 patient identification must be immediate

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) — One of five hospital bounded contexts
- [Context Map](../context-map/) — Shared kernel with Patient, partnership with Clinical
- [Domain Events](../domain-events/) — ED events trigger cross-context workflows
- [Domain Services](../domain-services/) — Triage assessment and bed assignment are domain services
- [Aggregates](../aggregates/) — EmergencyEncounter and TraumaActivation are aggregate roots
- [Regulatory Compliance](../regulatory-compliance/) — EMTALA requires treatment regardless of ability to pay

## References

- Gilboy, N., Tanabe, T., Travers, D., & Rosenau, A.M. "Emergency Severity Index (ESI): A Triage Tool for Emergency Department Care." AHRQ Publication No. 12-0014. Agency for Healthcare Research and Quality, 2011.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 2. Addison-Wesley, 2013.
- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003.
- American College of Emergency Physicians. "EMTALA Fact Sheet." ACEP, 2019.
- HL7 International. "FHIR R4 Encounter Resource." https://hl7.org/fhir/encounter.html
