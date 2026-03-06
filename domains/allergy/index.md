# Allergy Domain

## Introduction

This domain applies Domain-Driven Design (DDD) to allergy and immunology clinical practice. Allergy is a branch of medicine concerned with the diagnosis and management of hypersensitivity reactions mediated by the immune system, affecting an estimated 30-40% of the global population. Allergic conditions range from mild seasonal rhinitis to life-threatening anaphylaxis, requiring careful identification, documentation, and management across all healthcare settings.

The allergy domain encompasses allergen identification and classification, diagnostic assessment, reaction management, treatment and immunotherapy, and patient safety alerting. Each area operates as a distinct bounded context with its own models, terminology, and workflows, while sharing common domain concepts through well-defined integration patterns.

## Bounded Contexts

The allergy domain is decomposed into 5 bounded contexts:

1. **Allergen Registry** - Manages the comprehensive catalog of known allergens, including their classification by source (food, drug, environmental, insect venom, contact), molecular component profiles, and cross-reactivity relationships. This context serves as the authoritative reference for allergen identification across the domain.

2. **Diagnostic Assessment** - Governs the clinical evaluation of allergic conditions, encompassing patient history taking, skin prick testing, specific IgE blood testing, component-resolved diagnostics, patch testing, and oral food challenges. This context manages the workflow from clinical suspicion through confirmed diagnosis.

3. **Reaction Management** - Handles the documentation, clinical grading, and acute management of allergic reactions. This context captures reaction severity using standardized scales, manages anaphylaxis emergency protocols, and maintains a patient's reaction history to inform future clinical decisions.

4. **Treatment and Immunotherapy** - Manages ongoing treatment strategies including allergen avoidance plans, pharmacotherapy regimens (antihistamines, corticosteroids, epinephrine auto-injectors, biologics such as omalizumab), and allergen immunotherapy programs (subcutaneous and sublingual routes).

5. **Patient Safety and Alerts** - Governs the creation, maintenance, and dissemination of allergy alerts across healthcare systems, ensuring that prescribing, dispensing, and care delivery workflows include appropriate checks to prevent inadvertent allergen exposure.

## Strategic Design

- **Subdomains** classify areas by strategic importance: Diagnostic Assessment and Reaction Management are core subdomains; Allergen Registry and Treatment and Immunotherapy are supporting; Patient Safety and Alerts is generic.
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
- Pawankar, Ruby, et al. *WAO White Book on Allergy: Update 2013*. World Allergy Organization, 2013.
- Muraro, Antonella, et al. "EAACI Guidelines on Allergen Immunotherapy." *Allergy*, vol. 73, no. 4, 2018, pp. 765-798.
