# Medication Management Context

## Overview

The Medication Management Context handles all aspects of psychotropic
medication prescribing, dosage management, drug interaction safety, adherence
monitoring, and side effect tracking. This bounded context is classified as a
generic subdomain because psychotropic prescribing follows standardized
pharmacological protocols, drug interaction databases are commercially
available, and pharmacy integration patterns are well-established through
industry standards. However, the clinical safety requirements demand careful
modeling of invariants and validation rules.

## Purpose and Scope

This context answers the clinical question: what medications should the
patient take, at what doses, and are they safe and effective? It encompasses:

- Psychotropic medication prescribing including antidepressants (SSRIs, SNRIs,
  TCAs, MAOIs), anxiolytics (benzodiazepines, buspirone), antipsychotics
  (typical, atypical), mood stabilizers (lithium, valproate, lamotrigine),
  and stimulants (methylphenidate, amphetamine salts).
- Dosage titration with planned escalation or tapering schedules.
- Drug interaction checking against the patient's current medication list,
  including interactions with non-psychotropic medications.
- Adherence monitoring to identify patients who may not be taking medications
  as prescribed.
- Side effect tracking and assessment of medication tolerability.
- Medication discontinuation with appropriate taper planning for medications
  that require gradual reduction (e.g., SSRIs, benzodiazepines).

## Ubiquitous Language

Within this context, the following terms have precise meanings:

- Prescription: a medication order specifying the drug, dosage, route,
  frequency, and duration, issued by an authorized prescriber.
- Titration: the systematic adjustment of medication dosage to achieve
  therapeutic effect while minimizing side effects.
- Drug Interaction: a clinically significant effect that occurs when two
  or more medications are taken concurrently.
- Adherence: the degree to which a patient takes medication as prescribed.
- Taper Plan: a gradual dosage reduction schedule for safe medication
  discontinuation.
- Formulary: the list of medications covered by the patient's insurance
  plan, with tiered cost-sharing.

## Aggregate: Prescription

The Prescription aggregate is the root of this context. It encapsulates:

- PrescriptionId (unique identity)
- Patient reference (by identity)
- Prescribing clinician reference
- Medication (MedicationIdentifier value object)
- Current dosage (Dosage value object)
- Titration schedule (TitrationSchedule entity, if applicable)
- Prescribing rationale
- Drug interaction check results (DrugInteraction value objects)
- Monitoring requirements (lab tests, vital signs, symptom checks)
- Side effects reported
- Adherence status
- Prescription status (active, on-hold, discontinued, expired)
- Discontinuation reason and taper plan (if applicable)

Invariants enforced by the aggregate:

- Drug interaction checks must pass before a new prescription is activated.
  Contraindicated interactions block activation entirely; major interactions
  require documented clinical override.
- Dosage changes must follow the titration schedule if one exists.
- Medications requiring laboratory monitoring (e.g., lithium levels, hepatic
  panels for valproate) must have monitoring orders documented.
- Discontinuation of medications with withdrawal potential (benzodiazepines,
  SSRIs, SNRIs) must include a taper plan.
- Only authorized prescribers can issue or modify prescriptions.

## Domain Events Published

- MedicationPrescribed: signals a new medication order. Consumed by Outcomes
  Measurement for treatment tracking.
- DosageAdjusted: signals a dosage change during titration or clinical
  response adjustment.
- MedicationDiscontinued: signals medication stoppage with reason and taper
  plan if applicable.
- DrugInteractionDetected: signals a clinically significant interaction.
  May trigger urgent clinician review.
- AdherenceConcernIdentified: signals that adherence monitoring has detected
  a potential issue. May trigger outreach or clinical follow-up.

## Domain Services

- DrugInteractionCheckService: evaluates potential interactions against the
  patient's full medication list.
- TitrationPlanningService: creates titration schedules based on medication
  pharmacokinetics and clinical guidelines.

## Integration Points

- Upstream: consumes treatment directives from the Treatment Planning Context
  when pharmacotherapy is part of the treatment plan.
- Lateral: coordinates with the Crisis Intervention Context for emergency
  medication review during crisis episodes. Coordinates with Therapy
  Management to monitor medication effects reported during therapy sessions.
- Downstream: publishes medication events to the Outcomes Measurement Context.
- External: integrates with pharmacy benefit managers for formulary checking
  and electronic prescribing through NCPDP SCRIPT. Integrates with drug
  interaction databases (e.g., First Databank, Lexicomp) through anti-
  corruption layers.

## Clinical Standards

This context implements:

- FDA prescribing information and black box warnings for psychotropic
  medications.
- APA Practice Guidelines for pharmacotherapy of specific disorders.
- Clinical Pharmacogenomics Implementation Consortium (CPIC) guidelines
  for pharmacogenomic-guided prescribing.
- Beers Criteria for potentially inappropriate medications in older adults.
- NCPDP SCRIPT standard for electronic prescribing.

## Regulatory Considerations

- Controlled substance prescribing (benzodiazepines, stimulants) must comply
  with DEA regulations and state Prescription Drug Monitoring Program (PDMP)
  requirements.
- Electronic prescribing of controlled substances (EPCS) must meet DEA
  identity proofing and authentication requirements.
- Medication errors must be reported per institutional and state requirements.
- Informed consent for medication must be documented, including risks,
  benefits, and alternatives.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- Stahl, Stephen M. Stahl's Essential Psychopharmacology. Cambridge University Press, 2021.
- National Council for Prescription Drug Programs. NCPDP SCRIPT Standard. NCPDP, 2023.
