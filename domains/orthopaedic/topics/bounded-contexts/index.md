# Bounded Contexts in the Orthopaedic Domain

## Overview

A bounded context is a logical boundary within which a particular domain model is
defined and applicable. In the orthopaedic domain, bounded contexts separate the
distinct clinical workflows of assessment, surgery, implant management, sports
medicine, fracture care, and rehabilitation into independently modeled areas.

## Purpose

Bounded contexts prevent model ambiguity. The term "procedure" means different things
in Clinical Assessment (a diagnostic test), Surgical Planning (an operative plan),
and Rehabilitation (a therapeutic exercise). By enclosing each meaning within its
own bounded context, the orthopaedic system avoids confusion and maintains model
integrity.

## The Six Orthopaedic Bounded Contexts

### 1. Clinical Assessment Context

Owns the model for patient examination, musculoskeletal history, imaging orders and
interpretation, and diagnostic classification. The aggregate root is the Clinical
Encounter, which groups examination findings, imaging results, and provisional
diagnoses into a consistent unit.

### 2. Surgical Planning Context

Owns the model for pre-operative assessment, surgical approach selection, implant
templating, and operative scheduling. The aggregate root is the Surgical Plan,
which must be internally consistent regarding patient anatomy, chosen implants,
and surgeon availability.

### 3. Joint Replacement Context

Owns the model for arthroplasty procedures, implant registry entries, revision
tracking, and long-term outcome monitoring. The aggregate root is the Arthroplasty
Case, which links the patient, implant components, and outcome scores over time.

### 4. Sports Medicine Context

Owns the model for athletic injury assessment, arthroscopic procedures, and
return-to-play progression. The aggregate root is the Sports Injury Case, which
tracks the injury, interventions, and criteria-based return milestones.

### 5. Fracture Management Context

Owns the model for fracture classification (AO/OTA), reduction technique, fixation
method, and healing monitoring. The aggregate root is the Fracture Case, which
maintains the classification, treatment plan, and radiographic follow-up as a
consistent unit.

### 6. Rehabilitation Context

Owns the model for post-operative protocols, therapy sessions, functional milestones,
and patient-reported outcomes. The aggregate root is the Rehabilitation Episode,
which sequences therapy phases and tracks progress against protocol targets.

## Context Boundaries

Each bounded context defines its own:

- Ubiquitous language specific to its clinical domain.
- Aggregates, entities, and value objects.
- Domain events that it publishes.
- Repository interfaces for persistence abstraction.
- Invariants and business rules.

## Relationships Between Contexts

Contexts communicate through well-defined interfaces. Clinical Assessment publishes
diagnostic events consumed by Surgical Planning and Fracture Management. Joint
Replacement publishes implant-related events consumed by Rehabilitation. These
relationships are documented in the context map.

## Modeling Principles

- Each context has autonomy over its internal model.
- Shared concepts (e.g., Patient) are represented differently in each context,
  containing only the attributes relevant to that context.
- Cross-context data exchange occurs through published domain events or explicit
  integration interfaces, never through shared databases.

## Benefits

- Teams specializing in trauma surgery can evolve the Fracture Management model
  without affecting Joint Replacement.
- Regulatory requirements specific to implant tracking apply only within the Joint
  Replacement Context.
- Rehabilitation protocols can be updated independently of surgical planning workflows.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 14.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapters 2-3.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapters 6-7.
