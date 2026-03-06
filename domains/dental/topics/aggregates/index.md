# Aggregates - Dental Domain

## Overview

Aggregates are clusters of related domain objects treated as a single unit for data consistency. Each aggregate has a root entity that controls access to all objects within the aggregate and enforces business invariants. In the dental domain, aggregates model the natural groupings of clinical and administrative data that must remain consistent when modified, such as a patient's dental chart or a treatment plan with its constituent procedures.

## Aggregate Design Principles for Dental Systems

Dental aggregates must balance clinical data integrity with practical transaction boundaries. A dental chart containing findings for 32 teeth with multiple surfaces each could grow very large if modeled as a single aggregate. The design must consider which data elements truly need transactional consistency and which can be eventually consistent through domain events.

## Aggregates by Bounded Context

### Clinical Examination Context

**Patient Clinical Record** (Aggregate Root)
The Patient Clinical Record is the primary aggregate in this context. It contains the dental chart with tooth-level condition entries, active examination findings, and radiographic interpretation records. The aggregate enforces the invariant that each tooth can have only one current condition status (present, missing, impacted, supernumerary) and that examination findings reference valid tooth numbers and surface codes.

**Radiographic Series** (Aggregate Root)
A Radiographic Series groups related radiographic images taken during a single imaging session. It enforces the invariant that a full-mouth series contains a specific set of periapical and bitewing projections, and that a panoramic series contains exactly one panoramic image with optional supplemental views.

### Treatment Planning Context

**Treatment Plan** (Aggregate Root)
The Treatment Plan aggregate contains treatment items organized into phases with sequencing dependencies. It enforces invariants such as: prerequisite procedures must be scheduled before dependent procedures, all procedures must have associated informed consent before the plan is approved, and total estimated cost must equal the sum of individual procedure estimates.

**Case Presentation** (Aggregate Root)
The Case Presentation groups the visual and narrative materials prepared for patient communication about a treatment plan. It enforces the invariant that a presentation must reference an existing treatment plan and include at minimum a diagnosis summary and treatment options.

### Restorative Services Context

**Restorative Case** (Aggregate Root)
The Restorative Case tracks a single restorative procedure or a related group of procedures (such as multiple preparations in a quadrant). It enforces invariants around procedure staging: a crown cannot be cemented before the impression is taken, and a temporary restoration must be placed between preparation and final restoration appointments.

**Endodontic Case** (Aggregate Root)
The Endodontic Case tracks root canal therapy from access through obturation. It enforces that canal working lengths must be established before instrumentation is completed, and that all identified canals must be obturated before the case is marked complete.

### Orthodontic Management Context

**Orthodontic Case** (Aggregate Root)
The Orthodontic Case is the primary aggregate, spanning the entire treatment duration. It contains the initial assessment, appliance records, adjustment visit records, and retention plan. It enforces the invariant that active treatment cannot begin without a completed assessment and approved treatment plan, and that retention phase cannot begin until active treatment objectives are met.

### Periodontal Care Context

**Periodontal Record** (Aggregate Root)
The Periodontal Record maintains the longitudinal history of periodontal measurements and treatments for a patient. It enforces the invariant that disease staging and grading must be updated when probing measurements indicate a change in disease status, and that maintenance intervals must be set based on the current disease classification.

### Practice Management Context

**Patient Account** (Aggregate Root)
The Patient Account manages the administrative relationship with a patient, including insurance coverage details, appointment history, and financial ledger entries. It enforces the invariant that claims cannot be submitted without verified insurance eligibility, and that scheduled appointments must reference valid provider and operatory combinations.

**Insurance Claim** (Aggregate Root)
The Insurance Claim tracks a submitted claim through its lifecycle from creation to payment or denial. It enforces that claim line items must reference valid CDT codes and that the total claimed amount must equal the sum of line item charges.

## Aggregate Sizing Guidelines

Small aggregates are preferred in the dental domain to support concurrent access patterns common in dental practices, where multiple providers, hygienists, and front desk staff may access different aspects of a patient's record simultaneously. Large aggregates that lock entire patient records during clinical charting would create unacceptable contention. Domain events propagate changes between aggregates to maintain eventual consistency.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 6 on aggregates.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 10 on aggregate design rules.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 5 on aggregate patterns and boundaries.
