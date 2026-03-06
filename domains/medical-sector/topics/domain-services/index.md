# Domain Services in Medical Practice

## Overview

A domain service is a stateless operation that belongs to the domain but does not naturally fit within a single entity or value object. Domain services encapsulate business logic that spans multiple aggregates or requires coordination between different parts of the domain model. In medical practice, domain services handle operations such as drug interaction checking, clinical decision support evaluation, claims generation, and eligibility verification.

## Why Domain Services Matter in Medicine

Medical practice involves complex operations that cross aggregate boundaries:

- Checking drug interactions requires knowledge of all active medications, new prescriptions, and allergy records across multiple aggregates
- Generating a claim from an encounter requires data from the encounter, patient demographics, insurance policy, and procedure codes
- Evaluating clinical decision support rules requires patient history, current medications, lab results, and evidence-based guidelines
- Verifying insurance eligibility requires patient identity, insurance policy, and communication with external payer systems

These operations do not belong to any single aggregate because they require information from multiple sources and coordinate actions across boundaries.

## Key Domain Services in the Medical Domain

### DrugInteractionService (Pharmacy & Medication Context)

- **Purpose**: Evaluate potential drug-drug, drug-allergy, and drug-condition interactions before a prescription is finalized
- **Inputs**: Proposed medication (RxNorm code, dosage), patient's active medication list, patient's allergy list, patient's problem list
- **Outputs**: List of DrugInteraction value objects with severity, clinical effect, and recommendation
- **Consumers**: Clinical Workflow context (at order entry), Pharmacy context (at dispensing)
- **Why a service**: Interaction checking requires data from the Prescription aggregate, PatientRecord aggregate, and external drug databases

### ClinicalDecisionSupportService (Clinical Workflow Context)

- **Purpose**: Evaluate evidence-based rules and generate alerts to support clinical decision-making
- **Inputs**: Patient demographics, active problems, current medications, pending orders, relevant lab results
- **Outputs**: List of CDS alerts with severity, recommendation, and supporting evidence references
- **Examples**: Duplicate order detection, age-appropriate screening reminders, contraindicated medication alerts
- **Why a service**: CDS evaluation spans patient records, encounter data, medications, and lab results

### ClaimGenerationService (Insurance & Claims Context)

- **Purpose**: Transform a completed encounter into a properly coded insurance claim
- **Inputs**: Completed encounter with diagnoses and procedures, patient demographics, insurance policy, provider NPI
- **Outputs**: Claim aggregate with populated service lines, diagnosis codes, and billing provider information
- **Why a service**: Claim generation requires data from Patient Records, Clinical Workflow, and Insurance contexts

### EligibilityVerificationService (Insurance & Claims Context)

- **Purpose**: Verify a patient's insurance coverage before or at the time of service
- **Inputs**: Patient identity, insurance policy details, planned services
- **Outputs**: CoverageStatus value object with eligibility, copay, deductible, and coverage details
- **Why a service**: Eligibility verification communicates with external payer systems through an anti-corruption layer

### MedicationReconciliationService (Pharmacy & Medication Context)

- **Purpose**: Compare and reconcile a patient's medication lists across care settings during transitions of care
- **Inputs**: Patient's current medication list, medications from referral source or discharge summary, provider review decisions
- **Outputs**: Reconciled medication list with continue, modify, and discontinue decisions
- **Why a service**: Reconciliation spans multiple medication sources and requires clinical review coordination

### ReferralRoutingService (Clinical Workflow Context)

- **Purpose**: Determine the appropriate specialist or facility for a patient referral based on clinical need, insurance network, and availability
- **Inputs**: Referral request, patient insurance network, specialist availability, clinical urgency
- **Outputs**: Routed referral with assigned specialist, scheduled appointment, and authorization status
- **Why a service**: Routing considers insurance network rules, provider availability, and clinical requirements

## Domain Service Design Principles

- **Stateless**: Domain services do not hold state between operations; all required data is passed as parameters
- **Named using domain language**: Service names and method names come from the ubiquitous language (checkDrugInteractions, not processData)
- **Pure domain logic**: Domain services contain no infrastructure concerns (no database calls, no HTTP requests); they use repositories and anti-corruption layers for data access
- **Not a catch-all**: Only create a domain service when the operation genuinely spans multiple aggregates; prefer placing logic on entities or value objects when possible

## Relationships to Other Topics

- [Aggregates](../aggregates/) -- Domain services coordinate operations across multiple aggregates
- [Entities](../entities/) -- Domain services operate on entities but do not replace entity behavior
- [Value Objects](../value-objects/) -- Domain services accept and return value objects
- [Repositories](../repositories/) -- Domain services use repositories to load aggregate roots
- [Anti-Corruption Layer](../anti-corruption-layer/) -- Domain services use ACLs to access external system data
- [Domain Events](../domain-events/) -- Domain services may trigger domain events as a result of their operations

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 5. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 7. Addison-Wesley, 2013.
- Millett, Scott & Tune, Nick. "Patterns, Principles, and Practices of Domain-Driven Design." Chapter 22. Wrox, 2015.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 6. O'Reilly, 2021.
