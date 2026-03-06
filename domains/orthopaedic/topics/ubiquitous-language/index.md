# Ubiquitous Language in the Orthopaedic Domain

## Overview

Ubiquitous language is a shared vocabulary used consistently by domain experts and
developers within a bounded context. In the orthopaedic domain, ubiquitous language
ensures that orthopaedic surgeons, physiotherapists, nurses, radiologists, and
software developers use the same terms with the same meanings when discussing
models, workflows, and business rules.

## Purpose

Orthopaedic medicine has precise clinical terminology. Misunderstandings between
clinicians and developers can lead to incorrect models that compromise patient
safety. Ubiquitous language eliminates ambiguity by establishing a single, agreed-upon
vocabulary within each bounded context. The language appears in code, documentation,
conversations, and user interfaces.

## Language Per Bounded Context

### Clinical Assessment Language

- Clinical Encounter: A single patient visit for musculoskeletal evaluation.
- Examination Finding: An objective observation from physical examination.
- Imaging Study: A radiographic, MRI, CT, or ultrasound investigation.
- Provisional Diagnosis: The working clinical impression before confirmatory testing.
- Classification: A standardized categorization of a musculoskeletal condition.

### Surgical Planning Language

- Surgical Plan: The complete pre-operative specification for an orthopaedic procedure.
- Surgical Approach: The anatomical route used to access the operative site.
- Implant Template: The projected size and position of an implant on imaging.
- Pre-Operative Assessment: The medical evaluation confirming fitness for surgery.
- Operative Risk: A quantified assessment of potential surgical complications.

### Joint Replacement Language

- Arthroplasty Case: A joint replacement procedure with its associated data.
- Implant Component: A single part of a multi-component prosthetic joint.
- Bearing Surface: The articulating material combination in a prosthetic joint.
- Revision: A subsequent surgery to modify, replace, or remove an implant.
- Outcome Score: A validated patient-reported or clinician-reported measure.

### Sports Medicine Language

- Sports Injury Case: An athletic injury episode from onset through resolution.
- Arthroscopic Procedure: A minimally invasive joint surgery using a camera.
- Return-to-Play Protocol: A staged progression toward competitive sport resumption.
- Functional Test: A physical performance assessment measuring readiness.
- Injury Mechanism: The biomechanical cause of an athletic injury.

### Fracture Management Language

- Fracture Case: A bone fracture episode from diagnosis through healing.
- AO/OTA Code: The standardized alphanumeric fracture classification identifier.
- Reduction: The restoration of bone fragments to anatomical alignment.
- Fixation Method: The hardware or technique used to stabilize a fracture.
- Healing Status: The current state of bone union assessed by imaging.

### Rehabilitation Language

- Rehabilitation Episode: A complete course of post-operative or post-injury therapy.
- Therapy Phase: A defined stage within a rehabilitation protocol with specific goals.
- Functional Milestone: A measurable achievement indicating recovery progress.
- Range of Motion Target: The joint movement goal for a specific recovery phase.
- Weight-Bearing Progression: The staged increase in load permitted on a limb.

## Maintaining the Language

- Document terms in a glossary accessible to all team members.
- Use the exact domain terms in code: class names, method names, variable names.
- Review language with domain experts during each iteration.
- Update the glossary when new concepts emerge or meanings evolve.
- Reject developer jargon that does not appear in clinical vocabulary.

## Common Pitfalls

- Using generic terms like "item" or "record" instead of domain-specific terms.
- Allowing the same term to mean different things across contexts without explicit
  context boundaries.
- Inventing terms that clinicians do not recognize or use.
- Allowing technical infrastructure terms to leak into domain models.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 2.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 2.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 2.
