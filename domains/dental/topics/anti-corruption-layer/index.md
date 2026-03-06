# Anti-Corruption Layer - Dental Domain

## Overview

An anti-corruption layer (ACL) is a translation boundary that protects a bounded context from the models, terminology, and assumptions of external systems or other bounded contexts. In the dental domain, anti-corruption layers are essential at boundaries where dental-specific clinical models interface with insurance systems, electronic health record platforms, dental supply chains, and between bounded contexts that model the same concepts differently.

## Purpose in the Dental Domain

Dental practice systems must integrate with numerous external systems that use different data models, terminology, and assumptions about dental care. Without anti-corruption layers, these external models can distort the internal domain model, leading to a system that reflects insurance company logic rather than clinical reality, or supply chain catalog structure rather than material selection best practices.

The ACL translates between the clean internal domain model and the messy reality of external system interfaces. It ensures that the bounded context's ubiquitous language remains consistent even when communicating with systems that use different terms for the same concepts.

## ACL Between Bounded Contexts

### Treatment Planning to Orthodontic Management ACL

The Treatment Planning Context models treatment items as discrete procedures with CDT codes and estimated costs. The Orthodontic Management Context models orthodontic treatment as a continuous case with phases, milestones, and incremental progress tracking. The ACL translates individual treatment plan items (such as "comprehensive orthodontic treatment, adolescent") into the Orthodontic Management Context's case model with assessment phase, active treatment phase, and retention phase.

### Treatment Planning to Periodontal Care ACL

The Treatment Planning Context models periodontal treatment as a sequence of procedure items. The Periodontal Care Context models treatment as an ongoing disease management program. The ACL translates treatment plan items into the periodontal disease management model, mapping procedures like scaling and root planing into the longitudinal tracking structure that monitors pocket depths and clinical attachment levels over time.

## ACL for External System Integration

### Insurance Clearinghouse ACL

The Practice Management Context maintains an internal claim model using domain-specific terminology. Insurance clearinghouses require ANSI X12 837D transaction format with specific segment and element structures. The ACL translates between these models:

- Internal CDT procedure codes map to the X12 SV3 segment dental service elements.
- Internal tooth numbering (universal or ISO) maps to the X12 TOO segment tooth information.
- Internal diagnosis codes map to the X12 HI segment health information codes.
- Claim status responses in X12 277 format are translated back into domain-specific claim status events.

### Electronic Health Record ACL

The Clinical Examination Context maintains dental-specific charting and examination data. Integration with broader EHR systems requires translation to HL7 FHIR formats. The ACL handles:

- Dental chart entries map to FHIR Condition and Observation resources with dental-specific profiles.
- Radiographic reports map to FHIR DiagnosticReport resources with dental imaging extensions.
- Treatment plans map to FHIR CarePlan resources with dental procedure activity definitions.

### Dental Supply System ACL

The Restorative Services Context tracks materials by clinical properties (material type, shade, strength characteristics). Dental supply ordering systems use manufacturer-specific catalog numbers and packaging units. The ACL translates:

- Clinical material specifications map to supplier catalog item identifiers.
- Inventory consumption at the procedure level maps to supply reorder quantities at the packaging level.
- Material lot tracking for clinical records maps to supplier batch and expiration data.

## ACL Design Principles

1. The ACL is owned by the downstream context that needs protection from the external model.
2. Translation logic resides entirely within the ACL; neither the internal model nor the external system is modified.
3. The ACL exposes only the internal model's interfaces to the bounded context, hiding all external model details.
4. Changes to external system formats require ACL updates only, not changes to the domain model.
5. The ACL validates incoming data against domain invariants before allowing it to enter the bounded context.

## Implementation Considerations

Anti-corruption layers in the dental domain must handle the complexity of dental coding systems, which include CDT codes that change annually, tooth numbering systems that vary by country, and surface notation conventions that differ between clinical documentation and insurance claim formats. The ACL must be updated when these external standards change, without requiring modifications to the internal domain model.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 14 on anti-corruption layers.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 3 on context mapping and ACL patterns.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 9 on integrating bounded contexts.
