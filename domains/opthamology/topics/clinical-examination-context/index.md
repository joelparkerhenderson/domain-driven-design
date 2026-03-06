# Clinical Examination Context

## Overview

The Clinical Examination Context is a core bounded context in the ophthalmology domain
that encompasses all activities related to the comprehensive ophthalmologic examination.
This context owns the models for visual acuity testing, slit lamp biomicroscopy,
tonometry, refraction, and the overall clinical encounter. It is the primary entry
point for patient data into the ophthalmology system and is upstream to most other
bounded contexts.

## Scope

This context covers:

- Comprehensive eye examinations including history-taking, external examination,
  pupil assessment, motility evaluation, and confrontation visual fields.
- Visual acuity testing using Snellen, LogMAR, or other standardized methods,
  including uncorrected, best-corrected, and pinhole acuity.
- Slit lamp biomicroscopy for evaluation of the anterior segment (cornea, anterior
  chamber, iris, lens) and posterior segment (vitreous, retina, optic nerve).
- Tonometry for intraocular pressure measurement using Goldmann applanation,
  non-contact, or other validated methods.
- Refraction including manifest refraction, cycloplegic refraction, and
  determination of best-corrected visual acuity.
- Clinical assessment and diagnosis formulation.

## Aggregates

**PatientExamination** is the root aggregate. It encapsulates all findings from a
single clinical encounter. It contains visual acuity records for each eye, slit lamp
findings organized by anatomical structure, tonometry readings, and the clinician's
assessment and plan. The aggregate enforces that all findings are attributed to a
specific eye laterality and that the examination status follows a valid lifecycle
(in-progress, completed, amended).

**RefractionResult** is a separate aggregate capturing the detailed refraction
prescription. It includes sphere, cylinder, axis, add power, pupillary distance,
and vertex distance. Refraction may be performed independently of a full examination
(e.g., optical shop refraction), justifying its separation as a distinct aggregate.

## Key Entities

- ExaminationEncounter: tracks the visit lifecycle and metadata.
- SlitLampFinding: individual observations from biomicroscopy.
- TonometryReading: individual IOP measurements with method and timing.

## Key Value Objects

- VisualAcuity: measurement with notation system and correction status.
- RefractionPrescription: sphere, cylinder, axis, add, and PD.
- IntraocularPressure: pressure value with method and CCT correction.
- EyeLaterality: OD, OS, or OU designation.
- AnteriorSegmentFinding: structured finding from slit lamp evaluation.

## Domain Events Published

- ExaminationCompleted: signals finalization of a clinical encounter.
- RefractionRecorded: signals completion of a refraction.
- AbnormalFindingDetected: signals identification of a clinically significant
  abnormality requiring downstream action.

## Domain Events Consumed

- PostOperativeAssessmentRecorded (from Surgical Management): triggers correlation
  of post-operative findings with pre-operative examination data.
- ImagingStudyInterpreted (from Diagnostic Imaging): allows clinicians to review
  imaging results in the context of examination findings.

## Domain Services

- ExaminationCompletionService: validates examination completeness.
- ReferralDeterminationService: analyzes findings for subspecialty referral needs.

## Business Rules

- Every examination finding must specify eye laterality (OD, OS, or OU).
- Visual acuity must be recorded for both eyes at every comprehensive examination.
- IOP must be measured and recorded with the measurement method identified.
- Examination status transitions follow the sequence: in-progress, completed, amended.
  An examination cannot revert from completed to in-progress.
- Abnormal findings exceeding defined thresholds (e.g., IOP above 21 mmHg, suspicious
  optic nerve appearance) must generate an AbnormalFindingDetected event.

## Integration Points

- Upstream to Diagnostic Imaging: examination findings drive imaging orders.
- Upstream to Surgical Management: examination data informs operative planning.
- Upstream to Glaucoma Management: IOP and optic nerve findings feed glaucoma profiles.
- Upstream to Retinal Care: retinal findings trigger retinal assessments.
- Downstream from Surgical Management: receives post-operative assessment data.
- External: integrates with EHR systems through anti-corruption layer.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- American Academy of Ophthalmology. *Comprehensive Adult Medical Eye Evaluation PPP*. 2020.
