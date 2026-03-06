# Anti-Corruption Layer in the Sleep Quality Domain

## Overview

An anti-corruption layer (ACL) is a translation boundary that protects a bounded context from the concepts, terminology, and data models of external systems. In the sleep quality domain, anti-corruption layers are essential at boundaries where the domain model interfaces with device manufacturer APIs, electronic health record systems, insurance platforms, and third-party clinical databases. Without ACLs, the clean domain model would degrade as it absorbs the idiosyncrasies of external systems.

## Device Integration ACL

The most prominent anti-corruption layer in the sleep quality domain sits at the boundary of the Device Integration Context. Each device manufacturer (Fitbit, Apple Health, ResMed, Philips Respironics, Withings, Oura) uses its own proprietary data model with different field names, units, data granularity, and quality indicators. The ACL translates these proprietary representations into the canonical domain model.

For example, one wearable may report "deep sleep minutes" while another reports "slow wave sleep percentage." The ACL maps both representations to the domain concept of N3 sleep stage duration, applying appropriate unit conversions and confidence indicators. The consuming bounded contexts never see the device-specific representations; they work only with domain-standard value objects.

## EHR Integration ACL

The Sleep Disorders Context maintains an anti-corruption layer at its boundary with external electronic health record (EHR) systems. EHR systems use ICD-10-CM diagnostic codes (e.g., G47.00 for insomnia, G47.33 for obstructive sleep apnea) and HL7/FHIR data models. The ACL translates between ICD-10 codes and the domain's ICSD-3-based disorder classification model, which has finer-grained diagnostic criteria and different organizational principles.

The ACL also handles differences in patient identification, encounter models, and clinical note structures between EHR systems and the domain model. This translation prevents EHR implementation details from leaking into the clinical domain logic.

## Insurance and Billing ACL

When the domain interfaces with insurance and billing systems, an ACL translates between clinical domain concepts and billing constructs. Sleep studies may need to be represented as CPT codes (e.g., 95810 for polysomnography, 95811 for polysomnography with CPAP titration). The ACL ensures that billing classifications do not influence how the domain models clinical assessments internally.

## Research Database ACL

Integration with clinical research databases (ClinicalTrials.gov, sleep research repositories) requires translation between the domain's intervention and outcomes models and research-specific data formats (CDISC, REDCap). The ACL maps domain events and outcome records to research data structures without requiring the domain model to accommodate research-specific metadata.

## Design Principles

Anti-corruption layers in the sleep quality domain follow several design principles. The ACL is owned by the consuming bounded context, not the external system. Translation logic is isolated in dedicated adapter components that can be updated independently when external APIs change. The ACL validates incoming data against domain invariants, rejecting or flagging data that does not meet quality thresholds. The ACL handles error cases (missing data, invalid values, unsupported formats) without propagating failures into the domain.

## Bidirectional Translation

Some ACLs must handle bidirectional translation. When the domain publishes outcomes data to external research databases, the ACL translates domain concepts outward. When the domain receives updated diagnostic criteria from external clinical standards bodies, the ACL translates inward. Bidirectional ACLs maintain clear separation between inbound translation (external to domain) and outbound translation (domain to external).

## Data Quality and Confidence

A distinguishing feature of ACLs in the sleep quality domain is the handling of data confidence levels. Device data varies significantly in accuracy and reliability. Consumer wearables provide estimates of sleep stages with lower confidence than clinical polysomnography. The ACL annotates translated data with confidence metadata, allowing downstream contexts to weight data appropriately in their models.

## Versioning and Evolution

External APIs change frequently, particularly device manufacturer APIs. The ACL design accommodates API versioning by maintaining version-specific translators that can be activated or deprecated as external systems evolve. This versioning strategy prevents external API changes from requiring changes to the domain model.

## Testing Strategy

ACLs are tested with comprehensive fixture data covering the range of values, edge cases, and error conditions produced by each external system. Contract tests verify that the ACL produces valid domain objects from representative external data. Integration tests verify end-to-end data flow through the ACL.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley. Chapter 14 on anti-corruption layers.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley. Chapter 13 on integrating bounded contexts.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media. Chapter 9 on anti-corruption layers.
