# Context Map in the Dermatology Domain

A context map provides a visual and descriptive representation of the relationships and interactions between bounded contexts. In the dermatology domain, the context map reveals how clinical assessment, lesion management, procedure execution, cosmetic services, pathology coordination, and outcome tracking relate to one another. As Evans (2003) emphasizes, the context map is a strategic tool that makes inter-context relationships explicit, enabling teams to manage dependencies and plan integration strategies.

## Context Relationships

### Clinical Assessment to Lesion Management (Upstream-Downstream)

The Clinical Assessment Context is upstream, producing lesion findings during skin examinations. The Lesion Management Context is downstream, consuming these findings to initiate lesion tracking. This is a customer-supplier relationship where the Lesion Management Context depends on the quality and completeness of assessment data. When a dermatologist identifies a suspicious lesion during examination, the Clinical Assessment Context publishes a LesionIdentified event that the Lesion Management Context consumes to create a new Lesion Record.

### Lesion Management to Procedure Management (Upstream-Downstream)

The Lesion Management Context is upstream, determining when a lesion requires procedural intervention and specifying the type of procedure needed. The Procedure Management Context is downstream, receiving procedure requests and executing them according to its own protocols. A ProcedureRequested event from Lesion Management triggers the creation of a Procedure Session in the Procedure Management Context.

### Lesion Management to Pathology Integration (Upstream-Downstream)

When a biopsy is performed, the Lesion Management Context is upstream, initiating specimen collection and providing clinical context. The Pathology Integration Context is downstream, managing specimen accessioning and laboratory coordination. A BiopsyPerformed event triggers specimen tracking. The relationship reverses for diagnostic results: the Pathology Integration Context publishes a PathologyReportCompleted event that the Lesion Management Context consumes to update the lesion diagnosis.

### Procedure Management to Treatment Outcomes (Upstream-Downstream)

The Procedure Management Context is upstream, producing procedure completion data including parameters used and immediate results. The Treatment Outcomes Context is downstream, consuming this data to initiate outcome tracking. A ProcedureCompleted event triggers the creation of a Treatment Outcome Record that will be populated with follow-up assessments over time.

### Clinical Assessment to Treatment Outcomes (Shared Kernel)

These contexts share a small kernel of common concepts related to clinical scoring systems. Both contexts use standardized dermatology scoring (PASI, SCORAD, DLQI) -- the Clinical Assessment Context to establish baseline severity, and the Treatment Outcomes Context to measure change over time. The shared kernel includes the scoring value objects and their calculation rules. Following Vernon's (2013) guidance, this shared kernel is deliberately kept minimal.

### Cosmetic Services to Clinical Assessment (Conformist)

The Cosmetic Services Context conforms to the Clinical Assessment Context's patient skin profile model for basic patient information and skin type classification. Cosmetic practitioners need to reference the patient's Fitzpatrick skin type and any contraindicated conditions documented during clinical assessment, but they do not modify the clinical model. The Cosmetic Services Context adopts the Clinical Assessment Context's representation of these shared concepts.

### Pathology Integration to External Laboratory Systems (Anti-Corruption Layer)

The Pathology Integration Context interfaces with external laboratory information systems through an anti-corruption layer. External systems use HL7 messages and proprietary data formats that are translated into the dermatology domain's internal model. This prevents external system concepts and data formats from corrupting the domain model.

### Treatment Outcomes to Clinical Assessment (Feedback Loop)

Treatment outcome data feeds back into the Clinical Assessment Context for subsequent patient encounters. When a dermatologist performs a follow-up examination, outcome data from previous treatments informs the current assessment. This is modeled as a published language where the Treatment Outcomes Context exposes standardized outcome summaries that the Clinical Assessment Context can reference.

## Integration Strategies Summary

| Upstream Context | Downstream Context | Relationship Type |
|---|---|---|
| Clinical Assessment | Lesion Management | Customer-Supplier |
| Lesion Management | Procedure Management | Customer-Supplier |
| Lesion Management | Pathology Integration | Customer-Supplier (bidirectional) |
| Procedure Management | Treatment Outcomes | Customer-Supplier |
| Clinical Assessment | Treatment Outcomes | Shared Kernel |
| Clinical Assessment | Cosmetic Services | Conformist |
| Pathology Integration | External Labs | Anti-Corruption Layer |
| Treatment Outcomes | Clinical Assessment | Published Language |

## Managing the Context Map

The context map must evolve as the dermatology domain evolves. New treatment modalities, changes in regulatory requirements, or the addition of telemedicine capabilities may introduce new bounded contexts or alter existing relationships. Teams should review the context map regularly, particularly when new integration requirements emerge or when existing relationships create friction. Khononov (2021) recommends treating the context map as a living document that reflects the current state of the system.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 14 on context maps.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 3 on context mapping patterns.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 6 on context map patterns and team collaboration.
