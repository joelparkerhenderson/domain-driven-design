# Clinical Assessment Context

## Overview

The Clinical Assessment Context is the bounded context responsible for the systematic
evaluation of patients with musculoskeletal conditions. It encompasses the patient
history, physical examination, imaging interpretation, and diagnostic classification
that form the foundation for all subsequent orthopaedic treatment decisions.

## Scope

This context covers:

- Patient presentation and chief complaint documentation.
- Musculoskeletal physical examination including inspection, palpation, range of motion,
  strength testing, and special tests.
- Imaging ordering and result interpretation (radiography, MRI, CT, ultrasound).
- Application of diagnostic classification systems.
- Formulation and recording of provisional and confirmed diagnoses.
- Referral to appropriate treatment contexts (surgical or conservative).

This context does not cover: operative planning, surgical procedures, implant
selection, or rehabilitation protocols.

## Ubiquitous Language

- Clinical Encounter: A single patient visit for musculoskeletal evaluation.
- Examination Finding: An objective observation from the physical examination.
- Special Test: A named provocative maneuver to assess a specific structure.
- Imaging Study: A radiographic or advanced imaging investigation and its findings.
- Provisional Diagnosis: The working clinical impression before confirmation.
- Classification: A standardized categorization applied to a condition.
- Referral: A directive sending the patient to a treatment context.

## Aggregate Root

**ClinicalEncounter** is the aggregate root. It ensures that all findings,
imaging results, and diagnoses within a single patient visit remain consistent.
A diagnosis cannot be recorded without supporting examination findings. An imaging
study reference must correspond to the correct patient and anatomical region.

## Key Entities

- ClinicalEncounter: Tracks a single visit through scheduled, in-progress, completed.
- ImagingStudyReference: Tracks a specific imaging order and its resulting findings.

## Key Value Objects

- RangeOfMotion: Joint motion measured in degrees for each plane of movement.
- MuscleStrength: MRC grade (0-5) for a specific muscle group.
- GoniometryReading: A single angular measurement at a specific joint.
- SpecialTestResult: The named test and its positive/negative outcome.
- Laterality: Left, right, or bilateral.
- AnatomicalSite: Bone, segment, and side combined.
- KellgrenLawrenceGrade: Osteoarthritis severity grade (0-4).

## Domain Events Published

- PatientAssessed: Emitted when the clinical encounter is completed.
- ImagingStudyCompleted: Emitted when imaging results are available.
- DiagnosisConfirmed: Emitted when a diagnosis transitions from provisional to confirmed.

## Invariants

- An encounter must have at least one examination finding before a diagnosis is recorded.
- Imaging references must match the patient and anatomical region of the encounter.
- Classification codes must be valid for the diagnosed condition type.
- A completed encounter cannot be modified; amendments create a new encounter record.

## Integration Points

- Upstream: PACS (via anti-corruption layer) for imaging data.
- Upstream: EHR/HIS (via anti-corruption layer) for patient demographics.
- Downstream: Publishes events consumed by Surgical Planning, Fracture Management,
  and Sports Medicine contexts.

## Clinical Workflow

1. Patient presents with a musculoskeletal complaint.
2. Clinician records history and performs physical examination.
3. Examination findings are documented as value objects within the encounter.
4. Imaging is ordered if indicated; results are linked to the encounter.
5. Classification systems are applied to formalize the diagnosis.
6. The encounter is completed, publishing a PatientAssessed event.
7. A referral directs the patient to the appropriate treatment context.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- American Academy of Orthopaedic Surgeons. Clinical Practice Guidelines. https://www.aaos.org
