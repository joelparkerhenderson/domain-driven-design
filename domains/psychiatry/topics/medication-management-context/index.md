# Medication Management Context

## Overview

The Medication Management Context is a core bounded context in the psychiatry domain responsible for managing the complete lifecycle of psychopharmacological treatment. This context owns the workflows for prescribing psychiatric medications, monitoring therapeutic drug levels, checking drug interactions, incorporating pharmacogenomic data into prescribing decisions, and assessing polypharmacy risk. It consumes diagnostic information from the Diagnostic Assessment Context and publishes medication events to Outcomes Measurement and Crisis Management contexts.

## Responsibility

This context answers the clinical question: "What medications should this patient take, at what doses, and how should they be monitored?" It encompasses all activities from initial medication selection through ongoing management, dose adjustment, and discontinuation. The context does not diagnose conditions; it receives diagnostic information and translates it into pharmacological treatment decisions.

## Ubiquitous Language

Within this context, the following terms have specific meanings:

- **Prescription**: A clinician's formal order for a specific psychiatric medication, including all parameters needed to dispense and administer the drug.
- **Medication Regimen**: The complete set of active psychiatric medications for a patient, representing the holistic pharmacological treatment plan.
- **Titration**: The systematic process of adjusting medication dosage in defined steps to reach the optimal therapeutic dose while minimizing side effects.
- **Therapeutic Range**: The blood concentration range within which a medication produces therapeutic effects without unacceptable toxicity.
- **Polypharmacy**: The concurrent use of three or more psychotropic medications, requiring heightened attention to cumulative risk.
- **Metabolizer Status**: A patient's genetically determined rate of drug metabolism for specific enzymes, guiding dose personalization.

## Aggregates

### Prescription (Root Aggregate)

The central aggregate representing a single medication order with its full lifecycle.

Components:
- Medication identity (drug name, RxNorm code, drug class)
- Dosage specification (quantity, unit, route, frequency)
- Prescribing clinician reference and DEA number (for controlled substances)
- Indication (diagnosis reference from Diagnostic Assessment Context)
- Titration schedule (if applicable): step doses, step durations, target dose
- Monitoring requirements: laboratory tests, frequency, therapeutic ranges
- Black box warning acknowledgments
- Prescription status (draft, active, on hold, tapered, discontinued)
- Status change history with reasons

Invariants:
- Controlled substance prescriptions must include verified DEA registration.
- Dosage must be within FDA-approved ranges unless a clinical override is documented with justification.
- A prescription cannot be activated if a critical (contraindicated-severity) drug interaction exists without documented clinical justification.
- Titration schedules must specify the target dose and maximum titration rate.

### Medication Regimen (Root Aggregate)

Maintains the patient's complete active medication list and aggregate risk profile.

Components:
- Active prescription references
- Polypharmacy risk score and contributing factors
- Anticholinergic burden score
- QTc prolongation risk assessment
- Metabolic syndrome risk factors
- Last regimen review date and next review due date
- Regimen status (active, under review, simplified)

Invariants:
- Polypharmacy risk must be recalculated when prescriptions are added or removed.
- Regimens with three or more psychotropics must be reviewed at least every 90 days.
- Anticholinergic burden score exceeding threshold must trigger clinical alert.

## Value Objects

- **Dosage**: Quantity, unit, frequency, route of administration.
- **Therapeutic Range**: Medication, lower bound, upper bound, measurement unit.
- **Drug Interaction**: Drug pair, severity, mechanism, clinical recommendation.
- **Metabolizer Status**: Gene, phenotype, clinical implication.
- **Side Effect Profile**: Reported side effects, severity, onset, relationship to medication.
- **Prior Authorization Status**: Authorization request, insurer response, approval conditions.

## Domain Events

- **Medication Prescribed**: New prescription created and activated.
- **Dosage Adjusted**: Existing prescription dosage modified (titration step or clinical adjustment).
- **Medication Discontinued**: Prescription ended with documented reason.
- **Adverse Event Reported**: Clinically significant side effect or adverse reaction documented.
- **Drug Interaction Detected**: Significant interaction identified in the medication regimen.
- **Therapeutic Level Out of Range**: Drug monitoring result outside therapeutic range.
- **Polypharmacy Alert Triggered**: Regimen risk score exceeds defined threshold.

## Domain Services

- **Drug Interaction Checking Service**: Evaluates proposed medications against current regimen and diagnoses.
- **Polypharmacy Risk Assessment Service**: Calculates cumulative risk across the complete regimen.
- **Pharmacogenomic Recommendation Service**: Integrates genetic data with prescribing decisions per CPIC guidelines.

## Clinical Workflow

1. Diagnostic information received from the Diagnostic Assessment Context triggers medication evaluation.
2. Prescribing clinician reviews current medication regimen and patient characteristics.
3. Drug interaction checking service evaluates proposed medication against current regimen.
4. Pharmacogenomic profile is consulted if available, adjusting dose or drug selection.
5. Prescription is created with dosage, monitoring requirements, and titration schedule.
6. Prescription undergoes required approvals (controlled substance verification, prior authorization).
7. Active prescription is added to the medication regimen and regimen risk is recalculated.
8. Ongoing monitoring tracks therapeutic levels, side effects, and clinical response.
9. Dosage adjustments are made based on monitoring data and clinical response.
10. Discontinuation follows appropriate tapering protocols when clinically indicated.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- Stahl, Stephen M. *Stahl's Essential Psychopharmacology*. Cambridge University Press, 2021.
- Clinical Pharmacogenetics Implementation Consortium. "CPIC Guidelines." https://cpicpgx.org/.
- American Psychiatric Association. "Practice Guideline for the Treatment of Patients with Major Depressive Disorder." APA, 2010.
