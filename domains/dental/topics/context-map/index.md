# Context Map - Dental Domain

## Overview

The context map provides a visual and descriptive representation of how the six bounded contexts in the dental domain relate to one another. It identifies the nature of each relationship, the direction of data flow, and the integration pattern used at each boundary. Understanding these relationships is essential for managing dependencies and ensuring that changes in one context do not create unintended consequences in another.

## Context Relationships

### Clinical Examination Context -> Treatment Planning Context

Relationship type: Customer-Supplier. The Clinical Examination Context is the upstream supplier of diagnostic findings. The Treatment Planning Context is the downstream customer that consumes examination results, radiographic interpretations, and charting data to create treatment plans.

Integration pattern: Published Language. Diagnostic findings are communicated as structured domain events using a shared schema that both contexts agree upon. The Treatment Planning Context does not modify or extend examination data.

### Treatment Planning Context -> Restorative Services Context

Relationship type: Customer-Supplier. The Treatment Planning Context is the upstream supplier of approved treatment items. The Restorative Services Context is the downstream customer that receives authorized restorative procedures for execution.

Integration pattern: Conformist. The Restorative Services Context conforms to the treatment item definitions provided by the Treatment Planning Context, accepting the procedure codes, tooth numbers, and surface designations as specified.

### Treatment Planning Context -> Orthodontic Management Context

Relationship type: Customer-Supplier. The Treatment Planning Context supplies approved orthodontic treatment plans. The Orthodontic Management Context receives these plans and manages their extended execution timeline independently.

Integration pattern: Anti-Corruption Layer. The Orthodontic Management Context translates treatment plan items into its own model of orthodontic cases, which operate on a different timescale and require different tracking structures than general treatment items.

### Treatment Planning Context -> Periodontal Care Context

Relationship type: Customer-Supplier. The Treatment Planning Context supplies approved periodontal treatment sequences. The Periodontal Care Context receives these and manages the ongoing disease management cycle.

Integration pattern: Anti-Corruption Layer. The Periodontal Care Context translates treatment items into its longitudinal disease management model, adapting single-procedure items into recurring maintenance schedules.

### Clinical Examination Context -> Periodontal Care Context

Relationship type: Shared Kernel. Both contexts share a common model for periodontal probing measurements. The Clinical Examination Context records initial probing depths during examination, and the Periodontal Care Context uses the same measurement model for ongoing monitoring.

Integration pattern: Shared Kernel. A small, explicitly defined shared model for probing depth measurements (six sites per tooth, measured in millimeters) is co-owned by both contexts.

### Practice Management Context -> All Clinical Contexts

Relationship type: Open Host Service. The Practice Management Context provides scheduling, insurance, and billing services to all clinical contexts through a well-defined service interface.

Integration pattern: Open Host Service with Published Language. The Practice Management Context exposes appointment scheduling, insurance verification, and claims submission capabilities through a stable interface that clinical contexts consume without needing to understand internal administrative models.

### Restorative Services Context -> Clinical Examination Context

Relationship type: Customer-Supplier. The Restorative Services Context reports completed procedure outcomes back to the Clinical Examination Context so the dental chart can be updated with new restorations.

Integration pattern: Domain Events. Procedure completion events carry the information needed for the Clinical Examination Context to update the patient's dental chart.

### Orthodontic Management Context -> Clinical Examination Context

Relationship type: Customer-Supplier. The Orthodontic Management Context reports tooth movement outcomes and appliance status changes back to the Clinical Examination Context for chart updates.

Integration pattern: Domain Events. Progress tracking events inform the clinical record of current orthodontic status.

### Periodontal Care Context -> Clinical Examination Context

Relationship type: Customer-Supplier. The Periodontal Care Context reports periodontal treatment outcomes and updated measurements back to the Clinical Examination Context.

Integration pattern: Domain Events. Treatment completion and re-evaluation events update the clinical record with current periodontal status.

## External System Integrations

### Dental Insurance Clearinghouses

The Practice Management Context integrates with external insurance clearinghouses through an anti-corruption layer that translates between internal claim models and ANSI X12 837D transaction formats.

### Electronic Health Record Systems

The Clinical Examination Context integrates with broader EHR systems through an anti-corruption layer that maps dental-specific charting data to HL7 FHIR dental profiles.

### Dental Supply Systems

The Restorative Services Context integrates with dental supply ordering systems through an anti-corruption layer that translates material inventory needs into supplier-specific catalog formats.

## Context Map Summary Table

| Upstream Context | Downstream Context | Pattern |
|---|---|---|
| Clinical Examination | Treatment Planning | Published Language |
| Treatment Planning | Restorative Services | Conformist |
| Treatment Planning | Orthodontic Management | Anti-Corruption Layer |
| Treatment Planning | Periodontal Care | Anti-Corruption Layer |
| Clinical Examination | Periodontal Care | Shared Kernel |
| Practice Management | All Clinical Contexts | Open Host Service |
| Restorative Services | Clinical Examination | Domain Events |
| Orthodontic Management | Clinical Examination | Domain Events |
| Periodontal Care | Clinical Examination | Domain Events |

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 14 on context mapping.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 3 on context maps.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 8 on context mapping patterns.
