# Surgical Management Context

## Overview

The Surgical Management Context is a core bounded context in the ophthalmology domain
that covers the complete perioperative lifecycle of ophthalmic surgical procedures.
This context owns the models for cataract surgery, vitrectomy, corneal transplant,
oculoplastic procedures, and glaucoma surgical interventions. It manages operative
planning, informed consent, procedure documentation, and post-operative care tracking,
representing one of the highest-value areas of the ophthalmology domain.

## Scope

This context covers:

- Cataract surgery including phacoemulsification, extracapsular cataract extraction,
  and femtosecond laser-assisted surgery with IOL implantation.
- Vitrectomy procedures including pars plana vitrectomy for retinal detachment,
  macular hole, epiretinal membrane, and vitreous hemorrhage.
- Corneal transplant procedures including penetrating keratoplasty, DSAEK, DMEK,
  and anterior lamellar keratoplasty.
- Oculoplastic procedures including blepharoplasty, ptosis repair, orbital surgery,
  and lacrimal surgery.
- Glaucoma surgical interventions including trabeculectomy, tube shunt implantation,
  and minimally invasive glaucoma surgery (MIGS).
- Pre-operative assessment and surgical planning.
- Informed consent documentation and management.
- Operative procedure documentation and surgical records.
- Post-operative follow-up and outcome tracking.

## Aggregates

**SurgicalCase** is the root aggregate representing the complete lifecycle of a surgical
procedure. It contains the operative plan, surgical consent, procedure record, and
post-operative assessments. The aggregate enforces the invariant that a case cannot
proceed to the operative phase without a valid operative plan and documented informed
consent. The case transitions through states: planned, consented, scheduled, in-progress,
completed, and closed.

**OperativePlan** is an aggregate specifying the surgical approach for a procedure. For
cataract surgery, this includes the IOL model and power, incision location, and surgical
technique. For vitrectomy, it includes gauge size, tamponade agent, and approach. The
operative plan becomes immutable once the surgical case enters the in-progress state.

## Key Entities

- SurgicalConsent: informed consent documentation with risks discussed and patient
  acknowledgment.
- ProcedureRecord: documentation of the performed surgery including technique,
  duration, findings, and complications.
- PostOperativeAssessment: follow-up evaluation documenting recovery and outcomes.
- ImplantRecord: tracking of implanted devices (IOLs, tube shunts, corneal grafts)
  with specifications and lot numbers.

## Key Value Objects

- IOLSpecification: manufacturer, model, power, optic material, and haptic design.
- SurgicalRiskScore: calculated risk assessment with contributing factors.
- WoundArchitecture: incision type, location, width, and construction.
- ComplicationRecord: type, severity, management, and outcome of any complication.
- AnesthesiaRecord: anesthesia type (topical, local, general) and agents used.

## Domain Events Published

- SurgicalCaseScheduled: signals scheduling of a procedure.
- SurgicalCaseCompleted: signals completion of a surgical procedure with outcome data.
- PostOperativeAssessmentRecorded: signals a follow-up visit with recovery data.
- SurgicalComplicationReported: signals an intraoperative or post-operative complication.

## Domain Events Consumed

- ExaminationCompleted (from Clinical Examination): provides pre-operative clinical
  examination data for surgical planning.
- ImagingStudyInterpreted (from Diagnostic Imaging): provides biometry, topography,
  and other imaging data for operative planning.
- GlaucomaProgressionDetected (from Glaucoma Management): triggers evaluation for
  glaucoma surgical intervention.
- RetinalConditionDiagnosed (from Retinal Care): triggers evaluation for vitreoretinal
  surgical intervention.

## Domain Services

- IOLCalculationService: calculates IOL power using biometry data and selected formulas.
- SurgicalRiskAssessmentService: evaluates perioperative risk based on patient factors.
- SurgicalOutcomeAnalysisService: compares actual outcomes with predictions.

## Business Rules

- A surgical case requires a completed pre-operative examination within the preceding
  90 days (configurable by procedure type).
- Informed consent must be documented and acknowledged before the case can be scheduled.
- The operative plan must specify laterality (OD or OS) and include a laterality
  verification step to prevent wrong-site surgery.
- IOL selection for cataract surgery must be based on current biometry measurements
  (within 6 months) and must document the calculation formula used.
- All implanted devices must have complete traceability (manufacturer, model, serial
  number, lot number).
- Post-operative follow-up assessments must be recorded at defined intervals based on
  the procedure type (e.g., day 1, week 1, month 1 for cataract surgery).

## Integration Points

- Downstream from Clinical Examination: receives pre-operative clinical data.
- Downstream from Diagnostic Imaging: receives biometry, topography, and other
  pre-operative imaging data.
- Downstream from Glaucoma Management: receives referrals for glaucoma surgery.
- Downstream from Retinal Care: receives referrals for vitreoretinal surgery.
- Shared kernel with Refractive Services: perioperative management protocols.
- External: integrates with billing (CPT codes), scheduling, and EHR systems.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- American Academy of Ophthalmology. *Cataract in the Adult Eye PPP*. 2021.
- American Academy of Ophthalmology. *BCSC Section 11: Lens and Cataract*. 2023.
