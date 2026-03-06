# Aggregates in Ophthalmology

## Overview

Aggregates are clusters of related domain objects treated as a single unit for the
purpose of data changes and consistency enforcement. Each aggregate has a root entity
that serves as the sole entry point for external access. In the ophthalmology domain,
aggregates encapsulate the clinical concepts that must maintain internal consistency
during operations such as recording an examination, interpreting an imaging study,
or planning a surgical procedure.

## Aggregate Design Principles

Ophthalmology aggregates follow these design principles:

- Each aggregate enforces its own invariants. For example, a Patient Examination
  aggregate ensures that all findings are associated with a valid eye laterality
  (OD, OS, or OU) and that visual acuity measurements use recognized notation.
- Aggregates are kept as small as practical. Large aggregates create contention and
  performance problems. Each aggregate contains only the objects necessary to enforce
  its consistency rules.
- Cross-aggregate references use identity rather than direct object references.
  A Surgical Case references a Patient Examination by its identifier, not by holding
  a direct reference to the examination object.
- Aggregates communicate changes through domain events. When a Patient Examination
  is completed, it publishes an ExaminationCompleted event rather than directly
  updating downstream aggregates.

## Aggregates by Bounded Context

### Clinical Examination Context

**PatientExamination**: The root aggregate for clinical encounters. Contains visual
acuity records, slit lamp findings, tonometry measurements, and refraction results
for a single visit. Enforces the invariant that each finding is attributed to a
specific eye (OD/OS) or both eyes (OU).

**RefractionResult**: A subordinate aggregate capturing the detailed refraction
prescription including sphere, cylinder, axis, add power, and pupillary distance.
Maintained separately because refraction may be performed independently of a full
examination.

### Diagnostic Imaging Context

**ImagingStudy**: The root aggregate representing a diagnostic imaging investigation.
Contains one or more imaging series, each composed of individual images or data sets.
Enforces consistency between study metadata, imaging modality, and interpretation
status.

**OCTScan**: A specialized aggregate for optical coherence tomography data, containing
scan protocol, layer segmentation data, and thickness measurements. Maintains the
invariant that scan parameters are consistent with the selected protocol.

**VisualFieldTest**: An aggregate capturing perimetric test data including stimulus
parameters, patient responses, reliability indices, and derived metrics (mean
deviation, pattern standard deviation).

### Surgical Management Context

**SurgicalCase**: The root aggregate representing the complete lifecycle of a surgical
procedure. Contains the operative plan, surgical consent, procedure record, and
post-operative assessments. Enforces the invariant that a case cannot proceed without
a valid operative plan and documented consent.

**OperativePlan**: An aggregate specifying the surgical approach, technique, implant
selection (e.g., IOL model and power for cataract surgery), and anticipated risk
factors. Immutable once the surgical case enters the operative phase.

### Refractive Services Context

**RefractiveCandidateAssessment**: An aggregate capturing pre-operative evaluation
data including corneal topography, pachymetry, wavefront aberrometry, and candidacy
determination. Enforces the invariant that candidacy decisions reference all required
measurements.

**RefractiveProcedure**: An aggregate tracking the refractive procedure and its
parameters (e.g., ablation profile for LASIK, lens model for ICL).

### Glaucoma Management Context

**GlaucomaPatientProfile**: A longitudinal aggregate maintaining the patient's
glaucoma history including IOP trends, visual field progression, optic nerve imaging
history, and treatment history. Enforces the invariant that progression assessments
reference adequate baseline data.

**TreatmentPlan**: An aggregate specifying the current glaucoma treatment strategy
including medication regimen, target IOP, and follow-up schedule.

### Retinal Care Context

**RetinalAssessment**: An aggregate capturing clinical and imaging findings specific
to retinal pathology, including disease classification, severity grading, and
treatment indication determination.

**InjectionProtocol**: An aggregate governing intravitreal injection scheduling,
medication selection, administration records, and response monitoring. Enforces
the invariant that injection intervals comply with the selected treatment regimen.

## Aggregate Boundaries and Transactions

Each aggregate defines a transactional boundary. All changes within an aggregate are
committed atomically. Changes across aggregates are eventually consistent, coordinated
through domain events. This design prevents long-running transactions that would lock
clinical data during complex multi-step workflows.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapters 5-6.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 10.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 5.
