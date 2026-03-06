# Glaucoma Management Context

## Overview

The Glaucoma Management Context is a supporting bounded context in the ophthalmology
domain that addresses the chronic, longitudinal management of glaucoma. This context
owns the models for intraocular pressure monitoring, visual field progression analysis,
optic nerve imaging tracking, medical therapy management, and glaucoma surgical
intervention referral. Glaucoma management is inherently longitudinal, requiring the
tracking and analysis of clinical data over months and years to detect progression
and guide treatment decisions.

## Scope

This context covers:

- Intraocular pressure (IOP) monitoring and trend analysis, including diurnal
  variation assessment and target IOP determination.
- Visual field progression analysis using trend-based and event-based methods to
  detect worsening of glaucomatous visual field loss.
- Optic nerve head and retinal nerve fiber layer (RNFL) imaging tracking using
  OCT data to monitor structural progression.
- Medical therapy management including medication selection, dosage optimization,
  adherence monitoring, and side effect tracking across multiple drug classes
  (prostaglandin analogs, beta-blockers, alpha-agonists, carbonic anhydrase
  inhibitors, rho-kinase inhibitors).
- Laser treatment management including selective laser trabeculoplasty (SLT),
  argon laser trabeculoplasty (ALT), and laser peripheral iridotomy (LPI).
- Surgical intervention referral when medical and laser therapy are insufficient,
  including referral criteria for trabeculectomy, tube shunt, and MIGS procedures.
- Glaucoma risk factor assessment and suspect monitoring.

## Aggregates

**GlaucomaPatientProfile** is the root aggregate maintaining a longitudinal record
of a patient's glaucoma history. It contains IOP measurement series, visual field
test references, optic nerve imaging references, medication history, laser treatment
history, and progression assessment records. The aggregate enforces the invariant
that progression assessments reference adequate baseline data and that treatment
modifications are documented with clinical rationale.

**TreatmentPlan** is an aggregate specifying the current glaucoma treatment strategy.
It contains the active medication regimen, target IOP, follow-up schedule, and
escalation criteria. The treatment plan is versioned; each modification creates a
new version while preserving the history of previous plans.

## Key Entities

- MedicationRecord: a specific medication in the treatment regimen with dosage,
  frequency, and adherence data.
- ProgressionAssessment: a clinical determination of whether glaucoma is progressing,
  stable, or indeterminate.
- LaserTreatmentRecord: documentation of a laser procedure for glaucoma.

## Key Value Objects

- IntraocularPressure: IOP measurement with method and CCT correction.
- TargetIOP: clinically determined pressure target with rationale.
- CupToDiscRatio: optic nerve cup-to-disc measurement (horizontal and vertical).
- VisualFieldIndex: summary percentage of remaining visual field function.
- MeanDeviation: global visual field sensitivity measure in decibels.
- MedicationClass: classification of glaucoma medications by mechanism of action.

## Domain Events Published

- IOPRecorded: signals a new IOP measurement added to the patient profile.
- GlaucomaProgressionDetected: signals that analysis has determined the patient's
  glaucoma is progressing. Consumed by Surgical Management to evaluate surgical
  intervention candidacy.
- TreatmentPlanModified: signals a change in the glaucoma treatment strategy.
- GlaucomaSuspectIdentified: signals that a patient under monitoring has met
  criteria for glaucoma diagnosis.

## Domain Events Consumed

- ExaminationCompleted (from Clinical Examination): provides IOP readings and optic
  nerve assessment findings for incorporation into the longitudinal profile.
- ImagingStudyInterpreted (from Diagnostic Imaging): provides OCT RNFL thickness
  data and visual field test results for progression analysis.
- SurgicalCaseCompleted (from Surgical Management): provides outcome data from
  glaucoma surgical interventions for post-surgical IOP tracking.

## Domain Services

- ProgressionAnalysisService: applies statistical methods to longitudinal data to
  determine progression status.
- TreatmentEscalationService: evaluates treatment adequacy and recommends changes.
- TargetIOPCalculationService: calculates appropriate target IOP based on clinical
  factors.

## Business Rules

- A minimum of five reliable visual field tests is required before trend-based
  progression analysis can be performed.
- IOP measurements must record the measurement method because different methods have
  different accuracy profiles and are not directly comparable without correction.
- Target IOP must be set for every glaucoma patient and must be reviewed at least
  annually or whenever progression is detected.
- Medication changes must document the clinical rationale (insufficient IOP reduction,
  adverse effects, adherence issues, or cost concerns).
- Progression assessment must consider both structural (OCT RNFL, optic nerve) and
  functional (visual field) evidence; structure-function correlation strengthens
  progression determination.
- Referral for surgical intervention requires documented failure of maximum tolerated
  medical therapy or demonstrated progression despite medical treatment at target IOP.

## Integration Points

- Downstream from Clinical Examination: receives IOP and optic nerve data.
- Downstream from Diagnostic Imaging: receives OCT and visual field data.
- Upstream to Surgical Management: refers patients for glaucoma surgery.
- External: integrates with pharmacy systems for medication management.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- American Academy of Ophthalmology. *Primary Open-Angle Glaucoma PPP*. 2020.
- European Glaucoma Society. *Terminology and Guidelines for Glaucoma*. 5th Edition, 2020.
