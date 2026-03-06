# Entities in the Orthopaedic Domain

## Overview

An entity is a domain object defined by its unique identity rather than its attributes.
Entities persist over time, and their attributes may change while their identity
remains constant. In the orthopaedic domain, entities represent clinical objects
that must be tracked individually across the patient care pathway.

## Purpose

Orthopaedic care involves tracking specific patients, specific implants, specific
fractures, and specific rehabilitation episodes over extended periods. A hip implant
must be traceable from manufacture through implantation to potential revision decades
later. Entities provide this identity continuity, ensuring that domain operations
reference the correct clinical object even as its state evolves.

## Key Entities by Bounded Context

### Clinical Assessment Context

**Patient (Assessment View)**: Identified by a patient identifier, contains the
assessment-relevant demographics, medical history, and allergy information. Attributes
such as weight, medications, and comorbidities change over time while identity persists.

**Clinical Encounter**: Identified by an encounter identifier, represents a single
clinical visit. Transitions through states: scheduled, in-progress, completed. The
encounter accumulates examination findings and imaging references during the visit.

**Imaging Study Reference**: Identified by an accession number, represents a specific
imaging investigation ordered and its associated findings.

### Surgical Planning Context

**Surgical Plan**: Identified by a plan identifier, represents the complete operative
specification. Transitions through states: draft, reviewed, approved, executed.
Attributes including approach, implant selection, and positioning evolve during planning.

**Surgeon**: Identified by a surgeon identifier, represents an operating surgeon with
credentials, subspecialty, and implant preferences that inform planning decisions.

### Joint Replacement Context

**Arthroplasty Case**: Identified by a case identifier, represents a joint replacement
from surgery through long-term follow-up. Links to specific implant components and
accumulates outcome scores over years.

**Implant Component**: Identified by a serial number and catalog number, represents
a specific physical implant part. Tracked from surgical insertion through the
component's entire in-vivo life, including potential revision or removal.

### Sports Medicine Context

**Sports Injury Case**: Identified by a case identifier, tracks an athletic injury
from presentation through treatment and return to sport. State transitions reflect
the treatment pathway: assessed, treated, rehabilitating, cleared.

**Athlete Profile**: Identified by an athlete identifier, captures sport, position,
competitive level, and injury history relevant to sports medicine decision-making.

### Fracture Management Context

**Fracture Case**: Identified by a case identifier, tracks a fracture from diagnosis
through treatment and confirmed union or nonunion. State transitions: classified,
reduced, fixated, healing, united (or nonunion).

**Fixation Hardware**: Identified by a hardware identifier and catalog details,
represents the specific plate, screw, nail, or external fixator used. Tracked for
potential future removal or failure analysis.

### Rehabilitation Context

**Rehabilitation Episode**: Identified by an episode identifier, represents a complete
course of rehabilitation. Transitions through defined phases with measurable milestones.

**Therapy Session**: Identified by a session identifier, records a single therapy
appointment with the exercises performed, measurements taken, and therapist observations.

## Entity Design Principles

- Assign a unique, immutable identifier at creation time.
- Define identity equality: two entities are equal if and only if they have the same identity.
- Model state transitions explicitly using lifecycle states or state machines.
- Avoid exposing setters; use domain methods that enforce business rules on state changes.
- Keep entities focused: include only the attributes and behaviors needed within the
  bounded context.

## Identity Generation

- Use domain-meaningful identifiers where they exist (e.g., implant serial numbers).
- Use system-generated identifiers (UUIDs) when no natural identifier exists.
- Ensure identifiers are unique within their bounded context.
- Cross-context references use the identifier without importing the full entity.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 5.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 5.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 10.
