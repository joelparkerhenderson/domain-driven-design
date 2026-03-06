# Ubiquitous Language in Cardiology

## Overview

Ubiquitous language is a shared vocabulary developed collaboratively between domain experts (cardiologists, cardiac nurses, technologists) and software developers. In the cardiology domain, establishing a ubiquitous language is essential because cardiology employs highly specialized medical terminology with precise clinical meanings. Misunderstanding a term like "ejection fraction" or "STEMI" can lead to incorrect domain models and potentially dangerous clinical decision support.

## Purpose

The ubiquitous language serves multiple functions in cardiology domain modeling:

- **Eliminates ambiguity** between clinical and technical teams. When a cardiologist says "stress test," the development team understands this as a specific aggregate with defined attributes, behaviors, and invariants.
- **Reflects clinical reality** by using terms that cardiologists actually use in practice, rather than developer-imposed abstractions.
- **Enforces consistency** within each bounded context. The same term carries the same meaning everywhere within a given context.
- **Documents model decisions** by making the chosen terminology explicit and traceable to clinical guidelines.

## Language Development Process

Building the ubiquitous language for cardiology requires structured collaboration:

1. **Domain Expert Interviews**: Conduct sessions with practicing cardiologists from each subspecialty (interventional, electrophysiology, heart failure, imaging) to capture the terminology they use in daily practice.

2. **Guideline Review**: Analyze ACC/AHA clinical practice guidelines to identify standardized terminology. Guidelines provide authoritative definitions that resolve disputes about term meanings.

3. **Event Storming Workshops**: Use event storming sessions to surface domain events, commands, and aggregates. The terminology that emerges during these sessions becomes part of the ubiquitous language.

4. **Glossary Maintenance**: Maintain a living glossary (see ubiquitous-language.md) that documents each term with its precise definition, the bounded context where it applies, and any clinical references.

5. **Code Alignment**: Ensure that class names, method names, variable names, and module names in the codebase use the ubiquitous language exactly. A class named "EjectionFraction" is preferred over "EFValue" or "CardiacMetric."

## Context-Specific Terminology

The ubiquitous language is bounded by context. Key examples of context-specific meanings:

**Clinical Assessment Context**: "Assessment" refers to a clinical evaluation encounter. "Finding" refers to an observed clinical sign or reported symptom. "Score" refers to a validated risk stratification instrument result.

**Diagnostic Imaging Context**: "Study" refers to a complete imaging examination. "Measurement" refers to a quantified imaging parameter. "Protocol" refers to a defined image acquisition sequence.

**Interventional Procedures Context**: "Lesion" refers to a coronary artery stenosis. "Intervention" refers to a therapeutic catheter-based action. "Access" refers to the vascular entry site.

**Electrophysiology Context**: "Signal" refers to an intracardiac electrical recording. "Map" refers to a three-dimensional electrical activation representation. "Lead" refers to a pacemaker or ICD electrode.

**Heart Failure Management Context**: "Therapy" refers to a GDMT medication. "Titration" refers to the process of adjusting medication dosage. "Stage" refers to ACC/AHA heart failure progression.

**Cardiac Rehabilitation Context**: "Session" refers to a supervised exercise encounter. "Goal" refers to a measurable outcome target. "Phase" refers to a rehabilitation program stage (Phase I through Phase III).

## Language Governance

The ubiquitous language requires ongoing governance:

- **Change Control**: Any proposed change to a term in the ubiquitous language must be reviewed by both domain experts and development teams.
- **Cross-Context Translation**: When a concept crosses bounded context boundaries, explicit translation must be documented in the context map.
- **Guideline Updates**: When ACC/AHA guidelines introduce new terminology or redefine existing terms, the ubiquitous language must be updated accordingly.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 2 on communication and the use of language.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 2 on ubiquitous language.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 2 on discovering domain knowledge.
- ACC/AHA Key Data Elements and Definitions for Cardiovascular Endpoint Events in Clinical Trials. *Circulation*, 2015.
