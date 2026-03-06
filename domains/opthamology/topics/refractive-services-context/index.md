# Refractive Services Context

## Overview

The Refractive Services Context is a generic bounded context in the ophthalmology
domain that manages elective refractive correction procedures. This context owns
the models for LASIK, PRK, implantable collamer lens (ICL) procedures, and the
comprehensive pre-operative and post-operative management specific to refractive
surgery. As a generic subdomain, refractive services follows well-established
protocols with high standardization across practices.

## Scope

This context covers:

- LASIK (Laser-Assisted In Situ Keratomileusis) including femtosecond flap creation,
  excimer laser ablation, and flap repositioning.
- PRK (Photorefractive Keratectomy) including epithelial removal, excimer laser
  surface ablation, and contact lens bandage management.
- Implantable lens procedures including phakic IOL and implantable collamer lens
  (ICL) insertion for high refractive errors.
- Pre-operative candidacy evaluation including corneal topography, pachymetry,
  wavefront aberrometry, pupillometry, and tear film assessment.
- Informed consent specific to elective refractive procedures.
- Post-operative management including visual recovery tracking, complication
  monitoring, and enhancement assessment.
- Outcome tracking and refractive accuracy analysis.

## Aggregates

**RefractiveCandidateAssessment** is an aggregate capturing the comprehensive
pre-operative evaluation for refractive surgery candidacy. It contains corneal
topography data, pachymetry measurements, wavefront aberrometry results, tear film
assessment, and the candidacy determination. The aggregate enforces the invariant
that a candidacy determination must reference all required measurements and that
candidacy criteria thresholds are applied correctly.

**RefractiveProcedure** is an aggregate tracking the refractive procedure and its
parameters. For LASIK, this includes flap parameters, ablation profile, optical zone,
and treatment algorithm. For PRK, it includes ablation profile and mitomycin-C
application. For ICL, it includes lens model, power, and vault measurement. The
aggregate maintains the association between treatment parameters and post-operative
outcomes.

## Key Entities

- CandidacyDetermination: the clinical decision on surgical suitability.
- AblationProfile: laser treatment parameters for corneal reshaping.
- EnhancementRecord: documentation of any retreatment procedure.
- PostRefractiveCareRecord: follow-up tracking specific to refractive surgery.

## Key Value Objects

- CornealTopography: keratometry values, astigmatism axis, pattern classification.
- Pachymetry: central corneal thickness and thinnest point location.
- WavefrontAberrometry: Zernike coefficients and higher-order aberration data.
- ResidualStromalBed: calculated remaining corneal thickness after ablation.
- PredictedRefraction: expected post-operative refractive outcome.
- PupilDiameter: scotopic and photopic pupil measurements.

## Domain Events Published

- RefractiveCandidacyDetermined: signals a candidacy decision (suitable, unsuitable,
  or deferred) with rationale.
- RefractiveProcedureCompleted: signals completion of a refractive procedure with
  treatment parameters.
- RefractiveEnhancementRecommended: signals that a retreatment is being considered
  based on post-operative refractive outcome.

## Domain Events Consumed

- RefractionRecorded (from Clinical Examination): provides current refraction data
  for candidacy assessment comparison.
- ImagingStudyInterpreted (from Diagnostic Imaging): provides corneal topography
  and other pre-operative imaging data.

## Domain Services

- CandidacyEvaluationService: evaluates patient suitability against candidacy
  criteria for each procedure type.
- NomogramAdjustmentService: applies surgeon-specific treatment modifications based
  on historical outcome analysis.

## Business Rules

- Refractive surgery candidacy requires stable refraction (less than 0.50 diopter
  change over 12 months).
- Minimum corneal thickness must be verified: residual stromal bed must exceed
  250 micrometers for LASIK; total corneal thickness must meet procedure-specific
  minimums for PRK and ICL.
- Patients with certain conditions (keratoconus, significant dry eye, autoimmune
  disease) are contraindicated for corneal refractive surgery.
- Informed consent for elective refractive surgery must document specific risks
  including under/over-correction, dry eye, halos/glare, and rare complications.
- Post-operative follow-up is required at defined intervals: day 1, week 1, month 1,
  month 3, month 6, and month 12 for LASIK and PRK.
- Enhancement procedures require a minimum waiting period after the primary procedure
  (typically 3-6 months) and confirmation of refractive stability.

## Integration Points

- Downstream from Clinical Examination: receives refraction and examination data.
- Downstream from Diagnostic Imaging: receives corneal topography and wavefront data.
- Shared kernel with Surgical Management: perioperative management protocols, consent
  documentation standards, and sterile technique protocols.
- External: integrates with patient scheduling, billing, and laser device systems.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- American Academy of Ophthalmology. *Refractive Errors & Refractive Surgery PPP*. 2017.
- Azar, Dimitri T., and Douglas D. Koch. *LASIK: Fundamentals, Surgical Techniques, and Complications*. CRC Press, 2002.
