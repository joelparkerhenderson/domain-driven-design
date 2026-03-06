# Advance Statement About Care Domain

## Introduction

This domain applies Domain-Driven Design (DDD) to advance statements about care, which are documents that allow a person to record their wishes, preferences, values, beliefs, and feelings about their future care. Unlike advance decisions to refuse treatment, advance statements are not legally binding, but healthcare professionals are required to take them into account when making best interests decisions on behalf of a person who has lost capacity.

The advance statement about care domain encompasses statement authoring, care planning, stakeholder engagement, clinical interpretation, and review processes. Each area operates as a distinct bounded context with its own models, terminology, and workflows, while sharing common domain concepts through well-defined integration patterns.

## Bounded Contexts

The advance statement about care domain is decomposed into 5 bounded contexts:

1. **Statement Authoring** - Captures the person's wishes, preferences, values, beliefs, and feelings about their future care. This context provides structured and narrative formats for expressing what matters most to the individual, including preferences about place of care, daily routines, social interactions, and spiritual or cultural practices.

2. **Care Planning** - Manages the translation of a person's stated wishes into actionable care plans that can guide healthcare professionals. This context bridges the gap between expressed preferences and the practical realities of clinical care delivery.

3. **Stakeholder Engagement** - Governs the consultation and communication with family members, carers, advocates, and healthcare professionals who are involved in understanding and honoring the person's wishes. This context ensures that all relevant parties are aware of the statement and have contributed where appropriate.

4. **Clinical Interpretation** - Handles how healthcare professionals interpret and apply advance statements within the clinical context, balancing the person's expressed wishes against clinical judgment, available resources, and the best interests framework.

5. **Review and Update** - Manages the periodic review and amendment of advance statements to ensure they remain current and accurately reflect the person's evolving preferences, particularly after significant life events or changes in health status.

## Strategic Design

- **Subdomains** classify areas by strategic importance: Statement Authoring and Clinical Interpretation are core subdomains; Care Planning and Stakeholder Engagement are supporting; Review and Update is generic.
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
- NHS England. *Universal Principles for Advance Care Planning*. NHS, 2022.
- Sudore, Rebecca L., et al. "Defining Advance Care Planning for Adults: A Consensus Definition from a Multidisciplinary Delphi Panel." *Journal of Pain and Symptom Management*, vol. 53, no. 5, 2017, pp. 821-832.
