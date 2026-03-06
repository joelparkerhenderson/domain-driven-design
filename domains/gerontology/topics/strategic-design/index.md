# Strategic Design in the Gerontology Domain

## Overview

Strategic design in Domain-Driven Design addresses how to decompose a large, complex system into manageable bounded contexts. In the gerontology domain, strategic design is essential because geriatric care spans multiple clinical disciplines, care settings, regulatory frameworks, and stakeholder groups. The complexity of aging-related care demands careful boundary definition to ensure that each part of the system maintains internal consistency while integrating effectively with the whole.

Gerontology presents unique strategic challenges. An older adult's care journey may simultaneously involve geriatric assessment, cognitive screening, medication review, and end-of-life planning. Each of these areas has its own vocabulary, clinical workflows, and regulatory requirements. Strategic design provides the tools to model these areas as distinct bounded contexts while preserving the ability to coordinate care across boundaries.

## Decomposition Approach

The gerontology domain is decomposed into six bounded contexts, each reflecting a natural organizational and clinical boundary in geriatric care. This decomposition follows the principle that bounded contexts should align with team structures and areas of expertise, as described by Evans (2003) and reinforced by Conway's Law.

The six contexts are Geriatric Assessment, Care Coordination, Cognitive Health, Functional Independence, Medication Management, and End of Life Planning. Each context owns its models, enforces its invariants, and communicates with other contexts through well-defined interfaces and domain events.

## Strategic Patterns Applied

Context Mapping defines the relationships between bounded contexts. In gerontology, the Geriatric Assessment Context often acts as an upstream context, producing assessment results consumed by downstream contexts such as Care Coordination and Medication Management.

Subdomain classification distinguishes core subdomains (geriatric assessment, care coordination) that provide competitive advantage from supporting subdomains (medication management, functional independence) and generic subdomains (scheduling, notifications) that can leverage existing solutions.

Anti-corruption layers protect each bounded context from the models and assumptions of external systems, such as hospital EHR systems, pharmacy benefit managers, and insurance claim processors. These layers translate between the ubiquitous language of the gerontology domain and the terminology used by external systems.

## Ubiquitous Language

Strategic design begins with establishing a ubiquitous language shared among geriatricians, nurses, social workers, caregivers, and software developers. In gerontology, terms like "comprehensive geriatric assessment," "frailty index," "polypharmacy," and "goals of care" must have precise, agreed-upon meanings within their respective bounded contexts. The ubiquitous language eliminates ambiguity and ensures that the software model accurately reflects the clinical domain.

## Alignment with Organizational Structure

Geriatric care teams are inherently interdisciplinary. A geriatrician, nurse practitioner, pharmacist, social worker, physical therapist, and psychologist may all participate in a patient's care. Strategic design aligns bounded contexts with the areas of expertise these professionals bring, ensuring that each context is owned by a team with deep domain knowledge.

Care coordination, for example, is often led by geriatric care managers or social workers. Medication management is the province of pharmacists and prescribers. Cognitive health assessment falls to neuropsychologists and geriatric psychiatrists. This alignment ensures that the people closest to the domain inform the models within each bounded context.

## Key Considerations

Geriatric care is longitudinal, spanning years or decades. Strategic design must account for the fact that an older adult's needs evolve over time, moving from preventive screening to chronic disease management to end-of-life care. The bounded contexts must support this progression without requiring wholesale model redesign.

The domain must also accommodate the fragmented nature of healthcare delivery. Older adults receive care from multiple providers across hospitals, clinics, skilled nursing facilities, home health agencies, and community programs. Strategic design addresses this fragmentation through context mapping and integration patterns that enable data sharing while respecting organizational boundaries.

## Relationship to Tactical Design

Strategic design establishes the boundaries within which tactical patterns operate. Aggregates, entities, value objects, and domain events are defined within the scope of a bounded context. Without clear strategic boundaries, tactical patterns risk becoming entangled across domains, leading to the "big ball of mud" anti-pattern that Evans (2003) warns against.

In the gerontology domain, strategic design ensures that a "patient" in the Geriatric Assessment Context can have different attributes and behaviors than a "patient" in the Medication Management Context, reflecting the different concerns each context addresses.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media.
- Inouye, S.K., Studenski, S., Tinetti, M.E., & Kuchel, G.A. (2007). Geriatric Syndromes: Clinical, Research, and Policy Implications. Journal of the American Geriatrics Society.
