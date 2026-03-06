# Joint Replacement Context

## Overview

The Joint Replacement Context is the bounded context responsible for managing
arthroplasty procedures, implant tracking, registry reporting, revision management,
and long-term outcome monitoring. This context owns the lifecycle of a joint
replacement from the moment of surgical implantation through decades of follow-up,
including potential revision surgery.

## Scope

This context covers:

- Recording arthroplasty procedures with implant component details.
- Tracking implant serial numbers, lot numbers, and manufacturers.
- Submitting data to national and institutional joint registries.
- Monitoring patient outcomes using validated scoring systems.
- Identifying cases that may require revision surgery.
- Managing revision history and linking revision cases to primary procedures.
- Supporting implant recall and safety notifications.

This context does not cover: pre-operative planning (Surgical Planning Context),
diagnostic assessment (Clinical Assessment Context), or post-operative therapy
(Rehabilitation Context).

## Ubiquitous Language

- Arthroplasty Case: A joint replacement from implantation through follow-up.
- Implant Component: A single physical part of the prosthetic joint.
- Bearing Surface: The articulating material pairing in the prosthesis.
- Primary Arthroplasty: The first joint replacement at a given anatomical site.
- Revision Arthroplasty: A subsequent surgery to modify, replace, or remove implants.
- Outcome Score: A validated patient-reported or clinician-reported functional measure.
- Registry Submission: The structured data report sent to a national joint registry.
- Implant Recall: A manufacturer-initiated withdrawal of a specific implant product.

## Aggregate Root

**ArthroplastyCase** is the aggregate root. It maintains consistency across implant
components, bearing surface selection, outcome score history, and revision records.
All component serial numbers must be recorded at surgery. Outcome scores must be
collected at defined follow-up intervals. Revision records must link back to the
primary case.

## Key Entities

- ArthroplastyCase: Tracks the joint replacement through its entire lifecycle.
- ImplantComponent: Tracks a specific physical implant part by serial number.

## Key Value Objects

- BearingSurfacePairing: The material combination (ceramic-on-poly, metal-on-poly).
- ImplantSize: Dimensional specification and catalog identifier.
- CementType: Brand, antibiotic loading, and viscosity.
- OxfordHipScore: Patient-reported outcome measure (0-48).
- OxfordKneeScore: Patient-reported outcome measure (0-48).
- WOMACScore: Pain, stiffness, and function subscales.
- RegistrySubmissionRecord: Formatted data conforming to registry requirements.

## Domain Events Published

- ArthroplastyCompleted: Emitted when surgery is recorded with implant details.
- ImplantRegistered: Emitted when implant data is submitted to the registry.
- RevisionIndicated: Emitted when a case is flagged for potential revision.
- OutcomeScoreRecorded: Emitted when a follow-up outcome assessment is completed.

## Domain Events Consumed

- SurgicalPlanApproved: From Surgical Planning, providing planned implant details.
- ImplantSelectionFinalized: From Surgical Planning, confirming component choices.

## Invariants

- All implant components must form a valid manufacturer-approved combination.
- Serial numbers must be recorded for every implanted component.
- Outcome scores must be collected at protocol-defined intervals (6 weeks, 1 year, etc.).
- Revision cases must reference the primary arthroplasty case.
- Registry submission data must be complete before the submission is allowed.
- An implant component cannot be marked as in-vivo in two different patients.

## Domain Services

- RevisionAssessmentService: Evaluates whether a case warrants revision.
- RegistrySubmissionService: Validates and formats data for registry submission.

## Clinical Workflow

1. SurgicalPlanApproved event triggers case preparation.
2. At surgery, implant components are scanned and recorded with serial numbers.
3. ArthroplastyCompleted event is published with full operative details.
4. Registry submission is prepared and validated.
5. Follow-up outcome scores are recorded at defined intervals.
6. Outcome trends are monitored for signs of deterioration.
7. If revision is indicated, RevisionIndicated event triggers re-evaluation.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- National Joint Registry. Annual Reports. https://www.njrcentre.org.uk
- American Joint Replacement Registry. Annual Reports. https://www.aaos.org/registries
