# Ubiquitous Language in Gastroenterology

## Overview

Ubiquitous language is the shared vocabulary used consistently by domain experts,
developers, and stakeholders when discussing the gastroenterology domain. Every term
in the ubiquitous language has a precise, agreed-upon meaning within the bounded
context where it is used. The same term may carry different meanings in different
contexts, and the ubiquitous language makes these distinctions explicit.

## Purpose

In gastroenterology, precise clinical terminology is essential for patient safety and
clinical accuracy. The ubiquitous language bridges the gap between clinical
gastroenterology vocabulary and the software model. When a hepatologist says "MELD
score," the domain model must represent exactly what that clinician means: a specific
calculation using bilirubin, creatinine, and INR that predicts 90-day mortality in
cirrhosis. There is no room for interpretation or approximation.

## Language Formation Process

The ubiquitous language emerges from collaboration between gastroenterology domain
experts (physicians, nurses, dietitians, endoscopy technicians) and the development
team. The process involves:

1. Domain expert interviews to capture how clinicians describe their workflows.
2. Identification of terms that have precise clinical definitions versus terms used
   informally.
3. Resolution of terminology conflicts between bounded contexts.
4. Documentation of each term with its definition, context of use, and any constraints.
5. Continuous refinement as the model evolves and new clinical scenarios emerge.

## Context-Specific Language

### Clinical Assessment Context

Terms like "chief complaint," "review of systems," "alarm feature," and "differential
diagnosis" carry specific meanings tied to the evaluation workflow. An "alarm feature"
is not merely a concerning symptom; it is a defined set of findings (dysphagia, GI
bleeding, weight loss, anemia) that mandate specific diagnostic actions.

### Endoscopic Procedures Context

Terms like "scope insertion," "withdrawal time," "Boston Bowel Preparation Scale,"
and "polyp morphology" have precise procedural definitions. "Withdrawal time" refers
specifically to the measured duration of colonoscope withdrawal and correlates with
adenoma detection rate.

### Inflammatory Disease Context

Terms like "flare," "remission," "mucosal healing," and "treat-to-target" have
IBD-specific meanings. "Remission" encompasses both clinical remission (symptom
resolution) and endoscopic remission (mucosal healing), and the ubiquitous language
distinguishes between these explicitly.

### Hepatology Context

Terms like "decompensation," "variceal surveillance," "encephalopathy grading," and
"listing criteria" carry hepatology-specific meanings. "Decompensation" refers to the
onset of ascites, variceal bleeding, encephalopathy, or jaundice in a cirrhotic patient,
marking a critical transition in disease management.

### Motility Disorders Context

Terms like "integrated relaxation pressure," "distal contractile integral," and
"DeMeester score" are quantitative motility metrics with precise definitions established
by the Chicago Classification and Lyon Consensus.

### Nutrition Management Context

Terms like "malnutrition screening," "refeeding syndrome risk," "gluten challenge," and
"elimination diet" have nutrition-specific clinical definitions that guide assessment
and intervention protocols.

## Language Governance

The ubiquitous language is a living artifact. New terms are added when clinical practice
introduces new concepts. Existing terms are refined when ambiguity is discovered. The
glossary documented in the ubiquitous-language.md file serves as the authoritative
reference, and all code, documentation, and communication must use these terms
consistently.

## Relationship to Code

Every class, method, variable, and event in the domain model uses terms from the
ubiquitous language. A class named FlareEpisode corresponds directly to the clinical
concept of a disease flare. A method named CalculateMELDScore performs exactly the
calculation that a hepatologist expects. This direct correspondence between language
and code is what makes the model a true reflection of the domain.

## References

- Evans, E. (2003). *Domain-Driven Design*. Addison-Wesley. Chapter 2: Communication and the Use of Language.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley. Chapter 1: Getting Started with DDD.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media. Chapter 2: Discovering Domain Knowledge.
