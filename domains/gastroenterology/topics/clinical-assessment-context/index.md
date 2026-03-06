# Clinical Assessment Context

## Overview

The Clinical Assessment Context is the primary entry point for patients in the
gastroenterology domain. It owns the initial and ongoing evaluation of patients
presenting with gastrointestinal complaints, including symptom documentation, physical
examination, laboratory study ordering and interpretation, and imaging referrals.
This context establishes the clinical foundation upon which all other bounded contexts
build their specialized evaluations.

## Scope and Responsibilities

This context is responsible for:

- Documenting the patient's chief complaint and history of present illness.
- Performing and recording structured review of systems for gastrointestinal symptoms.
- Conducting and documenting physical examination findings.
- Ordering laboratory studies and integrating results into the clinical assessment.
- Generating imaging referrals with appropriate clinical indications.
- Identifying alarm features that require urgent diagnostic workup.
- Formulating differential diagnoses based on available clinical evidence.
- Creating referrals to specialized contexts (endoscopy, IBD, hepatology, motility, nutrition).

## Core Aggregates

### PatientEncounter

The central aggregate of this context. The PatientEncounter captures the full scope of
a clinical visit from patient check-in through documentation and sign-off. It contains
the chief complaint, review of systems responses, physical examination findings,
clinical impressions, and the encounter plan. The encounter transitions through states:
scheduled, checked-in, in-progress, documented, and signed.

Key invariant: a completed encounter must contain at minimum a chief complaint, at
least one clinical finding, and a documented assessment and plan.

### LabOrder

Manages laboratory test ordering and result tracking. A LabOrder is created within the
context of a PatientEncounter but has its own lifecycle because results arrive
asynchronously. The aggregate tracks ordered tests, specimen collection status, and
individual test results.

Key invariant: a lab order cannot transition to completed status until all ordered
tests have received results.

## Key Entities

- ClinicalAssessment: structured evaluation with findings and clinical impression.
- PhysicalExamination: documented findings from abdominal and related examination.
- ImagingReferral: referral to radiology with clinical indication and urgency.

## Key Value Objects

- ChiefComplaint: the patient's primary presenting symptom in their own words.
- ReviewOfSystems: structured inventory of symptom presence or absence by organ system.
- VitalSigns: blood pressure, heart rate, temperature, and other measurements.
- LabResult: individual test result with value, unit, reference range, and flag.
- AlarmFeature: categorized alarm feature type with associated clinical urgency.

## Domain Events

- AssessmentCompleted: signals that a clinical assessment has been finalized.
- AlarmFeatureIdentified: triggers urgent diagnostic pathways across contexts.
- LabResultReceived: notifies downstream contexts of new laboratory data.
- ImagingReferralCreated: initiates radiology coordination.

## Clinical Workflows

### Initial Evaluation Workflow

A new patient presents with gastrointestinal symptoms. The encounter captures the
chief complaint, collects a thorough review of systems, documents physical examination
findings, and generates an initial differential diagnosis. Based on the assessment,
the clinician may order labs, refer for imaging, or generate a referral to a
specialized context.

### Alarm Feature Workflow

When the assessment identifies an alarm feature (dysphagia, GI bleeding, unexplained
weight loss, iron deficiency anemia, new-onset symptoms after age 50), the
AlarmFeatureIdentified event is raised. This event triggers prioritized scheduling
in the Endoscopic Procedures Context and may generate urgent imaging referrals.

### Follow-Up Assessment Workflow

Patients returning for follow-up have their previous encounter data available for
comparison. The follow-up assessment documents interval changes, reviews new lab
results, and adjusts the clinical plan. Changes in disease status may generate
events consumed by specialized contexts.

## Integration Points

- Upstream: receives patient demographic data through anti-corruption layer from EHR.
- Downstream: provides referrals and clinical data to all five specialized contexts.
- External: sends lab orders to laboratory information systems and receives results.
- External: sends imaging referrals to radiology systems.

## Business Rules

- Alarm features must be flagged regardless of the clinician's clinical impression.
- Lab orders for specific panels follow standardized ordering protocols.
- Imaging referrals require documented clinical indication and prior authorization check.
- Follow-up intervals are determined by diagnosis severity and guideline recommendations.

## References

- Evans, E. (2003). *Domain-Driven Design*. Addison-Wesley. Chapters 5-6: Entities, Value Objects, Aggregates.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley. Chapter 10: Aggregates.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media. Chapter 8: Domain Model Patterns.
- ACG Clinical Guidelines for various GI conditions (American College of Gastroenterology).
