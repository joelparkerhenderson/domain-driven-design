# Fracture Management Context

## Overview

The Fracture Management Context is the bounded context responsible for the
classification, treatment, and healing monitoring of bone fractures. It applies
the AO/OTA classification system to categorize fractures systematically, manages
both operative and non-operative treatment pathways, and tracks healing progress
through radiographic and clinical follow-up.

## Scope

This context covers:

- Fracture classification using the AO/OTA alphanumeric system.
- Fracture pattern documentation (displacement, angulation, comminution).
- Treatment pathway selection: operative (ORIF, intramedullary nailing) or
  conservative (casting, splinting).
- Reduction documentation: closed or open, quality assessment.
- Fixation method recording: plates, screws, nails, external fixators, wires.
- Healing monitoring through serial radiographic assessment.
- Complication identification: malunion, nonunion, hardware failure.

This context does not cover: initial clinical assessment (Clinical Assessment
Context), detailed operative planning (Surgical Planning Context), implant registry
for arthroplasty (Joint Replacement Context), or rehabilitation therapy
(Rehabilitation Context).

## Ubiquitous Language

- Fracture Case: A bone fracture from diagnosis through union or treatment conclusion.
- AO/OTA Code: The standardized alphanumeric classification for a fracture.
- Reduction: Restoration of bone fragments to anatomical alignment.
- Closed Reduction: Non-surgical realignment by external manipulation.
- Open Reduction: Surgical exposure and realignment of fracture fragments.
- Fixation Method: The hardware or technique stabilizing the fracture.
- Healing Status: Current state of bone union (healing, united, delayed, nonunion).
- Comminution: A fracture broken into three or more fragments.
- Malunion: Healing in a non-anatomical position.
- Nonunion: Failure to heal within the expected timeframe.

## Aggregate Root

**FractureCase** is the aggregate root. It maintains consistency between the AO/OTA
classification, the treatment plan, the fixation method, and the healing assessment
history. The fixation method must be appropriate for the classified fracture type.
Healing assessments must follow the prescribed follow-up schedule.

## Key Entities

- FractureCase: Tracks the fracture through classified, reduced, fixated, healing,
  united (or nonunion) states.
- FixationHardware: Tracks specific plates, screws, nails, or external fixators
  by catalog and lot identifiers.

## Key Value Objects

- AOOTACode: Bone number, fracture type (A/B/C), group, and subgroup.
- FracturePattern: Morphology (simple, wedge, comminuted), displacement, angulation.
- ReductionQuality: Assessment of anatomical alignment after reduction attempt.
- FixationConfiguration: Combination of hardware type, size, material, and positioning.
- HealingAssessment: Radiographic evaluation with callus grade and alignment at a point in time.
- Laterality: Left, right, or bilateral.
- AnatomicalSite: Bone, segment, and side.

## Domain Events Published

- FractureClassified: Emitted when a fracture receives its AO/OTA code.
- FractureReduced: Emitted when reduction is performed and quality documented.
- FractureFixated: Emitted when fixation is completed with hardware details.
- HealingProgressAssessed: Emitted at each follow-up radiographic assessment.
- FractureUnionConfirmed: Emitted when radiographic union is achieved.
- NonunionDiagnosed: Emitted when healing failure is identified.

## Domain Events Consumed

- PatientAssessed: From Clinical Assessment, with imaging findings.
- DiagnosisConfirmed: From Clinical Assessment, confirming fracture diagnosis.
- SurgicalPlanApproved: From Surgical Planning, for operative fractures.

## Invariants

- AO/OTA codes must conform to the valid classification hierarchy.
- Fixation method must be appropriate for the fracture type and location.
- Reduction quality must be documented before fixation proceeds.
- Follow-up assessments must occur at protocol-defined intervals.
- A fracture cannot be marked as united without supporting radiographic evidence.
- Hardware failure must trigger reassessment of the treatment plan.

## Domain Services

- FractureClassificationService: Applies AO/OTA classification from imaging data.
- HealingProgressService: Evaluates healing against expected timelines.

## Clinical Workflow

1. PatientAssessed event arrives with fracture imaging findings.
2. Fracture is classified using the AO/OTA system.
3. FractureClassified event is published.
4. Treatment pathway is determined: operative or conservative.
5. If operative: SurgicalPlanApproved event triggers fixation surgery.
6. Reduction quality and fixation hardware are documented.
7. Serial follow-up imaging assesses healing progress.
8. FractureUnionConfirmed or NonunionDiagnosed event is published.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- AO Foundation. AO/OTA Fracture and Dislocation Classification. https://www.aofoundation.org
- Meinberg, E.G. et al. "Fracture and Dislocation Classification Compendium." Journal of Orthopaedic Trauma, 2018.
