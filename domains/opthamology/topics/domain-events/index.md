# Domain Events in Ophthalmology

## Overview

Domain events represent significant occurrences within the domain that other parts of
the system may need to react to. In the ophthalmology domain, domain events capture
clinically meaningful state changes such as the completion of an examination, the
interpretation of an imaging study, or the scheduling of a surgical case. Domain
events enable loose coupling between bounded contexts by allowing downstream contexts
to react to upstream changes without direct dependencies.

## Domain Event Principles

Domain events in the ophthalmology domain follow these principles:

- Events represent facts that have already occurred. They are named in past tense
  (ExaminationCompleted, not CompleteExamination).
- Events are immutable. Once published, an event cannot be modified or retracted.
- Events carry sufficient data for consumers to act without querying the source.
- Events are part of the ubiquitous language and are named using clinical terminology.
- Events do not contain business logic. They are pure data carriers.

## Domain Events by Bounded Context

### Clinical Examination Context Events

**ExaminationCompleted**: Published when a comprehensive eye examination is finalized.
Carries encounter ID, patient ID, examining clinician, examination date, summary of
findings including visual acuity, IOP readings, and significant clinical observations.
Consumed by Diagnostic Imaging (to correlate with pending studies), Glaucoma Management
(to update IOP records), and Retinal Care (to trigger assessments for retinal findings).

**RefractionRecorded**: Published when a refraction is completed. Carries refraction
prescription details (sphere, cylinder, axis, add), visual acuity with correction,
and laterality. Consumed by Refractive Services for candidacy assessment tracking.

**AbnormalFindingDetected**: Published when a clinically significant abnormality is
identified during examination. Carries finding type, severity, laterality, and
recommended follow-up. Triggers appropriate downstream workflows.

### Diagnostic Imaging Context Events

**ImagingStudyOrdered**: Published when a new imaging study is requested. Carries
order ID, study type, clinical indication, ordering clinician, and priority level.

**ImagingStudyAcquired**: Published when imaging data acquisition is complete.
Carries study ID, modality, acquisition parameters, and technician identity.

**ImagingStudyInterpreted**: Published when a clinician's interpretation is finalized.
Carries study ID, findings summary, clinical impressions, and recommendations.
Consumed by Glaucoma Management (for OCT and visual field results), Retinal Care
(for OCT and angiography results), and Surgical Management (for biometry results).

### Surgical Management Context Events

**SurgicalCaseScheduled**: Published when a surgical procedure is scheduled. Carries
case ID, procedure type, scheduled date, surgeon, and surgical site (laterality).

**SurgicalCaseCompleted**: Published when a surgical procedure is finished. Carries
case ID, procedure performed, surgeon, duration, complications (if any), and implant
details. Consumed by Clinical Examination (for post-operative follow-up scheduling)
and billing systems (through ACL for CPT code generation).

**PostOperativeAssessmentRecorded**: Published after each post-operative visit.
Carries assessment date, post-operative day, visual acuity, IOP, wound status,
and complication status.

### Refractive Services Context Events

**RefractiveCandidacyDetermined**: Published when a candidacy decision is made.
Carries assessment ID, candidacy status (suitable, unsuitable, deferred), rationale,
and determining clinician.

**RefractiveProcedureCompleted**: Published when a refractive procedure is performed.
Carries procedure type, treatment parameters, and immediate outcome assessment.

### Glaucoma Management Context Events

**IOPRecorded**: Published when an IOP measurement is added to a patient's glaucoma
profile. Carries measurement value, method, laterality, and timestamp.

**GlaucomaProgressionDetected**: Published when analysis determines that a patient's
glaucoma is progressing. Carries patient ID, progression evidence (visual field
decline, RNFL thinning), and current treatment status. Triggers treatment plan
review and potential referral to Surgical Management for glaucoma surgery.

**TreatmentPlanModified**: Published when a glaucoma treatment plan is changed.
Carries previous and new treatment details, rationale, and modifying clinician.

### Retinal Care Context Events

**RetinalConditionDiagnosed**: Published when a new retinal condition is identified.
Carries condition type, severity classification, laterality, and recommended
management approach.

**InjectionAdministered**: Published when an intravitreal injection is given. Carries
injection ID, medication, dosage, laterality, and administering clinician.

**TreatmentResponseEvaluated**: Published when the response to retinal treatment is
assessed. Carries pre- and post-treatment measurements (visual acuity, OCT thickness),
response classification (improved, stable, worsened), and next steps.

## Event Flow Patterns

Common event flow patterns in the ophthalmology domain include:

- Examination-to-imaging: ExaminationCompleted triggers imaging orders.
- Imaging-to-treatment: ImagingStudyInterpreted triggers treatment plan updates
  in Glaucoma Management or Retinal Care.
- Progression-to-surgery: GlaucomaProgressionDetected triggers surgical referral.
- Treatment-to-follow-up: InjectionAdministered triggers follow-up scheduling.

## Event Versioning

Domain events are versioned to support backward compatibility as the domain model
evolves. Consumers must handle both current and previous event versions during
transition periods.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 8.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 8.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 6.
