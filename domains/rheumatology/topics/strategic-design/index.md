# Strategic Design in the Rheumatology Domain

## Overview

Strategic design in Domain-Driven Design addresses how to decompose a large, complex system into manageable bounded contexts. In the rheumatology domain, strategic design is essential because the field spans multiple clinical disciplines, complex treatment protocols, and diverse outcome measures. The goal is to identify natural boundaries that align with how rheumatologists, nurses, pharmacists, and therapists actually work, ensuring that each bounded context has a coherent model and consistent terminology.

## Decomposition Approach

The rheumatology domain decomposes into six bounded contexts based on clinical workflow analysis. Each context represents a distinct area of clinical responsibility with its own model, rules, and ubiquitous language. The decomposition follows the principle that each context should be small enough to be understood by a single team yet large enough to encapsulate a meaningful clinical capability.

The primary axis of decomposition is the clinical workflow: assessment leads to diagnosis, diagnosis informs treatment selection, treatment requires monitoring, and monitoring feeds back into assessment. Secondary axes include the distinction between autoimmune and non-autoimmune joint diseases, and the separation of drug therapy management from broader pain management.

## Context Identification

The Clinical Assessment Context was identified as a distinct boundary because joint examination and serology interpretation follow standardized protocols (ACR/EULAR criteria) that are independent of specific disease management strategies. Assessment produces clinical data consumed by multiple downstream contexts.

The Autoimmune Disease Context and Joint Disease Context were separated because they involve fundamentally different pathophysiology, classification criteria, and treatment approaches. Rheumatoid arthritis and lupus require immunomodulatory strategies, while osteoarthritis and gout follow different therapeutic pathways.

The Biologic Therapy Context was isolated because biologic prescribing involves unique regulatory requirements, safety monitoring protocols, and infusion management logistics that cross-cut multiple disease contexts. This context has its own lifecycle distinct from disease management.

The Pain Management Context was separated because pain is a cross-cutting concern addressed through multimodal strategies involving physical therapy, occupational therapy, and pharmacologic interventions that serve patients across all disease categories.

The Outcomes Tracking Context was distinguished because longitudinal outcome measurement follows standardized instruments (DAS28, HAQ-DI, radiographic scoring) and serves quality reporting, research, and treat-to-target decision-making.

## Strategic Patterns Applied

Bounded Contexts define clear linguistic and model boundaries. The term "disease activity" means different things in the Autoimmune Disease Context (SLEDAI for lupus, DAS28 for RA) versus the Joint Disease Context (pain and function scores for osteoarthritis). Each context maintains its own definition.

The Context Map documents how contexts interact. The Clinical Assessment Context has a customer-supplier relationship with both disease contexts, providing assessment data they consume. The Biologic Therapy Context has a conformist relationship with external drug registry systems.

Subdomains are classified by strategic importance. Disease management and clinical assessment are core subdomains where competitive advantage lies. Pain management is a supporting subdomain. Generic subdomains include patient demographics and scheduling.

Anti-Corruption Layers protect each context from external system pollution. The Biologic Therapy Context uses an ACL to translate between internal therapy models and external pharmacy system concepts. The Outcomes Tracking Context uses an ACL to normalize data from diverse assessment instruments.

## Domain Expert Collaboration

Strategic design in rheumatology requires collaboration with domain experts including rheumatologists who understand disease classification, pharmacists who understand biologic therapy protocols, physical therapists who understand rehabilitation workflows, and clinical informaticists who understand outcome measurement standards.

The ubiquitous language emerges from these collaborative sessions. Terms like "disease flare," "treat-to-target," "biosimilar switching," and "radiographic progression" must have precise, shared definitions that both clinicians and developers use consistently.

## Alignment with Clinical Practice

Strategic design decisions align with how rheumatology practices actually operate. Assessment clinics are often separate from infusion centers. Outcome tracking serves both clinical care and quality reporting mandates. Pain management involves referrals to external therapy providers. These real-world organizational boundaries validate the bounded context decomposition.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapters 14-15.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapters 2-4.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapters 6-8.
- Brandolini, Alberto. "Introducing EventStorming." Leanpub, 2021.
- Smolen, J.S., et al. "Treating Rheumatoid Arthritis to Target: 2014 Update." Annals of the Rheumatic Diseases, 2016.
