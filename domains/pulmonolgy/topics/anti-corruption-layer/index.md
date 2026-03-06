# Anti-Corruption Layer in the Pulmonolgy Domain

## Overview

An anti-corruption layer (ACL) is a translation boundary that protects a bounded context from external system concepts, preventing foreign models from corrupting the internal domain model. In the pulmonolgy domain, anti-corruption layers are essential at integration points where external clinical systems, vendor-specific data formats, and legacy hospital information systems interact with the domain's bounded contexts.

The ACL pattern is particularly important in healthcare systems where multiple vendors, standards bodies, and legacy platforms produce data in incompatible formats. Without anti-corruption layers, the pulmonolgy domain model would be forced to accommodate vendor-specific data structures, proprietary terminology, and external system constraints that have nothing to do with the actual clinical domain.

## Anti-Corruption Layers in the Pulmonolgy Domain

### Sleep Medicine Context to External Sleep Lab Systems

The most prominent anti-corruption layer in the pulmonolgy domain sits between the Sleep Medicine Context and external sleep laboratory data systems. Sleep lab equipment vendors produce polysomnography data in proprietary formats, CPAP compliance data in device-specific schemas, and scoring results in varying structures.

The ACL translates vendor-specific data into the Sleep Medicine Context's domain model:

- Vendor-specific sleep stage classifications are mapped to the domain's standardized sleep stage model based on AASM criteria.
- Proprietary CPAP compliance data formats are translated into the domain's therapy adherence value objects.
- Device-specific pressure titration records are converted into the domain's titration result model.
- Vendor event scoring (apneas, hypopneas, arousals) is normalized to the domain's event classification scheme.

This ACL absorbs all vendor variability, ensuring that the Sleep Medicine Context's internal model remains clean, consistent, and vendor-independent.

### Critical Care Context to Pulmonary Rehabilitation Context

When patients transition from ICU care to pulmonary rehabilitation, an anti-corruption layer translates between the Critical Care Context's acute care model and the Pulmonary Rehabilitation Context's recovery-focused model.

The ACL performs the following translations:

- Ventilator mode history and weaning trajectory are translated into functional status assessments relevant to rehabilitation planning.
- ICU sedation scores and delirium assessments are converted into cognitive and physical readiness indicators.
- Critical care physiological parameters (PaO2/FiO2 ratios, compliance measurements) are translated into exercise tolerance estimates.
- ICU length of stay and complication history are mapped to rehabilitation risk stratification categories.

### Pulmonary Assessment Context to External Imaging Systems (PACS)

Chest imaging interpretation within the Pulmonary Assessment Context requires integration with radiology Picture Archiving and Communication Systems (PACS). The ACL translates between DICOM metadata, radiology report formats, and the assessment context's internal imaging interpretation model.

The ACL handles:

- DICOM header fields mapped to the domain's imaging study value objects.
- Radiology report structured data (if available) translated into the domain's finding classification model.
- Free-text radiology impressions preserved as reference data without forcing the assessment model to adopt radiology-specific terminology.

### Chronic Disease Management Context to External Pharmacy Systems

Treatment plan management in the Chronic Disease Management Context requires interaction with external pharmacy and medication management systems. The ACL translates between pharmacy formulary structures, medication coding systems (NDC, RxNorm), and the domain's treatment plan model.

The ACL ensures:

- External medication identifiers are mapped to the domain's medication value objects.
- Formulary availability data is translated into the domain's treatment option model without importing pharmacy-specific business rules.
- Prescription fulfillment events from external systems are converted into the domain's medication adherence tracking model.

## ACL Design Principles

Anti-corruption layers in the pulmonolgy domain follow several design principles:

- **Isolation**: The ACL is the only component that understands both the external model and the internal domain model. No other part of the bounded context references external data structures.

- **Translation Completeness**: Every external concept that enters the bounded context passes through the ACL. There are no backdoor integrations that bypass translation.

- **Domain Model Priority**: When conflicts arise between external representations and internal domain concepts, the internal domain model takes precedence. The ACL adapts external data to fit the domain, not the reverse.

- **Bidirectional Translation**: ACLs handle both inbound translation (external to internal) and outbound translation (internal to external) when the bounded context must send data to external systems.

- **Error Handling**: The ACL includes explicit handling for untranslatable data, unmapped codes, and format inconsistencies, raising domain-appropriate errors rather than propagating external system exceptions.

## When to Introduce an ACL

Not every integration point requires an anti-corruption layer. ACLs are justified when:

- The external system uses a fundamentally different model that would distort the internal domain if adopted directly.
- The external system is controlled by a different organization or vendor and may change without notice.
- Multiple external systems provide similar data in incompatible formats (such as multiple CPAP device vendors).
- The external system's model reflects concerns (billing, device management, vendor lock-in) that are irrelevant to the clinical domain.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 14 on anti-corruption layers as a strategy for maintaining model integrity.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 3 on implementing anti-corruption layers between bounded contexts.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 9 on anti-corruption layers and translation patterns.
