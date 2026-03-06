# Clinical/EMR Context in Hospital Management

## Overview

The Clinical/EMR (Electronic Medical Record) Context is the core bounded context of the hospital system. It manages clinical encounters, medical orders (laboratory, imaging, pharmacy), clinical notes, results, treatment plans, and diagnoses. This context directly supports patient care — the primary mission of the hospital.

## Core Responsibilities

- **Encounter Management**: Creating and managing clinical encounters (inpatient, outpatient, emergency) from start to completion
- **Order Management**: Placing, tracking, and resulting laboratory tests, imaging studies, and medication orders
- **Clinical Documentation**: Recording clinical notes, assessments, progress notes, and discharge summaries
- **Treatment Planning**: Defining and tracking care plans, clinical pathways, and treatment goals
- **Medication Management**: Prescribing medications, checking interactions, and reconciling medication lists
- **Clinical Decision Support**: Alerting providers to allergies, drug interactions, abnormal results, and evidence-based recommendations

## Aggregates

### Encounter Aggregate

- **Root**: Encounter (identified by EncounterID)
- **Internal entities**: ClinicalNote, Order (Lab, Imaging, Pharmacy)
- **Value objects**: DiagnosisCode (ICD-10), VitalSigns, ChiefComplaint, DischargeDisposition
- **Types**: Inpatient, Outpatient, Emergency, Observation
- **States**: Started → InProgress → PendingDischarge → Completed
- **Invariants**:
  - Must have an attending provider
  - Orders only placed on active encounters
  - Discharge requires all critical results reviewed
  - At least one diagnosis assigned before completion

### Order Aggregate (alternative design — separate from Encounter)

In some designs, Orders are separate aggregates referenced by EncounterID:

- **Root**: Order (identified by OrderID)
- **Value objects**: OrderType, Priority (stat, urgent, routine), ClinicalIndication, SpecimenType
- **States**: Ordered → Collected/Accepted → Processing → Resulted → Reviewed
- **Invariants**:
  - Must have a valid ordering provider with appropriate privileges
  - Stat orders must be acknowledged within defined timeframe
  - Results must be reviewed by an authorized provider

## Domain Events

| Event | Trigger | Consumers |
|-------|---------|-----------|
| EncounterStarted | New encounter begins | Billing (opens account) |
| VitalsRecorded | Vital signs documented | Clinical alerts (abnormal detection) |
| OrderPlaced | New lab/imaging/pharmacy order | Lab system, Radiology, Pharmacy |
| LabResultReceived | Lab result available | Ordering provider notification |
| CriticalResultFlagged | Result outside critical range | Immediate provider alert |
| PrescriptionIssued | New medication prescribed | Pharmacy, Drug interaction check |
| DiagnosisAssigned | Diagnosis code added | Billing (charge capture) |
| EncounterCompleted | Encounter closed | Billing (claim generation) |
| DischargeInstructionsIssued | Discharge paperwork created | Patient (notifications) |

## Domain Services

### ClinicalDecisionSupportService

Evaluates clinical data in real-time to provide evidence-based recommendations:

- Drug-allergy checking against patient's allergy list
- Drug-drug interaction detection across active medications
- Duplicate order detection
- Age/weight-based dosage validation
- Clinical pathway recommendations based on diagnosis

### MedicationReconciliationService

Compares medications across care transitions (admission, transfer, discharge):

- Identifies discrepancies between home medications and inpatient orders
- Flags medications that should be held, continued, or adjusted
- Produces reconciled medication list for discharge

## Integration with Other Contexts

### Patient → Clinical/EMR (Customer-Supplier)

Clinical context consumes patient identity, allergies, and medical history from the Patient context. Maintains a local PatientSummary projection.

### Scheduling → Clinical/EMR (Customer-Supplier)

AppointmentCheckedIn events trigger outpatient encounter creation. Provider assignments flow from scheduling to clinical.

### Clinical/EMR → Billing (Customer-Supplier with ACL)

Clinical events (EncounterCompleted, DiagnosisAssigned, OrderPlaced) flow to Billing through an anti-corruption layer that translates clinical concepts into billing codes (CPT, ICD-10).

### Emergency → Clinical/EMR (Partnership)

Emergency encounters transition to clinical inpatient encounters upon admission. Both teams coordinate on the handoff workflow.

### External Lab Systems → Clinical/EMR (Open Host Service)

External laboratory systems send results via HL7 v2 ORU messages or FHIR DiagnosticReport resources. An anti-corruption layer translates these into domain LabResult entities.

## Healthcare Standards Integration

- **FHIR Resources**: Encounter, DiagnosticReport, MedicationRequest, Observation, Condition
- **HL7 v2 Messages**: ORM (orders), ORU (results), ADT (admission/discharge/transfer)
- **Code Systems**: ICD-10 (diagnoses), CPT (procedures), LOINC (lab tests), SNOMED CT (clinical findings), RxNorm (medications)

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) — The core bounded context of the hospital system
- [Aggregates](../aggregates/) — Encounter is the central aggregate in this context
- [Domain Events](../domain-events/) — Clinical events drive billing, alerting, and interoperability
- [Healthcare Standards](../healthcare-standards/) — FHIR, HL7, ICD-10, LOINC are integral to this context
- [Anti-Corruption Layer](../anti-corruption-layer/) — Translates between external lab/pharmacy systems and clinical model
- [Event-Driven Architecture](../event-driven-architecture/) — Clinical workflows are inherently event-driven

## References

- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 2. Addison-Wesley, 2013.
- HL7 International. "HL7 FHIR R4 Specification." https://hl7.org/fhir/
- HL7 International. "HL7 Version 2 Product Suite." https://www.hl7.org/implement/standards/product_brief.cfm?product_id=185
- Wager, Karen A., Lee, Frances W., & Glaser, John P. "Health Care Information Systems: A Practical Approach for Health Care Management." Jossey-Bass, 2017.
- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003.
