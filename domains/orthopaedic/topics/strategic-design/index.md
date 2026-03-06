# Strategic Design in the Orthopaedic Domain

## Overview

Strategic design in Domain-Driven Design addresses how to decompose the large and complex
orthopaedic domain into manageable bounded contexts. Orthopaedic care spans multiple
clinical specialties, surgical disciplines, and rehabilitation pathways, making strategic
decomposition essential for managing complexity and aligning software models with the
realities of musculoskeletal medicine.

## Purpose

Strategic design ensures that each part of the orthopaedic system has a clear boundary,
a well-defined model, and explicit relationships with other parts. Without strategic
design, an orthopaedic system risks becoming a monolithic model where clinical assessment
terms conflict with surgical planning terms, or where rehabilitation concepts are
entangled with implant registry logic.

## Key Strategic Patterns

### Bounded Contexts

The orthopaedic domain is divided into six bounded contexts: Clinical Assessment,
Surgical Planning, Joint Replacement, Sports Medicine, Fracture Management, and
Rehabilitation. Each context owns its model and enforces its own invariants. For
example, the concept of "patient" exists in every context but carries different
attributes and behaviors in each.

### Context Map

The context map defines how the six bounded contexts interact. Upstream contexts
publish events and data that downstream contexts consume. For instance, Clinical
Assessment is upstream of Surgical Planning, which consumes diagnostic findings
to inform operative plans.

### Subdomains

The orthopaedic domain classifies its areas into core subdomains (Surgical Planning,
Joint Replacement, Fracture Management), supporting subdomains (Clinical Assessment,
Sports Medicine, Rehabilitation), and generic subdomains (scheduling, billing,
identity management). Core subdomains receive the most design investment because
they represent the primary competitive and clinical differentiators.

### Anti-Corruption Layers

Anti-corruption layers protect orthopaedic bounded contexts from external system
models. Hospital information systems (HIS), picture archiving and communication
systems (PACS), and electronic health records (EHR) all use different data models.
ACLs translate between these external representations and the orthopaedic domain model.

## Strategic Design Process

1. Engage domain experts: orthopaedic surgeons, physiotherapists, radiologists, nurses.
2. Identify the natural language boundaries where terminology diverges.
3. Group related concepts into candidate bounded contexts.
4. Define the relationships between contexts using the context map.
5. Classify subdomains by strategic importance to guide investment.
6. Design anti-corruption layers for external system integration.

## Benefits for Orthopaedics

- Prevents a single "patient record" model from collapsing under the weight of
  clinical, surgical, and rehabilitation requirements.
- Allows specialized teams (trauma surgeons, sports medicine physicians, therapists)
  to evolve their models independently.
- Enables compliance with different regulatory standards per context.
- Supports phased system rollout, one bounded context at a time.

## Challenges

- Orthopaedic surgeons often work across multiple contexts (e.g., a surgeon performs
  both joint replacements and fracture fixations), which requires careful modeling of
  shared concepts.
- Imaging data flows through multiple contexts and must be translated appropriately.
- Patient identity must be consistent across contexts without coupling models.

## Domain Expert Collaboration

Strategic design depends on close collaboration with orthopaedic domain experts.
Event Storming workshops, as described by Alberto Brandolini, are particularly
effective for discovering domain events and context boundaries in orthopaedic
workflows. These workshops bring surgeons, therapists, and developers together
to model clinical pathways on a shared timeline.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapters 14-16.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 2.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Part I.
- Brandolini, Alberto. Introducing EventStorming. Leanpub, 2021.
