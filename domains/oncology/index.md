# Oncology Domain - Domain-Driven Design

## Overview

This domain applies Domain-Driven Design (DDD) to oncology systems, modeling the full cancer care continuum from initial screening and diagnosis through treatment delivery and survivorship. The oncology domain is characterized by complex multidisciplinary workflows, rigorous regulatory requirements, evidence-based treatment protocols, and the critical need for coordinated communication across specialized care teams.

DDD provides a natural framework for oncology because cancer care is inherently organized into distinct bounded contexts, each with its own specialized vocabulary, workflows, and expert practitioners. The ubiquitous language bridges the gap between oncology domain experts (oncologists, pathologists, radiation therapists, pharmacists, surgical oncologists, nurses) and software developers building systems to support cancer care delivery.

## Bounded Contexts

1. **Diagnosis & Staging Context** - Encompasses cancer screening, biopsy procedures, pathology analysis, TNM staging classification, and molecular profiling. This context establishes the foundational clinical picture that drives all downstream treatment decisions.

2. **Treatment Planning Context** - Covers multidisciplinary tumor board review, treatment strategy formulation, clinical trial eligibility assessment, and alignment with NCCN guidelines. This context orchestrates the collaborative decision-making process among oncology specialists.

3. **Chemotherapy Management Context** - Manages regimen selection, dose calculation based on patient parameters, infusion scheduling, pre-medication protocols, and toxicity monitoring using CTCAE grading. This context ensures safe and effective systemic therapy delivery.

4. **Radiation Therapy Context** - Handles simulation and imaging for treatment planning, dose calculation and optimization, fractionation scheduling, image-guided radiation therapy (IGRT), and brachytherapy. This context manages the precise delivery of therapeutic radiation.

5. **Surgical Oncology Context** - Addresses resection planning, intraoperative decision-making, margin assessment, sentinel node biopsy, and reconstructive planning. This context manages the surgical interventions in cancer treatment.

6. **Survivorship Care Context** - Manages follow-up surveillance protocols, late effects monitoring, psychosocial support coordination, and survivorship care plan generation. This context supports patients after active treatment completion.

## Topics

### Strategic Design

- [Strategic Design](topics/strategic-design/index.md)
- [Bounded Contexts](topics/bounded-contexts/index.md)
- [Context Map](topics/context-map/index.md)
- [Ubiquitous Language](topics/ubiquitous-language/index.md)
- [Subdomains](topics/subdomains/index.md)
- [Anti-Corruption Layer](topics/anti-corruption-layer/index.md)

### Tactical Design

- [Aggregates](topics/aggregates/index.md)
- [Entities](topics/entities/index.md)
- [Value Objects](topics/value-objects/index.md)
- [Domain Events](topics/domain-events/index.md)
- [Repositories](topics/repositories/index.md)
- [Domain Services](topics/domain-services/index.md)

### Bounded Context Details

- [Diagnosis & Staging Context](topics/diagnosis-staging-context/index.md)
- [Treatment Planning Context](topics/treatment-planning-context/index.md)
- [Chemotherapy Management Context](topics/chemotherapy-management-context/index.md)
- [Radiation Therapy Context](topics/radiation-therapy-context/index.md)
- [Surgical Oncology Context](topics/surgical-oncology-context/index.md)
- [Survivorship Care Context](topics/survivorship-care-context/index.md)

### Cross-Cutting Concerns

- [Event-Driven Architecture](topics/event-driven-architecture/index.md)
- [Integration Patterns](topics/integration-patterns/index.md)
- [Oncology Standards](topics/oncology-standards/index.md)
- [Regulatory Compliance](topics/regulatory-compliance/index.md)

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- American Joint Committee on Cancer (AJCC). AJCC Cancer Staging Manual, 8th Edition. Springer, 2017.
- National Comprehensive Cancer Network (NCCN). NCCN Clinical Practice Guidelines in Oncology. https://www.nccn.org/guidelines
- National Cancer Institute (NCI). Common Terminology Criteria for Adverse Events (CTCAE) v5.0. 2017.
