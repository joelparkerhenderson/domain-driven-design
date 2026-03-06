# Pharmacy & Medication Context in Medical Practice

## Overview

The Pharmacy & Medication Context is the bounded context responsible for managing prescriptions, drug interaction checking, formulary management, medication administration records, controlled substance tracking, and pharmacy dispensing workflows. This context works in close partnership with the Clinical Workflow context, validating medication safety at the point of prescribing and managing the dispensing lifecycle.

## Core Responsibilities

- **Prescription Management**: Receiving, validating, and tracking prescriptions from creation through dispensing and refills
- **Drug Interaction Checking**: Screening for drug-drug, drug-allergy, drug-condition, and drug-food interactions
- **Formulary Management**: Maintaining lists of covered medications with tier classifications and prior authorization requirements
- **Medication Administration**: Recording when and how medications are given to patients (inpatient/clinical settings)
- **Controlled Substance Tracking**: Managing DEA-regulated medications with prescriber validation and quantity tracking
- **Medication Reconciliation**: Comparing and reconciling medication lists during transitions of care
- **Refill Management**: Processing prescription renewals with appropriate clinical validation

## Aggregates

### Prescription Aggregate

- **Aggregate Root**: Prescription (identified by PrescriptionID)
- **Value Objects**: Medication (RxNorm code, NDC, brand/generic name), Dosage (amount, unit, route, frequency, duration, PRN indicator), Quantity, RefillAuthorization (number of refills, last refill date), PrescriberInfo (NPI, DEA number)
- **Invariants**: Controlled substances (schedules II-V) require a valid DEA number; dosage must be within safe ranges defined by drug reference; refills cannot exceed maximum allowed by regulation; schedule II substances cannot have refills

### MedicationAdministrationRecord Aggregate

- **Aggregate Root**: MedicationAdministrationRecord (identified by MAR_ID)
- **Value Objects**: AdministeredDose, AdministrationTime, Route, AdministeredBy, PatientResponse
- **Invariants**: Administration must reference a valid active prescription; time cannot be in the future; administering nurse must be identified

### FormularyEntry Aggregate

- **Aggregate Root**: FormularyEntry (identified by FormularyEntryID)
- **Value Objects**: FormularyTier (1-4), CoverageRules, PriorAuthorizationCriteria, QuantityLimits, StepTherapyRequirements
- **Invariants**: Each medication has at most one formulary entry per insurance plan; tier assignment must be valid

## Domain Events

| Event | Trigger | Consumers |
|-------|---------|-----------|
| PrescriptionIssued | New prescription created | Pharmacy dispensing |
| DrugInteractionDetected | Interaction screening finds issue | Clinical Workflow (alert) |
| MedicationDispensed | Pharmacy fills prescription | Patient Records, Clinical |
| MedicationAdministered | Medication given to patient | Patient Records |
| ControlledSubstancePrescribed | Schedule II-V drug prescribed | Regulatory monitoring, PDMP |
| PrescriptionDiscontinued | Medication stopped | Patient Records, Clinical |
| RefillProcessed | Refill dispensed | Patient Records |
| FormularyUpdated | Formulary list changed | Clinical (affects prescribing) |

## Domain Services

### DrugInteractionService

Evaluates potential interactions by cross-referencing the proposed medication against the patient's active medications, allergies, and conditions. Uses external drug databases (via ACL) for interaction data. Returns a list of DrugInteraction value objects with severity and clinical recommendations.

### MedicationReconciliationService

Compares medication lists from different sources (current prescriptions, discharge summary, referral) and produces a reconciled list with provider-reviewed decisions for each medication (continue, modify, discontinue).

## Integration with Other Contexts

### Pharmacy <-> Clinical Workflow (Partnership)

The Clinical Workflow context sends medication orders; the Pharmacy context validates them and sends back interaction alerts. Both contexts share a published language for medication concepts (RxNorm codes, dosage formats).

### Pharmacy <- Patient Records (Customer-Supplier)

The Pharmacy context subscribes to AllergyRecorded and ProblemListUpdated events to maintain current allergy and condition data for interaction screening.

### Pharmacy <-> External Drug Databases (ACL)

Drug interaction databases, formulary services, and PDMPs expose proprietary APIs. An anti-corruption layer translates between external drug identifiers and internal RxNorm/NDC codes.

### Pharmacy <-> Insurance & Claims (Customer-Supplier)

The Insurance context provides formulary and prior authorization data. The Pharmacy context checks coverage before dispensing.

## Controlled Substance Handling

Controlled substances require additional domain rules:

- **Schedule II**: No refills permitted; new prescription required each time; 90-day maximum supply
- **Schedules III-V**: Up to 5 refills within 6 months of issue date
- **PDMP Reporting**: Each dispensing event must be reported to the state Prescription Drug Monitoring Program
- **DEA Validation**: Prescriber must have a valid, active DEA registration for the substance schedule

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) -- Pharmacy & Medication is one of six medical bounded contexts
- [Clinical Workflow Context](../clinical-workflow-context/) -- Partner for medication prescribing and safety
- [Patient Records Context](../patient-records-context/) -- Source of allergy and condition data
- [Anti-Corruption Layer](../anti-corruption-layer/) -- ACLs translate external drug database formats
- [Value Objects](../value-objects/) -- RxNormCode, NDCCode, Dosage, DrugInteraction are key value objects
- [Domain Services](../domain-services/) -- DrugInteractionService and MedicationReconciliationService
- [Regulatory Compliance](../regulatory-compliance/) -- DEA controlled substance regulations

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
- NLM. "RxNorm Overview." https://www.nlm.nih.gov/research/umls/rxnorm/
- FDA. "National Drug Code Directory." https://www.fda.gov/drugs/drug-approvals-and-databases/national-drug-code-directory
- DEA. "Title 21 Code of Federal Regulations." https://www.deadiversion.usdoj.gov/
- HL7 International. "FHIR R4 MedicationRequest Resource." https://hl7.org/fhir/medicationrequest.html
