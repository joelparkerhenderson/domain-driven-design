# Entities in Ophthalmology

## Overview

Entities are domain objects with a unique identity that persists over time even as
their attributes change. In the ophthalmology domain, entities represent clinical
objects that must be tracked throughout their lifecycle, such as a specific imaging
study, a particular surgical case, or an individual injection administration. The
identity of an entity is what distinguishes it from all other objects of the same
type, regardless of how its attributes evolve.

## Entity Characteristics

Entities in the ophthalmology domain share these characteristics:

- Unique identity: each entity has an identifier that is unique within its bounded
  context and remains constant throughout the entity's lifecycle.
- Mutable state: unlike value objects, entities can change their attributes over time.
  An Imaging Study entity transitions from ordered to acquired to interpreted.
- Lifecycle management: entities are created, modified, and sometimes deactivated or
  archived. Their lifecycle is governed by domain rules.
- Equality by identity: two entities are equal if and only if they share the same
  identity, regardless of their current attribute values.

## Entities by Bounded Context

### Clinical Examination Context

**ExaminationEncounter**: Represents a single clinical visit. Identified by encounter
ID. Attributes include encounter date, examining clinician, chief complaint, and
examination status (in-progress, completed, amended). The encounter's state changes
as findings are recorded and the examination is finalized.

**SlitLampFinding**: Represents a specific finding observed during slit lamp
biomicroscopy. Identified by finding ID. Attributes include anatomical structure
examined (cornea, anterior chamber, lens, vitreous), finding description, severity,
and laterality.

**TonometryReading**: Represents a specific IOP measurement. Identified by reading
ID. Attributes include measurement value, measurement method (Goldmann applanation,
non-contact, iCare), timestamp, and eye laterality.

### Diagnostic Imaging Context

**ImagingOrder**: Represents a request for a diagnostic imaging study. Identified by
order ID. Transitions through states: requested, scheduled, acquired, interpreted,
finalized. Tracks the ordering clinician, clinical indication, and priority.

**ImageSeries**: Represents a specific series within an imaging study. Identified by
series ID. Contains modality type, acquisition parameters, number of images, and
acquisition timestamp.

**ImagingInterpretation**: Represents a clinician's interpretation of imaging data.
Identified by interpretation ID. Contains findings, impressions, recommendations,
and interpreting clinician identity.

### Surgical Management Context

**SurgicalConsent**: Represents the informed consent documentation for a procedure.
Identified by consent ID. Tracks consent date, consenting clinician, risks discussed,
patient acknowledgment, and consent status.

**ProcedureRecord**: Represents the documentation of a performed surgical procedure.
Identified by procedure ID. Contains procedure type, surgeon, start and end times,
technique details, complications, and implant records.

**PostOperativeAssessment**: Represents a follow-up evaluation after surgery.
Identified by assessment ID. Tracks post-operative day, visual acuity, wound status,
IOP, and complication status.

### Refractive Services Context

**CandidacyDetermination**: Represents the clinical decision on a patient's suitability
for refractive surgery. Identified by determination ID. Tracks candidacy status
(suitable, unsuitable, deferred), rationale, and determining clinician.

**AblationProfile**: Represents the laser treatment parameters for a refractive
procedure. Identified by profile ID. Contains optical zone, ablation depth, treatment
algorithm, and nomogram adjustments.

### Glaucoma Management Context

**MedicationRecord**: Represents a specific glaucoma medication in a patient's
treatment regimen. Identified by record ID. Tracks medication name, class (e.g.,
prostaglandin analog, beta-blocker), dosage, frequency, start date, and adherence
notes.

**ProgressionAssessment**: Represents a clinical determination of glaucoma
progression. Identified by assessment ID. Contains assessment date, progression
status (stable, suspect, progressing), evidence basis, and management implications.

### Retinal Care Context

**InjectionAdministration**: Represents a single intravitreal injection event.
Identified by administration ID. Tracks injection date, medication administered,
dosage, administering clinician, eye laterality, and any immediate complications.

**ConditionEpisode**: Represents an episode of a retinal condition requiring active
management. Identified by episode ID. Tracks condition type (diabetic retinopathy,
AMD, retinal detachment), onset date, current severity, and management status.

## Entity Identity Strategies

In the ophthalmology domain, entity identity follows these strategies:

- System-generated UUIDs for internal entities (imaging studies, surgical cases).
- Natural identifiers where appropriate (encounter numbers from scheduling systems).
- Composite identifiers avoided in favor of single surrogate identifiers to simplify
  cross-context references.

## Entity Lifecycle Patterns

Entities follow common lifecycle patterns:

- State machine: entities like ImagingOrder and SurgicalCase transition through
  well-defined states with guarded transitions.
- Append-only history: entities like GlaucomaPatientProfile accumulate historical
  records without modifying past entries.
- Soft deletion: clinical entities are never physically deleted; they are marked as
  amended, cancelled, or superseded.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 5.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 5.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 5.
