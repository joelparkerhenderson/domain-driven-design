# Advance Decision to Refuse Treatment Domain

## Introduction

This domain applies Domain-Driven Design (DDD) to advance decisions to refuse treatment, which are legally binding documents that allow a person to specify medical treatments they wish to refuse in the future, should they lose the capacity to make or communicate those decisions. Advance decisions are a cornerstone of patient autonomy, enabling individuals to maintain control over their healthcare even when they can no longer participate directly in clinical decision-making.

The advance decision to refuse treatment domain encompasses decision authoring, capacity assessment, legal validation, clinical application, stakeholder notification, and revocation processes. Each area operates as a distinct bounded context with its own models, terminology, and workflows, while sharing common domain concepts through well-defined integration patterns.

## Bounded Contexts

The advance decision to refuse treatment domain is decomposed into 6 bounded contexts:

1. **Decision Authoring** - Captures the patient's wishes regarding specific treatments they wish to refuse, including the clinical circumstances under which the refusal applies. This context manages the structured documentation of treatment preferences and the conditions that trigger applicability.

2. **Capacity Assessment** - Manages the evaluation of whether a person has the mental capacity to make an advance decision at the time of authoring. This includes formal assessments by qualified professionals, documentation of capacity findings, and the principle that capacity is presumed unless established otherwise.

3. **Legal Validation** - Ensures that advance decisions meet all statutory requirements for validity and applicability, including proper witnessing, signatures, written format for life-sustaining treatment refusals, and compliance with jurisdictional legislation such as the Mental Capacity Act 2005.

4. **Clinical Application** - Governs how healthcare professionals identify, interpret, and apply advance decisions during treatment episodes. This context handles the clinical workflow of checking for existing advance decisions, assessing their applicability to the current situation, and determining whether the decision is valid and applicable.

5. **Notification and Distribution** - Manages the communication of advance decisions to relevant parties including healthcare providers, hospitals, general practitioners, family members, and legal representatives, ensuring decisions are accessible when needed.

6. **Revocation and Amendment** - Handles the processes by which a person can withdraw, modify, or update their advance decision while they retain capacity, including oral revocation, written amendments, and the effect of subsequent lasting powers of attorney.

## Strategic Design

- **Subdomains** classify areas by strategic importance: Decision Authoring and Clinical Application are core subdomains; Capacity Assessment and Legal Validation are supporting; Notification and Distribution is generic.
- **Context Map** defines the relationships between bounded contexts.
- **Anti-Corruption Layers** protect bounded contexts from external systems.

## Tactical Design

- **Aggregates** enforce consistency boundaries around related concepts.
- **Entities** represent domain objects with unique identity.
- **Value Objects** capture immutable measurements, classifications, and types.
- **Domain Events** signal significant occurrences that trigger workflows across contexts.
- **Repositories** abstract the persistence of aggregate roots.
- **Domain Services** encapsulate operations spanning multiple aggregates.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- Department for Constitutional Affairs. *Mental Capacity Act 2005: Code of Practice*. The Stationery Office, 2007.
- Sabatino, Charles P. "The Evolution of Health Care Advance Planning Law and Policy." *The Milbank Quarterly*, vol. 88, no. 2, 2010, pp. 211-239.
