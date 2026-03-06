# Strategic Design in Medical Practice

## Overview

Strategic design in Domain-Driven Design (DDD) addresses how to decompose a large, complex medical practice system into manageable parts that align with clinical workflows, regulatory requirements, and care delivery models. It focuses on identifying the most important areas of the domain, defining clear boundaries between subsystems, and establishing how those subsystems communicate while maintaining patient safety and data integrity.

## Relevance to Medical Practice

Medical practice systems span clinical care, diagnostics, pharmacy, insurance reimbursement, and telemedicine. Strategic design provides tools to manage this complexity by:

- Aligning software models with how clinicians, pharmacists, and administrators actually think about their work
- Separating concerns so that changes in insurance claims processing do not ripple into clinical decision support
- Identifying which parts of the system deserve the most investment and the best engineering expertise
- Enabling independent teams to work on different bounded contexts without constant coordination
- Ensuring regulatory compliance boundaries are explicitly modeled

## Key Concepts

### Knowledge Crunching

Knowledge crunching is the collaborative process of extracting domain knowledge from medical professionals and translating it into a software model. In medical practice, this means:

- Conducting workshops with physicians, pharmacists, radiologists, and claims specialists
- Observing clinical workflows such as patient intake, order entry, and prescription fulfillment
- Iteratively refining the model as understanding deepens
- Resolving terminology conflicts (e.g., "order" means different things in clinical, pharmacy, and imaging contexts)

### Domain Vision Statement

A domain vision statement captures the core purpose of the system. For a medical practice system:

> "To support safe, evidence-based clinical care by modeling patient records, clinical workflows, diagnostics, medications, insurance claims, and telemedicine as first-class domain concepts, enabling clinicians to focus on patient outcomes while maintaining regulatory compliance and interoperability."

### Core Domain Identification

Not all parts of the medical system are equally important. Strategic design distinguishes:

- **Core Domain**: Clinical workflow coordination, clinical decision support, and medication safety -- the areas that directly impact patient outcomes and differentiate the system
- **Supporting Subdomains**: Insurance claims processing, referral management, telehealth scheduling -- necessary but not the primary clinical differentiator
- **Generic Subdomains**: Authentication, notification services, audit logging -- commoditized capabilities available as off-the-shelf solutions

### Distillation

Distillation separates essential complexity from accidental complexity. In medical practice:

- Clinical decision support logic is core and requires custom domain modeling
- Claims adjudication rules are supporting and may follow established industry patterns
- User authentication is generic and should use standard identity providers

## Medical Domain Decomposition

Strategic design guides the medical system into six bounded contexts:

- **Patient Records Context** -- EHR data, demographics, medical history, problem lists, identity management
- **Clinical Workflow Context** -- Order entry, clinical decision support, care plans, referrals, encounters
- **Diagnostics & Imaging Context** -- Lab orders, results, radiology studies, pathology reports, DICOM integration
- **Pharmacy & Medication Context** -- Prescriptions, drug interactions, formulary management, medication administration
- **Insurance & Claims Context** -- Coverage verification, claims submission, adjudication, explanation of benefits
- **Telemedicine Context** -- Virtual visits, remote patient monitoring, telehealth scheduling, device integration

Each context has its own model, language, and team ownership, reducing cognitive load and enabling independent evolution.

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) -- The primary output of strategic design
- [Context Map](../context-map/) -- Visualizes relationships between bounded contexts
- [Subdomains](../subdomains/) -- Classification of domain areas by strategic importance
- [Ubiquitous Language](../ubiquitous-language/) -- Shared vocabulary within each bounded context
- [Anti-Corruption Layer](../anti-corruption-layer/) -- Protects context boundaries from external system leakage

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapters 14-16. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Part I: Strategic Design. Addison-Wesley, 2013.
- Vernon, Vaughn. "Domain-Driven Design Distilled." Addison-Wesley, 2016.
- Millett, Scott & Tune, Nick. "Patterns, Principles, and Practices of Domain-Driven Design." Part III. Wrox, 2015.
- Brandolini, Alberto. "Introducing EventStorming." Leanpub, 2021.
- Benson, Tim & Grieve, Grahame. "Principles of Health Interoperability." Springer, 2021.
