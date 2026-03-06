# Advance Statement About Care Domain - AGENTS.md

## Overview

This domain applies Domain-Driven Design (DDD) to advance statements about care (also known as advance care statements or statements of wishes and preferences), encompassing patient preferences and values documentation, care planning and goals of care, stakeholder engagement and shared decision-making, clinical care coordination, cultural and spiritual considerations, and review and update processes.

## Bounded Contexts

1. Statement Authoring - Captures the person's wishes, preferences, values, beliefs, and feelings about their future care, including what matters most to them and how they would like to be cared for.
2. Care Planning - Manages the translation of a person's stated wishes into actionable care plans, coordinating between the patient's preferences and clinical possibilities.
3. Stakeholder Engagement - Governs communication and consultation with family members, carers, advocates, and healthcare professionals involved in understanding and honoring the person's wishes.
4. Clinical Interpretation - Handles how healthcare professionals interpret and apply advance statements within the clinical context, recognizing that advance statements are not legally binding but must be taken into account.
5. Review and Update - Manages the periodic review and amendment of advance statements to ensure they remain current and reflective of the person's evolving preferences and circumstances.

## Domain Principles

- Model using shared ubiquitous language between patients, carers, clinicians, social workers, chaplains, and advocates.
- Group the system into bounded contexts reflecting person-centered care planning and healthcare delivery workflows.
- Business logic remains pure and independent of infrastructure (database, UI, web, mobile).
- Follow NHS Advance Care Planning guidance, Mental Capacity Act 2005 best interests checklist, and equivalent jurisdictional frameworks.
- Maintain patient dignity, data protection (GDPR/HIPAA), and person-centered care as cross-cutting concerns.

## Key Topics

- strategic-design
- bounded-contexts
- context-map
- ubiquitous-language
- subdomains
- anti-corruption-layer
- aggregates
- entities
- value-objects
- domain-events
- repositories
- domain-services
- statement-authoring
- care-planning
- stakeholder-engagement
- clinical-interpretation
- review-and-update
- event-driven-architecture
- integration-patterns
- advance-care-planning-standards
- regulatory-compliance

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- NHS England. Universal Principles for Advance Care Planning. NHS, 2022.
- Sudore, Rebecca L., et al. "Defining Advance Care Planning for Adults: A Consensus Definition from a Multidisciplinary Delphi Panel." Journal of Pain and Symptom Management, vol. 53, no. 5, 2017, pp. 821-832.
