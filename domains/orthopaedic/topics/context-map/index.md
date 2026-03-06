# Context Map in the Orthopaedic Domain

## Overview

A context map is a visual and textual representation of the relationships and
interactions between bounded contexts. In the orthopaedic domain, the context map
defines how Clinical Assessment, Surgical Planning, Joint Replacement, Sports
Medicine, Fracture Management, and Rehabilitation exchange information and
coordinate clinical workflows.

## Purpose

The context map makes explicit the dependencies, data flows, and integration
patterns between orthopaedic bounded contexts. Without a context map, teams risk
creating implicit couplings that undermine the independence of each context and
introduce fragility into the system.

## Context Relationships

### Clinical Assessment --> Surgical Planning (Customer-Supplier)

Clinical Assessment is the upstream supplier of diagnostic findings. Surgical Planning
is the downstream customer that consumes examination results, imaging interpretations,
and classification data to build operative plans. The Clinical Assessment team
accommodates the data needs of Surgical Planning through a published interface.

### Clinical Assessment --> Fracture Management (Customer-Supplier)

Clinical Assessment supplies fracture classification and imaging data to the Fracture
Management Context. The AO/OTA classification produced during assessment drives
the fracture treatment pathway.

### Clinical Assessment --> Sports Medicine (Customer-Supplier)

Clinical Assessment provides injury diagnosis and imaging findings to Sports Medicine,
which uses them to determine whether conservative or arthroscopic treatment is indicated.

### Surgical Planning --> Joint Replacement (Partnership)

Surgical Planning and Joint Replacement operate as partners. The surgical plan specifies
implant selection and surgical approach, which the Joint Replacement Context records
and tracks. Both teams collaborate closely because implant selection directly affects
registry data and long-term outcomes.

### Surgical Planning --> Fracture Management (Partnership)

For operative fractures, Surgical Planning and Fracture Management collaborate as
partners. The fixation strategy is planned in Surgical Planning and executed and
monitored in Fracture Management.

### Joint Replacement --> Rehabilitation (Customer-Supplier)

Joint Replacement is upstream, publishing arthroplasty completion events with implant
details and weight-bearing restrictions. Rehabilitation is downstream, consuming
these events to initiate the appropriate post-operative protocol.

### Fracture Management --> Rehabilitation (Customer-Supplier)

Fracture Management supplies fixation details and weight-bearing status to
Rehabilitation, which uses this information to select the correct healing protocol
and milestone targets.

### Sports Medicine --> Rehabilitation (Customer-Supplier)

Sports Medicine supplies injury details and surgical intervention data to
Rehabilitation, which constructs return-to-play protocols based on the specific
injury and procedure performed.

## External System Integration

### PACS Integration (Anti-Corruption Layer)

The Picture Archiving and Communication System uses DICOM standards. An anti-corruption
layer translates DICOM imaging data into the Clinical Assessment Context's model.

### HIS/EHR Integration (Anti-Corruption Layer)

Hospital Information Systems use HL7/FHIR standards. Anti-corruption layers translate
patient demographics, orders, and results between HIS/EHR and each orthopaedic context.

### National Joint Registry (Conformist)

The Joint Replacement Context conforms to the data submission requirements of national
joint registries (e.g., NJR, AJRR). The context adopts the registry's data model
for submission purposes.

## Integration Patterns Used

- Published Language: Domain events with standardized schemas.
- Open Host Service: Each context exposes a service interface for consumers.
- Anti-Corruption Layer: Translates external system models at the boundary.
- Conformist: Joint Replacement conforms to external registry standards.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 14.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 3.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 8.
