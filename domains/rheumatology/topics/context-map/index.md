# Context Map in the Rheumatology Domain

## Overview

A context map provides a visual and conceptual representation of the relationships and interactions between bounded contexts. In the rheumatology domain, the context map documents how clinical assessment feeds into disease management, how disease management drives therapy selection, how therapy and pain management operate in parallel, and how outcomes tracking aggregates data from all upstream contexts. Understanding these relationships is essential for designing integration points and managing dependencies.

## Context Relationships

### Clinical Assessment to Autoimmune Disease (Customer-Supplier)

The Clinical Assessment Context is the upstream supplier that produces assessment data including joint counts, serology results, and disease activity scores. The Autoimmune Disease Context is the downstream customer that consumes this data to classify diseases, detect flares, and adjust treatment plans. The Autoimmune Disease Context defines what assessment data it needs, and the Clinical Assessment Context accommodates those requirements. This is a customer-supplier relationship because the assessment context serves multiple consumers and must balance their needs.

### Clinical Assessment to Joint Disease (Customer-Supplier)

Similarly, the Clinical Assessment Context supplies assessment data to the Joint Disease Context. Joint examinations, synovial fluid analysis orders, and imaging results flow downstream. The Joint Disease Context uses this data for osteoarthritis staging, gout diagnosis, and injection therapy planning. The assessment context provides a published language of standardized clinical findings that the joint disease context interprets according to its own rules.

### Autoimmune Disease to Biologic Therapy (Customer-Supplier)

The Autoimmune Disease Context acts as a customer requesting biologic therapy when disease activity remains above target despite conventional DMARDs. It sends therapy requests specifying disease type, prior treatments, and contraindications. The Biologic Therapy Context, as supplier, selects appropriate agents, manages prescribing workflows, and reports therapy status back. This relationship requires careful contract definition because the Biologic Therapy Context must enforce its own safety and regulatory rules independently.

### Joint Disease to Biologic Therapy (Customer-Supplier)

The Joint Disease Context has a similar but less frequent relationship with the Biologic Therapy Context. Some crystal arthropathy cases with refractory disease may require biologic therapy (such as IL-1 inhibitors for gout). The interaction follows the same pattern: the joint disease context requests therapy, and the biologic therapy context manages prescribing.

### Pain Management as Shared Service

The Pain Management Context operates as a shared service consumed by both the Autoimmune Disease Context and the Joint Disease Context. Patients from either context may need multimodal pain management. The Pain Management Context receives referrals containing the patient's diagnosis, functional limitations, and pain assessment. It maintains its own model and returns therapy progress reports.

### Outcomes Tracking as Aggregator

The Outcomes Tracking Context sits downstream of all other contexts, aggregating data to build longitudinal patient records. It has conformist relationships with the Clinical Assessment Context (receiving DAS28 scores), the Autoimmune Disease Context (receiving disease activity trajectories), the Joint Disease Context (receiving radiographic scores), the Biologic Therapy Context (receiving therapy response data), and the Pain Management Context (receiving functional outcome measures).

## Integration Patterns Used

### Published Language

The Clinical Assessment Context publishes assessment results using standardized clinical data formats. DAS28 scores, serology results, and joint counts follow ACR/EULAR definitions, providing a published language that all downstream contexts understand without translation.

### Open Host Service

The Biologic Therapy Context exposes an open host service for therapy requests. This service accepts standardized therapy request messages and returns therapy status updates. Multiple upstream contexts can interact with this service using the same protocol.

### Anti-Corruption Layer

The Outcomes Tracking Context uses anti-corruption layers to translate between different assessment instruments and its own normalized outcome model. When receiving a SLEDAI score from the Autoimmune Disease Context and a DAS28 score from the Clinical Assessment Context, the ACL normalizes these into a comparable disease trajectory format.

### Shared Kernel

The Clinical Assessment Context and the Autoimmune Disease Context share a small kernel of common definitions around joint examination terminology. Both contexts agree on what constitutes a "tender joint" and a "swollen joint" and use identical definitions. This shared kernel is small and carefully governed to prevent coupling.

## External System Integrations

The Biologic Therapy Context integrates with external pharmacy systems and drug registries. An anti-corruption layer translates between the internal therapy model and external prescription formats (e.g., NCPDP SCRIPT standard).

The Outcomes Tracking Context integrates with external quality reporting systems (CMS MIPS, HEDIS measures). An ACL translates internal outcome measures into the required reporting formats.

## Upstream and Downstream Summary

Upstream contexts (producers): Clinical Assessment, Autoimmune Disease, Joint Disease.
Midstream contexts (producers and consumers): Biologic Therapy, Pain Management.
Downstream contexts (consumers): Outcomes Tracking.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 14.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 3.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 8.
- Hohpe, Gregor, and Bobby Woolf. Enterprise Integration Patterns. Addison-Wesley, 2003.
