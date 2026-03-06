# Integration Patterns in the Dermatology Domain

Integration patterns define how bounded contexts communicate, share data, and maintain consistency within the dermatology domain. Following the patterns cataloged by Evans (2003) and elaborated by Vernon (2013) and Khononov (2021), the dermatology domain uses a combination of shared kernel, published language, open host service, customer-supplier, conformist, and anti-corruption layer patterns to manage inter-context relationships.

## Shared Kernel Pattern

The shared kernel pattern is used between the Clinical Assessment Context and the Treatment Outcomes Context for clinical severity scoring systems. Both contexts need to calculate and interpret PASI scores, SCORAD indices, and DLQI scores. Rather than duplicating the scoring logic and risking inconsistency, these contexts share a small, carefully managed kernel containing the scoring value objects and their calculation rules.

The shared kernel includes the PASI Score value object with its four-region calculation algorithm, the SCORAD Index value object with its extent-intensity-symptom formula, and the DLQI Score value object with its ten-question structure and severity banding. Both contexts import these shared definitions and agree to coordinate any changes to them. The kernel is deliberately kept minimal -- it includes only the scoring calculations and not the broader assessment or treatment models.

Changes to the shared kernel require agreement from teams owning both contexts. Vernon (2013) warns that shared kernels create coupling and should be used only when the shared concepts are truly stable and the cost of duplication exceeds the cost of coordination. In dermatology, standardized scoring systems like PASI and SCORAD change infrequently and are defined by external clinical bodies, making them good candidates for a shared kernel.

## Published Language Pattern

The Treatment Outcomes Context uses a published language to expose standardized outcome summaries that other contexts can consume. This published language defines the format and semantics of treatment response data, severity score trends, and recurrence status in a way that consuming contexts can understand without knowledge of the Treatment Outcomes Context's internal model.

The published language includes outcome summary structures that carry the treatment modality, diagnosis, response classification, score trajectory (improving, stable, worsening), and time since treatment initiation. The Clinical Assessment Context consumes this published language to display relevant outcome history during patient encounters. The Lesion Management Context consumes it to inform follow-up scheduling and recurrence monitoring decisions.

The Pathology Integration Context also uses a published language -- the structured pathology report format -- that defines how histopathology findings, diagnoses, and staging information are communicated to consuming contexts. This published language translates the rich pathology domain into a structured format that the Lesion Management and Treatment Outcomes Contexts can interpret.

## Open Host Service Pattern

The Pathology Integration Context provides an open host service for laboratory communication. This service exposes well-defined interfaces for specimen submission, status inquiry, and result retrieval using standardized healthcare interoperability protocols (HL7 v2, FHIR). External laboratories interact with this service using established healthcare messaging standards, and internal contexts interact with it through the domain's event-driven architecture.

The open host service decouples the internal domain model from the external communication protocol. New laboratory integrations can be added by implementing additional protocol adapters behind the same open host interface. This follows Evans' (2003) recommendation that open host services should provide a protocol layer that gives access to the system using a well-documented, stable protocol.

## Customer-Supplier Pattern

Several inter-context relationships in the dermatology domain follow the customer-supplier pattern, where the upstream context (supplier) provides data or services that the downstream context (customer) depends on.

Clinical Assessment to Lesion Management: The Clinical Assessment Context supplies lesion findings that the Lesion Management Context consumes. The Lesion Management Context has legitimate needs that influence what information the Clinical Assessment Context must provide (such as dermoscopic patterns, ABCDE scores, and anatomical locations with sufficient precision for surgical planning). The teams negotiate the content and format of LesionIdentified events.

Lesion Management to Procedure Management: The Lesion Management Context supplies procedure requests (through ExcisionPlanFinalized events) that the Procedure Management Context uses to plan and execute procedures. The Procedure Management Context needs specific information (diagnosis, margin requirements, anatomical location, patient skin type) that influences what the Lesion Management Context includes in its events.

Procedure Management to Treatment Outcomes: The Procedure Management Context supplies procedure completion data that the Treatment Outcomes Context uses to initiate outcome tracking. The Treatment Outcomes Context needs procedure parameters, immediate outcomes, and complication data to establish the baseline for longitudinal tracking.

## Conformist Pattern

The Cosmetic Services Context adopts a conformist relationship with the Clinical Assessment Context for patient skin profile data. The Cosmetic Services Context accepts the Clinical Assessment Context's model for Fitzpatrick skin type, documented skin conditions, and medication history without attempting to negotiate changes to these models. This is appropriate because the clinical assessment model for these concepts is well-established, clinically standardized, and the Cosmetic Services Context has no domain-specific reason to model them differently.

## Anti-Corruption Layer Pattern

The Pathology Integration Context uses anti-corruption layers when interfacing with external laboratory information systems. Each external laboratory has its own data format, terminology, and communication protocol. The ACL translates between these external representations and the domain's internal model, protecting the domain from being corrupted by external system concepts. Detailed ACL design is covered in the anti-corruption layer topic.

## Choosing Integration Patterns

The selection of integration patterns follows guidelines from Khononov (2021). Shared kernel is chosen when contexts share stable, slowly evolving concepts and the teams are closely aligned. Published language is chosen when a context needs to expose data for consumption by multiple other contexts with different needs. Customer-supplier is chosen when there is a clear directional dependency and the teams can negotiate interface requirements. Conformist is chosen when the downstream context has no leverage or desire to influence the upstream model. Anti-corruption layer is chosen when integrating with external systems whose models differ fundamentally from the domain.

## Pattern Evolution

Integration patterns may evolve as the dermatology domain matures. A conformist relationship may shift to customer-supplier as the downstream team's needs become more specific. A shared kernel may be dissolved into a published language if the coordination overhead exceeds its benefits. The context map should be reviewed periodically to ensure that integration patterns still reflect the actual relationships and power dynamics between teams and contexts.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 14 on context mapping patterns.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 3 and Chapter 13 on integration patterns.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 6 on bounded context integration.
