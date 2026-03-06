# Strategic Design in Oncology

## Overview

Strategic design in Domain-Driven Design addresses how to decompose a large oncology system into manageable bounded contexts, each reflecting the natural organizational and clinical boundaries of cancer care delivery. Oncology is inherently multidisciplinary, involving distinct specialties that collaborate around a shared patient but maintain their own models, vocabularies, and workflows. Strategic design captures this reality by defining explicit boundaries, mapping relationships between contexts, and classifying subdomains by their strategic importance to the organization.

## Decomposing Oncology into Bounded Contexts

The oncology domain naturally decomposes into six bounded contexts that mirror how cancer care is organized in clinical practice. The Diagnosis and Staging Context handles the initial identification and classification of cancer. The Treatment Planning Context orchestrates multidisciplinary decision-making. The Chemotherapy Management, Radiation Therapy, and Surgical Oncology Contexts each manage a distinct treatment modality. The Survivorship Care Context addresses long-term follow-up after active treatment.

This decomposition follows the DDD principle that bounded contexts should align with organizational boundaries and team structures. In oncology, pathologists own the diagnostic model, radiation oncologists own the radiation treatment model, and medical oncologists own the chemotherapy model. Each team has its own specialized language, and strategic design respects these natural divisions rather than forcing a single unified model.

## Context Mapping in Oncology

The relationships between oncology bounded contexts are defined through a context map. The Diagnosis and Staging Context acts as an upstream context, publishing staging results that the Treatment Planning Context consumes. The Treatment Planning Context in turn acts as upstream to the three treatment modality contexts, providing approved treatment plans that each modality context translates into modality-specific orders.

Key relationship patterns in oncology include the customer-supplier pattern between Treatment Planning and the modality contexts, the published language pattern using TNM staging and NCCN guidelines, and the anti-corruption layer pattern protecting oncology contexts from legacy laboratory and imaging systems.

## Subdomain Classification

Core subdomains in oncology represent the areas of highest strategic value and competitive differentiation. Treatment planning, where multidisciplinary expertise is applied to create individualized care strategies, is a core subdomain. Chemotherapy dose optimization and radiation treatment planning, which require sophisticated clinical algorithms, are also core subdomains.

Supporting subdomains provide necessary capabilities that enable the core but are not differentiators themselves. Pathology reporting, which follows standardized formats, and survivorship care planning, which applies published guidelines, are supporting subdomains. They require domain knowledge but follow well-established patterns.

Generic subdomains can be addressed with off-the-shelf solutions. Patient demographics, scheduling infrastructure, and document management are generic subdomains in oncology that do not require specialized domain modeling.

## Shared Kernel and Published Language

Oncology bounded contexts share certain foundational concepts through a shared kernel. The patient identity, represented as a medical record number, is shared across all contexts. The cancer diagnosis, including histology and primary site, is a published language element that all contexts reference.

Published languages in oncology are well-established clinical standards. TNM staging provides a standardized classification system understood across all contexts. NCCN guidelines provide a shared reference for treatment recommendations. CTCAE grading provides a common toxicity assessment language. These published languages reduce translation overhead between contexts and anchor the ubiquitous language in clinically validated terminology.

## Strategic Design Decisions

Strategic design in oncology must account for several domain-specific considerations. The temporal nature of cancer care means that context boundaries may shift as a patient progresses from diagnosis through treatment to survivorship. The regulatory environment imposes constraints on how contexts can share data, particularly regarding clinical trial data and protected health information.

The high-stakes nature of oncology care means that consistency boundaries must be carefully designed. Treatment orders that depend on staging information must have well-defined consistency guarantees. Domain events that trigger safety-critical workflows, such as toxicity alerts that may require treatment modification, must be designed with appropriate delivery guarantees.

## Alignment with Organizational Structure

Strategic design in oncology should reflect the organizational structure of cancer centers. Most cancer centers organize around disease sites (breast, lung, gastrointestinal) and treatment modalities (medical oncology, radiation oncology, surgical oncology). The bounded context model aligns with the modality-based organization, while disease-site specialization is captured within each context through subdomains or modules.

This alignment ensures that the teams responsible for each bounded context have the domain expertise to maintain and evolve the model. It also supports Conway's Law, which observes that system architecture tends to mirror organizational communication structures.

## Key Principles

Strategic design in oncology follows several guiding principles. First, let clinical workflow boundaries drive context boundaries rather than imposing arbitrary technical divisions. Second, use published clinical standards as published languages to anchor shared understanding. Third, protect each context's model integrity through explicit translation at boundaries. Fourth, classify subdomains honestly to focus investment on areas of genuine strategic value.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 14: Maintaining Model Integrity.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 2: Domains, Subdomains, and Bounded Contexts.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Part II: Strategic Design.
- Brandolini, Alberto. Introducing EventStorming. Leanpub, 2021.
