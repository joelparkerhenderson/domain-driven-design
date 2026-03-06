# Ubiquitous Language - Kinesiology Domain

## Overview

Ubiquitous language is the shared vocabulary that domain experts, developers, and stakeholders use to communicate about the domain model. In Domain-Driven Design, this language is not merely documentation; it is embedded in the code, the conversations, the tests, and the architecture. Every term in the ubiquitous language has a precise, agreed-upon meaning within its bounded context.

For the kinesiology domain, the ubiquitous language bridges the gap between movement science professionals (kinesiologists, physical therapists, athletic trainers, strength coaches, sport scientists) and the software developers who build systems to support their practice. Without a shared language, critical nuances of movement science can be lost in translation, leading to software that misrepresents clinical concepts.

## Language Development Process

The ubiquitous language for the kinesiology domain was developed through collaboration with domain experts across the six bounded contexts. Each term was evaluated for clarity, precision, and consistency with established professional usage in kinesiology, exercise science, and sports medicine literature.

Terms that have different meanings in different bounded contexts are explicitly documented with their context-specific definitions. The term "protocol" means a standardized assessment battery in the Movement Assessment Context, a phased recovery plan in the Rehabilitation Planning Context, and a screening procedure in the Injury Prevention Context. These distinctions are maintained at the bounded context boundary rather than forced into a single overloaded definition.

## Language Governance

The ubiquitous language is a living artifact that evolves as the domain model matures. Changes to the language follow a governance process. Any stakeholder may propose a new term or a modification to an existing definition. The proposal is reviewed by domain experts from the affected bounded contexts. Accepted changes are propagated to code, documentation, and training materials simultaneously.

Terms should not be introduced casually. Each addition to the ubiquitous language represents a commitment to consistent usage across all communication channels. Synonyms are explicitly noted and resolved: the team agrees on a single canonical term for each concept and documents any aliases that domain experts may use informally.

## Context-Specific Language

Each bounded context maintains its own subset of the ubiquitous language. Some terms are shared across contexts through published language contracts, while others are strictly internal.

Movement Assessment Context uses terms rooted in clinical evaluation: gait cycle, stance phase, swing phase, joint angle, manual muscle test grade, dermatome, myotome, and special test. These terms carry specific clinical meanings defined by professional organizations.

Exercise Prescription Context uses training science terminology: mesocycle, microcycle, training age, repetition maximum, rate of perceived exertion, training density, and exercise regression. These terms reflect exercise physiology and strength and conditioning conventions.

Rehabilitation Planning Context uses clinical recovery language: tissue healing phase, weight-bearing status, functional milestone, discharge criteria, and therapeutic dosage. These terms align with physical therapy and sports medicine practice.

Performance Analysis Context uses biomechanics and measurement terminology: ground reaction force, center of pressure, joint moment, angular velocity, electromyographic activation, and coefficient of variation. These terms reflect laboratory measurement conventions.

Injury Prevention Context uses epidemiological and risk management terminology: injury incidence rate, relative risk, odds ratio, number needed to treat, acute-to-chronic workload ratio, and cumulative load. These terms reflect sports injury epidemiology conventions.

Research and Education Context uses academic and professional development terminology: systematic review, meta-analysis, level of evidence, effect size, curriculum objective, and competency standard. These terms reflect research methodology and educational design conventions.

## Language Consistency Rules

Use the canonical term consistently in all code, documentation, and conversation. Never use informal synonyms in code even if they are common in casual conversation.

When a term crosses a bounded context boundary, document the translation explicitly. Do not assume that the same word carries the same meaning in two different contexts.

Define all abbreviations on first use and maintain an abbreviation registry. Kinesiology is rich in abbreviations (ROM, FMS, RFD, ACWR, 1RM, RPE, EMG) and their expanded forms must be unambiguous.

Ground all quantitative terms in their units of measurement. Range of motion is measured in degrees. Force is measured in newtons. Workload may be measured in arbitrary units, session-RPE, or tonnage depending on context, and the chosen unit must be explicit.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 2 on ubiquitous language.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 1 on ubiquitous language development.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 2 on discovering domain knowledge.
- American College of Sports Medicine. ACSM's Guidelines for Exercise Testing and Prescription. 11th ed. Wolters Kluwer, 2022.
