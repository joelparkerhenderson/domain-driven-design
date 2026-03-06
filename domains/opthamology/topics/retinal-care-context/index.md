# Retinal Care Context

## Overview

The Retinal Care Context is a supporting bounded context in the ophthalmology domain
that manages the diagnosis, treatment, and longitudinal monitoring of retinal
conditions. This context owns the models for diabetic retinopathy screening and
management, age-related macular degeneration (AMD) treatment, retinal detachment
assessment, and intravitreal injection protocols. Retinal care involves complex
treatment regimens that require careful monitoring of treatment response and
disease activity over extended periods.

## Scope

This context covers:

- Diabetic retinopathy screening and classification using established grading systems
  (ETDRS, International Clinical Diabetic Retinopathy Severity Scale).
- Diabetic macular edema assessment and treatment including intravitreal anti-VEGF
  therapy, intravitreal corticosteroids, and laser photocoagulation.
- Age-related macular degeneration management including classification (dry/wet),
  monitoring of geographic atrophy, and anti-VEGF treatment for neovascular AMD.
- Retinal detachment assessment including classification (rhegmatogenous, tractional,
  exudative) and referral for surgical intervention.
- Intravitreal injection protocol management including medication selection, dosing
  regimens (fixed interval, PRN, treat-and-extend), scheduling, and response monitoring.
- Retinal vascular disorders including retinal vein occlusion and retinal artery
  occlusion.
- Vitreoretinal interface disorders including epiretinal membrane and macular hole.
- Treatment response evaluation using visual acuity, OCT measurements, and clinical
  examination findings.

## Aggregates

**RetinalAssessment** is an aggregate capturing clinical and imaging findings specific
to retinal pathology. It contains disease classification, severity grading, treatment
indication determination, and treatment response evaluation. The aggregate enforces
the invariant that severity classifications follow recognized grading systems and that
treatment decisions are documented with clinical rationale.

**InjectionProtocol** is an aggregate governing the intravitreal injection treatment
regimen. It contains the medication selection, dosing schedule, treatment regimen
type (fixed, PRN, treat-and-extend), administration records, and response monitoring
data. The aggregate enforces the invariant that injection intervals comply with the
selected treatment regimen and that treatment response assessments are performed at
required intervals.

## Key Entities

- ConditionEpisode: an episode of a retinal condition requiring active management.
- InjectionAdministration: a single intravitreal injection event.
- TreatmentResponseRecord: evaluation of response to treatment at a specific time point.
- LaserTreatmentRecord: documentation of retinal laser photocoagulation.

## Key Value Objects

- DRSeverityGrade: diabetic retinopathy severity classification.
- AMDClassification: AMD type, stage, and feature characterization.
- InjectionDose: medication, concentration, and volume for intravitreal injection.
- CentralMacularThickness: OCT-derived macular thickness measurement.
- TreatmentResponse: classification of response (improved, stable, worsened).
- DiseaseActivityMarker: clinical or imaging indicator of active disease.

## Domain Events Published

- RetinalConditionDiagnosed: signals identification of a new retinal condition
  requiring management. Consumed by Surgical Management when surgical intervention
  is indicated (e.g., retinal detachment requiring vitrectomy).
- InjectionAdministered: signals completion of an intravitreal injection.
- TreatmentResponseEvaluated: signals assessment of treatment effectiveness with
  pre- and post-treatment comparison data.
- TreatmentRegimenModified: signals a change in injection protocol or treatment
  approach.

## Domain Events Consumed

- ExaminationCompleted (from Clinical Examination): provides retinal findings from
  clinical examination for correlation with imaging and treatment data.
- ImagingStudyInterpreted (from Diagnostic Imaging): provides OCT macular thickness,
  fundus photography, and fluorescein angiography results for treatment monitoring.
- SurgicalCaseCompleted (from Surgical Management): provides outcome data from
  vitreoretinal surgical procedures.

## Domain Services

- RetinalSeverityClassificationService: applies established grading systems to
  classify retinal condition severity.
- InjectionSchedulingService: determines injection timing based on treatment protocol
  and response assessment.
- TreatmentResponseAssessmentService: evaluates treatment response by comparing
  pre- and post-treatment metrics.

## Business Rules

- Diabetic retinopathy screening is required at least annually for all diabetic
  patients; more frequently for patients with established retinopathy.
- Severity classification must use a recognized grading system (ETDRS or International
  Clinical scale) and must be documented with supporting clinical and imaging evidence.
- Anti-VEGF injection protocols must specify the regimen type (fixed, PRN, or
  treat-and-extend) and document the criteria for treatment extension or reduction.
- Treatment response must be evaluated at every injection visit using visual acuity
  and OCT measurements at minimum.
- Retinal detachment assessment must classify the type (rhegmatogenous, tractional,
  exudative) and document the extent, location, and macular status (macula-on or
  macula-off) because these factors determine surgical urgency.
- Switching anti-VEGF agents requires documented inadequate response to the current
  agent with a minimum treatment trial (typically 3 injections).
- All intravitreal injections must document laterality, medication, dose, and the
  absence of immediate post-injection complications.

## Integration Points

- Downstream from Clinical Examination: receives retinal examination findings.
- Downstream from Diagnostic Imaging: receives OCT, fundus photography, and
  angiography results.
- Upstream to Surgical Management: refers patients for vitreoretinal surgery.
- External: integrates with pharmacy systems for anti-VEGF medication management
  and with laboratory systems for HbA1c monitoring in diabetic patients.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- American Academy of Ophthalmology. *Diabetic Retinopathy PPP*. 2019.
- American Academy of Ophthalmology. *Age-Related Macular Degeneration PPP*. 2019.
- ETDRS Research Group. *Early Treatment Diabetic Retinopathy Study*. Archives of Ophthalmology.
