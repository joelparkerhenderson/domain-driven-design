# Ubiquitous Language in the Mental Health Domain

## Overview

Ubiquitous language is the shared vocabulary that domain experts and developers
use consistently in conversation, documentation, and code. In DDD, the
ubiquitous language is not a glossary that sits on a shelf; it is the living
language embedded in every model, every class name, every method signature,
and every user story. When clinicians say "assessment" and developers write
an Assessment class, both must mean exactly the same thing.

In the mental health domain, establishing ubiquitous language is both critical
and challenging. Mental health professionals come from diverse disciplines
(psychiatry, psychology, social work, psychiatric nursing, counseling) and
each discipline carries its own terminological traditions. A psychiatrist's
"mental status examination" and a psychologist's "psychological testing" are
related but distinct concepts that the ubiquitous language must differentiate.

## Why Ubiquitous Language Matters

Without a shared language, mental health systems suffer from:

- Ambiguity: "evaluation" could mean an intake screening, a diagnostic
  interview, a psychological test battery, or a utilization review.
- Translation overhead: developers spend time converting clinical jargon
  into technical terms and back again, introducing errors at each translation.
- Model erosion: the code gradually diverges from clinical reality because
  developers guess at meanings rather than using precise clinical terms.
- Stakeholder frustration: clinicians cannot validate software behavior
  because the interface uses unfamiliar terminology.

## Building the Language

The ubiquitous language for the mental health domain was developed through
collaboration between clinical domain experts and domain modelers. The process
involved:

1. Identifying core concepts through clinical workflow analysis.
2. Resolving terminological conflicts between disciplines (e.g., whether
   "intervention" means a therapeutic technique or a crisis response).
3. Binding each term to a specific bounded context where its meaning is
   precise and unambiguous.
4. Documenting the language in the ubiquitous-language.md glossary.
5. Validating the language against published clinical standards (DSM-5,
   APA Practice Guidelines).

## Context-Specific Language

Some terms carry different meanings across bounded contexts, which is expected
and healthy in DDD:

- "Assessment" in the Clinical Assessment Context means a structured
  diagnostic evaluation. In the Outcomes Measurement Context, it means a
  repeated symptom measurement administered at regular intervals.
- "Plan" in the Treatment Planning Context means an individualized treatment
  plan. In the Crisis Intervention Context, it means a safety plan.
- "Note" in the Therapy Management Context means a progress note documenting
  a therapy session. In the Medication Management Context, it could refer
  to a prescribing rationale note.

These differences are not bugs; they are features of bounded context design.
Each context maintains its own precise subset of the ubiquitous language.

## Key Terms

The mental health domain's ubiquitous language includes approximately 30 core
terms spanning all six bounded contexts. Key categories include:

Clinical Processes: assessment, screening, intake, clinical formulation,
diagnostic criteria, informed consent, session, progress note.

Treatment Concepts: treatment plan, treatment goal, care level, modality,
evidence-based practice, therapeutic alliance.

Crisis and Safety: crisis intervention, risk assessment, safety plan.

Medication: psychotropic medication, titration, drug interaction.

Measurement: outcome measure, patient-reported outcome, symptom tracking,
functional assessment.

Foundational: biopsychosocial model, comorbidity, DSM-5, PHQ-9, GAD-7,
domain event.

## Language Governance

The ubiquitous language is a living artifact. Governance practices include:

- New terms must be proposed by either a domain expert or a developer and
  validated by both before adoption.
- Existing terms may only be redefined through explicit consensus.
- Deprecated terms are removed from code and documentation simultaneously.
- Each bounded context team is responsible for its language subset.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 2 on ubiquitous language.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 1 on the ubiquitous language.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 2 on discovering domain knowledge.
- American Psychiatric Association. Diagnostic and Statistical Manual of Mental Disorders, Fifth Edition (DSM-5). APA Publishing, 2013.
- Substance Abuse and Mental Health Services Administration (SAMHSA). National Behavioral Health Quality Framework. SAMHSA, 2015.
