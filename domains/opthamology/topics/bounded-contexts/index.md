# Bounded Contexts in Ophthalmology

## Overview

Bounded contexts define logical boundaries within which a specific domain model is
consistent and complete. In the ophthalmology domain, six bounded contexts partition
the system along natural clinical and operational lines. Each context owns its own
ubiquitous language, aggregates, entities, value objects, and domain events. The same
clinical concept may appear in multiple contexts but carries a different meaning and
representation in each.

## The Six Bounded Contexts

### 1. Clinical Examination Context

This context encompasses all activities related to the comprehensive ophthalmologic
examination. It owns the models for visual acuity testing, slit lamp biomicroscopy,
tonometry, and refraction. The Patient Examination aggregate is the central concept,
capturing all findings from a single clinical encounter. This context is upstream to
most other contexts because examination findings drive diagnostic, surgical, and
treatment decisions.

### 2. Diagnostic Imaging Context

This context manages the acquisition, storage, and interpretation of ophthalmic
images and functional tests. It owns models for optical coherence tomography (OCT),
fundus photography, fluorescein angiography, and visual field perimetry. The Imaging
Study aggregate captures the lifecycle of a diagnostic investigation from order through
acquisition to interpretation. This context produces reports consumed by the Clinical
Examination, Glaucoma Management, and Retinal Care contexts.

### 3. Surgical Management Context

This context covers the perioperative lifecycle of ophthalmic surgical procedures
including cataract surgery, vitrectomy, corneal transplant, and oculoplastic surgery.
The Surgical Case aggregate tracks a procedure from pre-operative assessment through
the operative event to post-operative recovery. It consumes examination findings and
imaging results from upstream contexts and produces surgical outcome data.

### 4. Refractive Services Context

This context manages elective refractive procedures such as LASIK, PRK, and
implantable lens surgery. The Refractive Candidate Assessment aggregate governs
patient eligibility determination based on corneal topography, pachymetry, and
ocular health evaluation. The Refractive Procedure aggregate tracks the procedure
and post-operative recovery with specific attention to visual outcome metrics.

### 5. Glaucoma Management Context

This context addresses the chronic management of glaucoma, encompassing intraocular
pressure monitoring, visual field progression analysis, medical therapy management,
and surgical interventions (trabeculectomy, tube shunts, MIGS). The Glaucoma Patient
Profile aggregate maintains a longitudinal record of IOP measurements, visual field
tests, optic nerve imaging, and treatment history to support progression analysis
and treatment decisions.

### 6. Retinal Care Context

This context manages retinal conditions including diabetic retinopathy, age-related
macular degeneration, retinal detachment, and conditions requiring intravitreal
injections. The Retinal Assessment aggregate captures clinical and imaging findings
specific to retinal pathology. The Injection Protocol aggregate governs the scheduling,
administration, and monitoring of intravitreal anti-VEGF therapy.

## Boundary Rationale

Context boundaries in the ophthalmology domain follow these principles:

- Clinical subspecialty alignment: each context maps to a recognized area of
  ophthalmologic practice with its own trained specialists.
- Workflow independence: each context can operate autonomously for its core functions
  without real-time dependencies on other contexts.
- Terminology precision: the same term may carry different implications in different
  contexts (e.g., "treatment plan" in Glaucoma Management versus Retinal Care).
- Data ownership: each context is the authoritative source for its own data, and other
  contexts receive data through domain events or queries.

## Cross-Context Concepts

Certain concepts span multiple contexts but are represented differently in each:

- Patient identity is shared across all contexts but is not owned by any single context;
  it belongs to an external identity management system.
- Eye laterality (OD/OS/OU) appears in every context as a value object but carries
  context-specific implications for clinical workflow.
- Clinical findings may originate in the Clinical Examination Context and be consumed
  by downstream contexts, but each context transforms them into its own internal model.

## Context Autonomy

Each bounded context is designed for maximum autonomy. Changes to the internal model
of one context should not force changes in other contexts. This autonomy is maintained
through well-defined integration points using domain events and anti-corruption layers.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 14.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 2.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapters 7-8.
- American Academy of Ophthalmology. *Preferred Practice Patterns*.
