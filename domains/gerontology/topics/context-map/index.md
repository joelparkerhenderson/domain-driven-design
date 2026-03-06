# Context Map in the Gerontology Domain

## Overview

A context map is a visual and conceptual representation of the relationships and interactions between bounded contexts in a domain. In the gerontology domain, the context map documents how the six bounded contexts (Geriatric Assessment, Care Coordination, Cognitive Health, Functional Independence, Medication Management, and End of Life Planning) relate to each other and to external systems. The context map reveals upstream-downstream relationships, shared kernels, and integration strategies.

Evans (2003) describes the context map as one of the most important artifacts in strategic design. It makes explicit the political, organizational, and technical realities that shape how models interact. In geriatric care, these realities include fragmented healthcare delivery, regulatory constraints, and the interdisciplinary nature of clinical teams.

## Upstream and Downstream Relationships

The Geriatric Assessment Context serves as the primary upstream context. It produces comprehensive assessment results that inform decisions in nearly every other context. When a CGA is completed, its findings flow downstream to Care Coordination (for care plan development), Cognitive Health (for cognitive screening triggers), Functional Independence (for ADL/IADL baseline establishment), and Medication Management (for polypharmacy review initiation).

The Care Coordination Context acts as both a downstream consumer and an upstream producer. It consumes assessment results and produces coordinated care plans that the End of Life Planning Context and Functional Independence Context rely upon for alignment with overall patient goals.

The Medication Management Context consumes assessment data and cognitive health data (to account for adherence capacity) and publishes medication change events consumed by Care Coordination for care plan updates.

The End of Life Planning Context is primarily a downstream consumer, receiving care plan information and health status updates, while publishing advance directive and goals-of-care decisions that shape upstream care delivery in all other contexts.

## Relationship Types

Customer-Supplier: The Geriatric Assessment Context is the supplier and the Cognitive Health Context is the customer. The assessment context provides screening results in a format that the cognitive health context requires, and the cognitive health team can request changes to the assessment output format.

Conformist: The Medication Management Context conforms to external pharmacy formulary systems. The external system dictates the medication classification model, and the gerontology domain adapts its model accordingly rather than negotiating changes.

Anti-Corruption Layer: The Care Coordination Context uses an anti-corruption layer when integrating with hospital electronic health record (EHR) systems. The EHR model of a "patient encounter" differs from the gerontology domain's model of a "care episode," requiring translation at the boundary.

Published Language: The Cognitive Health Context publishes cognitive test results using standardized scoring formats (MMSE 0-30, MoCA 0-30) that any downstream context can interpret without bilateral negotiation.

Shared Kernel: The Geriatric Assessment Context and Functional Independence Context share a kernel containing the PatientDemographics value object and the FunctionalStatusScore value object, maintained jointly by both teams.

## External System Integrations

Hospital EHR Systems: The gerontology domain integrates with hospital EHRs through anti-corruption layers in the Care Coordination and Geriatric Assessment contexts. HL7 FHIR resources serve as the published language for data exchange.

Pharmacy Benefit Managers: The Medication Management Context integrates with pharmacy benefit managers using a conformist relationship, adapting to external formulary and prior authorization models.

Insurance and Payer Systems: The Care Coordination Context integrates with insurance systems for authorization of services such as home health, skilled nursing, and hospice care. An anti-corruption layer translates between payer terminology and the domain's ubiquitous language.

Community Service Directories: The Care Coordination Context integrates with community resource databases, using an open host service pattern to query available services by location, type, and eligibility criteria.

## Event Flows

Domain events form the primary communication mechanism between bounded contexts. Key event flows include AssessmentCompleted flowing from Geriatric Assessment to all downstream contexts, CognitiveDeclineDetected flowing from Cognitive Health to Care Coordination and End of Life Planning, MedicationRegimenChanged flowing from Medication Management to Care Coordination, and AdvanceDirectiveRecorded flowing from End of Life Planning to all contexts for care plan alignment.

## Maintaining the Context Map

The context map is a living artifact that evolves as the domain understanding deepens and organizational structures change. Regular review sessions involving representatives from each bounded context team ensure that the map accurately reflects current relationships and integration points. Changes to the context map trigger reassessment of anti-corruption layers and integration contracts.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media.
- HL7 International. (2019). FHIR (Fast Healthcare Interoperability Resources) Specification.
