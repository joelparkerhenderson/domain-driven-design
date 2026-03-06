# Context Map in the Audiology Domain

## Overview

The context map provides a visual and conceptual representation of the relationships and interactions between the six bounded contexts in the audiology domain. It documents how information flows between contexts, which contexts depend on others, and what integration patterns govern their communication. The context map is the primary strategic tool for understanding the audiology system as a whole.

## Context Relationships

### Hearing Assessment to Device Management (Customer-Supplier)

The Hearing Assessment Context acts as the upstream supplier, producing diagnostic data that the Device Management Context consumes. Device Management is the downstream customer that depends on accurate audiometric results to select and program devices. The supplier publishes assessment results in a standardized format (published language) that Device Management translates into its own model. This relationship is the most critical in the audiology domain because device fitting accuracy depends entirely on diagnostic accuracy.

### Hearing Assessment to Rehabilitation (Customer-Supplier)

The Hearing Assessment Context supplies diagnostic findings to the Rehabilitation Context, which uses them to design appropriate rehabilitation programs. The degree and type of hearing loss, speech recognition abilities, and communication needs all flow from assessment to rehabilitation. The Rehabilitation Context adapts these inputs into its own model of patient capabilities and intervention targets.

### Hearing Assessment to Pediatric Audiology (Shared Kernel)

The Hearing Assessment and Pediatric Audiology contexts share a kernel of core audiometric concepts. Both contexts use audiograms, hearing thresholds, and hearing loss classifications. However, Pediatric Audiology extends these shared concepts with age-specific testing methods and developmental monitoring. The shared kernel is tightly controlled to ensure changes in core audiometric concepts propagate correctly to both contexts.

### Hearing Assessment to Clinical Documentation (Published Language)

The Hearing Assessment Context publishes test results in a standardized format that the Clinical Documentation Context consumes for report generation. The published language includes audiogram data, test parameters, and clinical impressions. Clinical Documentation does not interpret these results; it formats and distributes them according to documentation standards.

### Device Management to Clinical Documentation (Published Language)

The Device Management Context publishes fitting records, verification measurements, and device specifications to Clinical Documentation. The published language ensures that device-related documentation meets manufacturer and regulatory requirements. This relationship enables the Clinical Documentation Context to generate comprehensive fitting reports without understanding device programming details.

### Rehabilitation to Clinical Documentation (Published Language)

The Rehabilitation Context publishes treatment plans, progress notes, and outcome measures to Clinical Documentation. The published language standardizes how rehabilitation activities and outcomes are recorded, enabling longitudinal tracking and reporting.

### Vestibular Services to Clinical Documentation (Published Language)

The Vestibular Services Context publishes evaluation findings and rehabilitation progress to Clinical Documentation. Balance assessment results and VRT outcomes flow through a published language that the documentation context formats into vestibular-specific reports.

### Vestibular Services to Rehabilitation (Conformist)

When vestibular rehabilitation overlaps with general rehabilitation services, the Vestibular Services Context conforms to the Rehabilitation Context's model for tracking exercise programs and outcomes. The Vestibular Services Context adopts the Rehabilitation Context's vocabulary for intervention planning while maintaining its own model for vestibular-specific assessment.

### Pediatric Audiology to Device Management (Customer-Supplier)

The Pediatric Audiology Context acts as a customer of the Device Management Context for pediatric device fitting. Pediatric-specific requirements (smaller ear canals, growth considerations, tamper resistance) are communicated as constraints that the Device Management Context must accommodate.

### Pediatric Audiology to Rehabilitation (Customer-Supplier)

The Pediatric Audiology Context supplies developmental and family context information to the Rehabilitation Context, which uses it to design age-appropriate intervention programs. Early intervention goals and family engagement plans flow from Pediatric Audiology to Rehabilitation.

### External System Integration (Anti-Corruption Layers)

All bounded contexts that interface with external systems use anti-corruption layers. Device Management maintains an ACL for hearing aid manufacturer programming interfaces. Clinical Documentation maintains an ACL for electronic health record (EHR) systems. The Hearing Assessment Context maintains an ACL for audiometer calibration and data transfer systems.

## Upstream and Downstream Patterns

The Hearing Assessment Context is the primary upstream context in the audiology domain. Most other contexts depend on its diagnostic output. The Clinical Documentation Context is the primary downstream context, consuming information from all other contexts. Device Management and Rehabilitation occupy middle positions, both consuming assessment data and producing documentation data.

## Map Governance

The context map is a living document that evolves as the audiology domain model matures. Changes to context relationships require agreement from the teams responsible for both contexts involved. The shared kernel between Hearing Assessment and Pediatric Audiology receives the most careful governance because changes affect two core clinical workflows.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley. Chapter 14 on context mapping.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley. Chapter 3 on context maps.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media. Chapter 5 on context mapping patterns.
- Brandolini, A. (2009). Strategic Domain Driven Design with Context Mapping. InfoQ.
