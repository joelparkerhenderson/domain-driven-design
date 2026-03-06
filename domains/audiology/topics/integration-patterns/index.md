# Integration Patterns in the Audiology Domain

## Overview

Integration patterns define how bounded contexts in the audiology domain connect, share information, and coordinate activities. Domain-Driven Design identifies several integration patterns that characterize the relationships between contexts, each with different implications for coupling, autonomy, and complexity. The audiology domain employs multiple integration patterns based on the nature of each inter-context relationship.

## Published Language

Published language is the most prevalent integration pattern in the audiology domain. A published language defines a well-documented, shared format for exchanging information between bounded contexts. Both the producing and consuming contexts agree on the data format, but each translates to and from this shared format internally.

### Audiometric Data Format

The primary published language in the audiology domain is the standardized audiometric data format used to communicate hearing assessment results to downstream contexts. This format specifies how audiogram data (thresholds at each frequency for each ear and conduction type), hearing loss classification, speech recognition scores, and clinical recommendations are represented. The Device Management Context translates this published format into its own model of the patient's hearing profile. The Rehabilitation Context translates it into its model of communication capability. The Clinical Documentation Context formats it for clinical reporting.

### Clinical Event Format

A second published language defines how clinical events are structured for cross-context consumption. This format specifies standard event attributes (identifier, type, timestamp, correlation ID) and payload conventions that all contexts follow when publishing and consuming domain events.

## Customer-Supplier

The customer-supplier pattern characterizes relationships where one context (the supplier) provides data that another context (the customer) depends on. The supplier accommodates the customer's needs within its own delivery schedule.

### Assessment to Device Management

The Hearing Assessment Context (supplier) provides diagnostic data to the Device Management Context (customer). The Device Management team communicates its data needs (what audiometric data is required for fitting, in what format, at what level of detail), and the Hearing Assessment team ensures its published language meets these requirements. The supplier does not tailor its internal model to the customer but ensures its published output satisfies downstream needs.

### Pediatric Audiology to Rehabilitation

The Pediatric Audiology Context (supplier) provides developmental and family context to the Rehabilitation Context (customer). The Rehabilitation team requires information about the child's developmental status, family engagement level, and age-appropriate communication expectations to design effective intervention programs.

## Shared Kernel

The shared kernel pattern applies when two bounded contexts share a subset of their domain model. Changes to the shared subset require coordination between both contexts.

### Hearing Assessment and Pediatric Audiology

These two contexts share a kernel of core audiometric concepts: audiogram representation, hearing threshold value objects, hearing loss classification, and pure tone average calculation. Pediatric Audiology extends these shared concepts with pediatric-specific additions (age-appropriate testing methods, developmental context), but the core audiometric model is shared. Changes to how thresholds are represented or how hearing loss is classified must be coordinated between both contexts because both depend on identical definitions.

The shared kernel is kept as small as possible to minimize coordination overhead. Only the fundamental audiometric data structures are shared; the testing workflows, business rules, and domain services remain context-specific.

## Conformist

The conformist pattern applies when a downstream context adopts the upstream context's model without modification, choosing not to translate between models.

### Vestibular Services to Rehabilitation

When vestibular rehabilitation programs are tracked alongside general aural rehabilitation, the Vestibular Services Context conforms to the Rehabilitation Context's model for exercise tracking and outcome measurement. Rather than creating its own model of rehabilitation activities, the Vestibular Services Context adopts the Rehabilitation Context's intervention and outcome structures for the rehabilitation components of vestibular care.

## Anti-Corruption Layer

The anti-corruption layer pattern protects a bounded context from external system concepts by providing a translation boundary. This pattern is used extensively in the audiology domain for external system integration.

### Manufacturer Integration

The Device Management Context uses anti-corruption layers to interface with hearing device manufacturer systems. Each manufacturer's proprietary data model is translated through a dedicated ACL adapter, preventing manufacturer-specific concepts from contaminating the domain model.

### EHR Integration

The Clinical Documentation Context uses an anti-corruption layer to interface with electronic health record systems. EHR-specific data models, workflows, and interchange formats (HL7 FHIR, HL7 v2, CDA) are translated at the boundary, keeping the documentation context's internal model focused on audiology-specific concerns.

## Open Host Service

The open host service pattern provides a well-defined protocol for accessing a bounded context's capabilities. Any consumer that understands the protocol can integrate without bilateral coordination.

### Clinical Documentation as Open Host

The Clinical Documentation Context exposes an open host service for report retrieval and referral status queries. Any context (or external system) that understands the documentation service's protocol can retrieve reports, check referral status, and subscribe to documentation events without requiring custom integration work.

## Pattern Selection Criteria

The choice of integration pattern for each relationship in the audiology domain follows several criteria. The strategic importance of the relationship determines whether tight coordination (shared kernel) or loose coupling (published language) is appropriate. The frequency of change in each context influences whether conformist (low change) or ACL (high change) patterns are suitable. The number of consumers determines whether published language (many consumers) or customer-supplier (few consumers) is efficient.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley. Chapter 14 on integration patterns.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley. Chapter 3 on context mapping and integration.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media. Chapter 5 on integration patterns.
- Hohpe, G., & Woolf, B. (2003). Enterprise Integration Patterns. Addison-Wesley.
