# Subdomains - Otolaryngology Domain

## Overview

Subdomains classify areas of a domain by their strategic importance to the organization. Domain-Driven Design distinguishes three types: core subdomains that provide competitive differentiation, supporting subdomains that are necessary but not differentiating, and generic subdomains that can be addressed with off-the-shelf solutions. In otolaryngology, this classification guides investment decisions about where to build custom domain models versus where to leverage existing solutions.

## Core Subdomains

Core subdomains represent the areas where the otolaryngology system must excel to provide unique clinical value. These require the deepest domain modeling, the most careful ubiquitous language development, and the highest investment in domain expert collaboration.

### Clinical Assessment Context

Clinical assessment is core because it embodies the diagnostic reasoning that differentiates ENT practice. The ability to model complex diagnostic workflows, correlate multi-modal findings (examination, endoscopy, audiometry, imaging), and support clinical decision-making is where the domain model provides the greatest value. A generic assessment tool cannot capture the nuanced relationships between an endoscopic finding, an audiometric pattern, and an imaging result that together point to a specific diagnosis.

### Surgical Management Context

Surgical management is core because it captures the complexity of ENT surgical decision-making, procedural planning, and outcome tracking. Each surgical procedure (tonsillectomy, septoplasty, FESS, thyroidectomy, head and neck oncologic resection) has unique pre-operative requirements, intra-operative considerations, and post-operative protocols. Modeling these workflows with domain-level fidelity enables evidence-based surgical planning and outcomes analysis.

## Supporting Subdomains

Supporting subdomains are necessary for the otolaryngology system to function but follow more established clinical protocols. They require custom modeling but benefit from more standardized patterns.

### Voice Disorders Context

Voice disorders management follows well-established evaluation and treatment protocols (laryngoscopic examination, voice therapy, phonosurgery indications). While custom modeling is needed to capture the relationship between laryngoscopic findings and voice therapy protocols, the clinical workflows are more standardized than core subdomains. The Voice Handicap Index and voice therapy protocols follow published evidence-based guidelines.

### Sleep Disorders Context

Sleep medicine in the ENT context follows established diagnostic criteria (AHI thresholds, Epworth Sleepiness Scale) and treatment protocols (CPAP titration, UPPP indications). The domain model captures these protocols faithfully but benefits from alignment with well-documented clinical guidelines from the American Academy of Sleep Medicine.

### Allergy Management Context

Allergy testing and immunotherapy follow standardized protocols (skin test interpretation criteria, immunotherapy dosing schedules, safety monitoring requirements). While the relationship between allergy sensitization patterns and treatment selection requires custom modeling, the underlying protocols are well-established and documented in practice parameters from the Joint Task Force on Practice Parameters.

### Hearing Services Context

Audiological evaluation and hearing aid fitting follow standardized protocols (audiometric test procedures, hearing aid verification methods, cochlear implant candidacy criteria). The domain model captures these protocols and the longitudinal tracking of hearing status, but the clinical workflows align with published audiological best practices from the American Academy of Audiology.

## Generic Subdomains

Generic subdomains in the otolaryngology domain can leverage off-the-shelf or commodity solutions.

### Patient Identity Management

Managing patient demographics, insurance information, and contact details is a generic concern shared across all healthcare domains. This can be addressed through integration with existing patient registration systems.

### Scheduling

Appointment scheduling for clinical visits, surgical cases, and follow-up appointments is a generic concern that can be handled by external scheduling systems integrated through anti-corruption layers.

### Billing and Coding

While each bounded context produces billable events, the actual billing workflow (claim submission, payment posting, denial management) is generic and best handled by specialized billing systems. The domain model translates clinical events into billing codes (CPT, ICD-10) at the boundary.

### Document Management

Storage and retrieval of clinical documents, imaging files, and audiometric records is a generic infrastructure concern handled by document management or PACS systems.

## Strategic Investment Implications

| Subdomain Type | Investment Level | Domain Expert Involvement | Custom Modeling |
|---------------|-----------------|--------------------------|-----------------|
| Core | Highest | Deep, ongoing collaboration | Fully custom |
| Supporting | Moderate | Regular consultation | Custom with standard patterns |
| Generic | Minimal | Requirements only | Off-the-shelf integration |

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 15 on distilling the core domain.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 2 on subdomains.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 1 on subdomain classification.
