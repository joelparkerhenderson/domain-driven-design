# Bounded Contexts - Dental Domain

## Overview

Bounded contexts define the logical boundaries within the dental domain where specific models, terminology, and rules are internally consistent. Each bounded context in the dental domain corresponds to a cohesive area of dental practice operations with its own aggregate roots, entities, value objects, and domain events. The six bounded contexts reflect the natural divisions observed in dental practice workflow and organizational structure.

## Clinical Examination Context

The Clinical Examination Context owns all diagnostic activities performed during patient visits. This context is the authoritative source for the patient's current oral health status.

Key responsibilities:
- Recording findings from visual and tactile oral examination.
- Managing radiographic imaging orders, acquisition, and interpretation.
- Maintaining the dental chart with tooth-by-tooth condition records.
- Detecting and classifying carious lesions by severity and location.
- Performing and recording periodontal probing measurements at six sites per tooth.

The Patient Clinical Record is the primary aggregate root in this context. It maintains consistency across examination findings, radiographic records, and charting data for a single patient.

## Treatment Planning Context

The Treatment Planning Context translates diagnostic findings from the Clinical Examination Context into sequenced, prioritized treatment plans that patients can review and approve.

Key responsibilities:
- Creating treatment plans with procedure sequencing based on clinical urgency and dependencies.
- Preparing case presentations that communicate findings and options to patients.
- Managing informed consent documentation for proposed procedures.
- Estimating costs including insurance coverage and patient responsibility amounts.

The Treatment Plan is the primary aggregate root. It enforces invariants such as prerequisite ordering (extraction before bridge preparation) and ensures all proposed procedures have associated consent documentation.

## Restorative Services Context

The Restorative Services Context manages the execution of restorative dental procedures. This context tracks materials, techniques, and outcomes for each restorative intervention.

Key responsibilities:
- Recording direct restorations (amalgam and composite fillings) with surface and material details.
- Managing indirect restorations (crowns, bridges, inlays, onlays) through preparation, impression, and cementation stages.
- Tracking implant cases from surgical placement through prosthetic loading.
- Managing endodontic procedures including access, instrumentation, and obturation.

The Restorative Case is the primary aggregate root, maintaining consistency across multi-visit procedures and ensuring material and technique selections are compatible.

## Orthodontic Management Context

The Orthodontic Management Context handles the extended timeline of orthodontic treatment from initial assessment through retention. This context operates on a fundamentally different timescale than other clinical contexts.

Key responsibilities:
- Assessing malocclusion using Angle's classification and cephalometric analysis.
- Managing appliance selection and placement for braces or clear aligner systems.
- Tracking tooth movement progress against treatment predictions at each adjustment visit.
- Planning and monitoring the retention phase following active treatment.

The Orthodontic Case is the primary aggregate root, enforcing consistency across the multi-month or multi-year treatment timeline and tracking cumulative progress.

## Periodontal Care Context

The Periodontal Care Context manages the diagnosis and ongoing treatment of periodontal disease. This context follows a disease management model with recurring assessment and maintenance cycles.

Key responsibilities:
- Recording scaling and root planing procedures by quadrant and tooth involvement.
- Tracking pocket depth measurements over time to assess disease progression or improvement.
- Scheduling and managing periodontal maintenance appointments at prescribed intervals.
- Classifying periodontal disease stage and grade according to current classification systems.

The Periodontal Record is the primary aggregate root, maintaining the longitudinal history of probing depths, treatment interventions, and disease classification changes.

## Practice Management Context

The Practice Management Context handles the administrative and business operations that support clinical care delivery. This context interacts with all clinical contexts to coordinate scheduling, billing, and patient communication.

Key responsibilities:
- Managing appointment scheduling across providers, operatories, and procedure types.
- Verifying patient insurance eligibility, benefits, and coverage limitations.
- Processing insurance claims with appropriate CDT procedure codes and documentation.
- Operating patient recall systems for preventive care and follow-up appointments.

The Patient Account is the primary aggregate root, maintaining consistency across scheduling, insurance, and financial data for each patient.

## Context Boundary Rules

Each bounded context enforces these boundary rules:
- No direct database sharing between contexts; all inter-context communication uses domain events or explicit integration interfaces.
- Each context owns its data and is the sole authority for mutations within its boundary.
- Shared concepts (such as "patient" or "tooth") are modeled independently in each context with only the attributes relevant to that context's responsibilities.
- Anti-corruption layers translate between context-specific models when integration is required.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 14 on bounded contexts.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapters 2-3 on bounded context definition.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 7 on modeling boundaries of bounded contexts.
