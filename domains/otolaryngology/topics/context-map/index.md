# Context Map - Otolaryngology Domain

## Overview

A context map provides a visual and conceptual representation of the relationships and interactions between bounded contexts. In the otolaryngology domain, the context map documents how the six bounded contexts communicate, share data, and maintain their boundaries. It identifies upstream and downstream relationships, integration patterns, and translation mechanisms used at each boundary.

## Context Relationships

### Clinical Assessment Context (Upstream)

The Clinical Assessment Context is the primary upstream context, producing diagnostic information consumed by all other contexts. It acts as an open host service, publishing standardized clinical findings that downstream contexts can consume.

- **To Surgical Management**: Supplies pre-operative diagnostic findings, imaging results, and clinical assessments. Relationship: Customer-Supplier, where Surgical Management is the customer requesting specific diagnostic workups.
- **To Voice Disorders**: Provides laryngoscopic examination results and initial voice assessments. Relationship: Customer-Supplier.
- **To Sleep Disorders**: Supplies airway examination findings, neck circumference measurements, and Mallampati scores. Relationship: Customer-Supplier.
- **To Allergy Management**: Provides nasal endoscopy findings, CT sinus imaging, and symptom assessments. Relationship: Customer-Supplier.
- **To Hearing Services**: Supplies initial audiometric screening results and otoscopic examination findings. Relationship: Customer-Supplier.

### Surgical Management Context

The Surgical Management Context consumes diagnostic data from Clinical Assessment and produces surgical outcome data consumed by subspecialty contexts.

- **From Clinical Assessment**: Receives pre-operative diagnostic workup. Relationship: Customer-Supplier (downstream customer).
- **To Hearing Services**: Publishes surgical outcome events for cochlear implant and ear surgery cases. Relationship: Partnership, as both contexts collaborate closely on surgical hearing rehabilitation.
- **To Voice Disorders**: Publishes phonosurgery outcomes and post-operative laryngeal status. Relationship: Partnership.

### Voice Disorders Context

- **From Clinical Assessment**: Receives laryngoscopic findings and initial voice screening. Relationship: Customer-Supplier (downstream customer).
- **From Surgical Management**: Receives phonosurgery outcomes for ongoing voice rehabilitation. Relationship: Partnership.
- **To Hearing Services**: Shares voice assessment data for patients with combined voice and hearing disorders. Relationship: Shared Kernel for common patient communication assessment concepts.

### Sleep Disorders Context

- **From Clinical Assessment**: Receives airway examination findings and initial sleep screening results. Relationship: Customer-Supplier (downstream customer).
- **To Surgical Management**: Requests surgical consultation for patients failing conservative therapy (UPPP, tongue base procedures). Relationship: Customer-Supplier (Sleep Disorders as customer).
- **To Allergy Management**: Shares nasal obstruction data for patients where allergic rhinitis contributes to sleep-disordered breathing. Relationship: Partnership.

### Allergy Management Context

- **From Clinical Assessment**: Receives nasal endoscopy findings and imaging results. Relationship: Customer-Supplier (downstream customer).
- **From Sleep Disorders**: Receives nasal obstruction assessments for patients with concurrent allergic rhinitis and sleep apnea. Relationship: Partnership.
- **To Surgical Management**: Requests sinus surgery consultation for medically refractory chronic rhinosinusitis. Relationship: Customer-Supplier (Allergy Management as customer).

### Hearing Services Context

- **From Clinical Assessment**: Receives otoscopic findings and initial audiometric screening. Relationship: Customer-Supplier (downstream customer).
- **From Surgical Management**: Receives surgical outcomes for cochlear implant and ear reconstruction cases. Relationship: Partnership.
- **From Voice Disorders**: Receives voice assessment data for patients with combined disorders. Relationship: Shared Kernel.

## External System Integrations

### Electronic Health Record (EHR)

All bounded contexts integrate with external EHR systems through an anti-corruption layer. The EHR integration uses HL7 FHIR resources as a published language, translating domain-specific concepts into standardized clinical data exchange formats.

### Laboratory Information System (LIS)

The Allergy Management Context integrates with laboratory systems for serum-specific IgE results. An anti-corruption layer translates laboratory result formats into domain-specific allergy test result value objects.

### Billing and Coding Systems

All contexts integrate with billing systems through a conformist relationship, conforming to CPT and ICD-10 coding standards. Each context translates its domain events into billable encounters using standardized code sets.

## Integration Patterns Summary

| Upstream | Downstream | Pattern | Translation |
|----------|-----------|---------|-------------|
| Clinical Assessment | Surgical Management | Customer-Supplier | Anti-Corruption Layer |
| Clinical Assessment | Voice Disorders | Customer-Supplier | Anti-Corruption Layer |
| Clinical Assessment | Sleep Disorders | Customer-Supplier | Anti-Corruption Layer |
| Clinical Assessment | Allergy Management | Customer-Supplier | Anti-Corruption Layer |
| Clinical Assessment | Hearing Services | Customer-Supplier | Anti-Corruption Layer |
| Surgical Management | Hearing Services | Partnership | Shared Kernel |
| Surgical Management | Voice Disorders | Partnership | Shared Kernel |
| Sleep Disorders | Allergy Management | Partnership | Published Language |
| All Contexts | EHR | Open Host Service | Published Language (FHIR) |
| All Contexts | Billing | Conformist | Published Language (CPT/ICD-10) |

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 14 on context maps.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 3 on context mapping.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 8 on context mapping patterns.
