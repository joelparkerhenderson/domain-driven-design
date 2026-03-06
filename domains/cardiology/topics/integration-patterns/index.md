# Integration Patterns in Cardiology

## Overview

Integration patterns define how bounded contexts in the cardiology domain communicate, share data, and maintain consistency across boundaries. Domain-Driven Design identifies several integration patterns, each appropriate for different relationship types between contexts. In cardiology, these patterns govern how clinical data flows from patient assessment through diagnosis, treatment, and rehabilitation.

## Pattern Catalog

### Shared Kernel

A shared kernel is a small, explicitly defined portion of the domain model that two bounded contexts share. Changes to the shared kernel require agreement from both teams.

**Application in Cardiology**: The Clinical Assessment Context and Heart Failure Management Context share a kernel containing:

- Patient vital signs model (blood pressure, heart rate, weight, oxygen saturation).
- Cardiac biomarker result model (troponin, BNP/NT-proBNP with value, units, and reference ranges).
- NYHA functional classification model.
- Basic patient demographic reference.

This shared kernel exists because both contexts need identical representations of these fundamental clinical concepts, and any divergence would create clinical data integrity risks. The shared kernel is kept deliberately small to minimize coupling. Changes require review and agreement from clinicians and developers working in both contexts.

### Published Language

A published language is a well-documented, shared data format that a bounded context uses to communicate with any number of consumers. The publishing context defines the language; consumers adapt to it.

**Application in Cardiology**: The Diagnostic Imaging Context publishes imaging results using a standardized published language based on professional society measurement guidelines:

- Echocardiographic measurements follow ASE (American Society of Echocardiography) standards for chamber quantification, valve assessment, and diastolic function grading.
- Cardiac CT reports follow SCCT (Society of Cardiovascular Computed Tomography) structured reporting standards.
- Cardiac MRI reports follow SCMR (Society for Cardiovascular Magnetic Resonance) standardized protocols.

Any consuming context (Clinical Assessment, Heart Failure Management, Interventional Procedures) can interpret imaging results because they are published in this standardized format. The imaging context is not modified to suit individual consumers.

### Customer-Supplier

In a customer-supplier relationship, the downstream (customer) context depends on the upstream (supplier) context. The supplier provides what the customer needs, but the customer has some influence over the supplier's development priorities.

**Application in Cardiology**:

- **Clinical Assessment (supplier) to Diagnostic Imaging (customer)**: The imaging context needs clinical referral data structured in a specific way (indication, relevant history, urgency). The imaging team can request that referral events include specific data elements.
- **Heart Failure Management (supplier) to Cardiac Rehabilitation (customer)**: Rehabilitation needs GDMT status, exercise clearance, and functional classification. The rehabilitation team can request specific data elements in the heart failure context's published events.
- **Diagnostic Imaging (supplier) to Heart Failure Management (customer)**: Heart failure management needs serial ejection fraction data in a format supporting trend analysis.

### Conformist

In a conformist relationship, the downstream context conforms to the upstream context's model without the ability to influence it. This occurs when the upstream context is maintained by an external organization or team that does not accommodate downstream needs.

**Application in Cardiology**:

- **External EHR System to All Cardiology Contexts**: The EHR publishes patient data in its own format. Cardiology contexts must conform to the EHR's data model at the integration boundary, using anti-corruption layers to translate internally.
- **External Laboratory System to Clinical Assessment**: Lab results arrive in HL7/FHIR format defined by the laboratory system. The clinical assessment context conforms to this format at the boundary.
- **National Registries to Interventional Procedures**: The ACC NCDR CathPCI registry defines specific data elements and formats. The interventional context conforms to these requirements for registry submission.

### Open Host Service

An open host service provides a well-defined protocol or API that any number of consumers can use to access a bounded context's capabilities.

**Application in Cardiology**:

- **Billing and Coding Service**: A generic subdomain providing CPT code lookup, ICD-10 diagnosis code mapping, and claim submission services. All cardiology contexts access this service through a uniform interface when they need billing-related operations.
- **Patient Identity Service**: A generic service providing patient identity resolution (MPI - Master Patient Index) used by all contexts to ensure consistent patient identification.
- **Notification Service**: A generic service for sending clinical alerts, appointment reminders, and result notifications. All contexts use the same notification interface.

### Partnership

In a partnership, two bounded contexts coordinate their development because they have mutual dependencies. Both teams must align their release schedules and changes.

**Application in Cardiology**:

- **Interventional Procedures and Electrophysiology**: These contexts have a partnership relationship for hybrid procedures (e.g., combined structural heart intervention and ablation, left atrial appendage closure with pulmonary vein isolation). Both contexts must coordinate procedure modeling, shared operating room workflows, and combined outcomes reporting.

### Anticorruption Layer

Described in detail in the anti-corruption-layer topic. Used whenever a bounded context integrates with an external system whose model differs significantly from the cardiology domain model.

### Separate Ways

Two contexts have no integration and operate independently.

**Application in Cardiology**: The Cardiac Rehabilitation Context and the Electrophysiology Context have minimal direct integration. Rehabilitation programs for post-ablation patients receive relevant data through the Clinical Assessment Context rather than directly from electrophysiology.

## Integration Strategy Selection Guide

| Relationship Type | When to Use | Cardiology Example |
|---|---|---|
| Shared Kernel | Both contexts need identical model for small subset | Clinical Assessment + Heart Failure: vital signs |
| Published Language | One context publishes to many consumers | Imaging publishes standardized measurements |
| Customer-Supplier | Downstream can influence upstream priorities | HF Management requests data from Imaging |
| Conformist | Upstream is external and unchangeable | Lab system format conformance |
| Open Host Service | Generic capability used by many contexts | Billing code lookup service |
| Partnership | Mutual dependencies require coordination | Interventional + Electrophysiology hybrids |
| Separate Ways | No meaningful integration needed | Rehabilitation and Electrophysiology |

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 14 on maintaining model integrity.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 13 on integrating bounded contexts.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapters 8-9 on integration patterns.
- HL7 International. *FHIR R4 Specification* for healthcare data exchange standards.
