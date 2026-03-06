# Allergy Domain - AGENTS.md

## Overview

This domain applies Domain-Driven Design (DDD) to allergy and immunology clinical practice, encompassing allergen identification and classification, diagnostic testing and assessment, allergic reaction management and treatment, immunotherapy and desensitization, patient education and allergen avoidance, and anaphylaxis emergency response.

## Bounded Contexts

1. Allergen Registry - Manages the catalog of known allergens including their classification (food, drug, environmental, insect venom, contact), molecular profiles, and cross-reactivity relationships.
2. Diagnostic Assessment - Governs the clinical evaluation of allergic conditions including patient history, skin prick testing, specific IgE blood tests, patch testing, and oral food challenges.
3. Reaction Management - Handles the documentation, grading, and acute management of allergic reactions ranging from mild localized responses to severe systemic anaphylaxis.
4. Treatment and Immunotherapy - Manages ongoing treatment plans including allergen avoidance strategies, pharmacotherapy (antihistamines, corticosteroids, biologics), and allergen immunotherapy (subcutaneous and sublingual).
5. Patient Safety and Alerts - Governs the creation, maintenance, and dissemination of allergy alerts across healthcare systems to prevent inadvertent allergen exposure during prescribing, dispensing, and care delivery.

## Domain Principles

- Model using shared ubiquitous language between allergists, immunologists, primary care physicians, pharmacists, nurses, and patients.
- Group the system into bounded contexts reflecting clinical allergy workflows from diagnosis through treatment.
- Business logic remains pure and independent of infrastructure (database, UI, web, mobile).
- Follow NICE Clinical Guidelines for allergy, World Allergy Organization (WAO) standards, and EAACI guidelines.
- Maintain patient safety, drug interaction checking, and clinical decision support as cross-cutting concerns.

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
- allergen-registry
- diagnostic-assessment
- reaction-management
- treatment-and-immunotherapy
- patient-safety-and-alerts
- event-driven-architecture
- integration-patterns
- allergy-standards
- regulatory-compliance

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- Pawankar, Ruby, et al. WAO White Book on Allergy: Update 2013. World Allergy Organization, 2013.
- Muraro, Antonella, et al. "EAACI Guidelines on Allergen Immunotherapy." Allergy, vol. 73, no. 4, 2018, pp. 765-798.
