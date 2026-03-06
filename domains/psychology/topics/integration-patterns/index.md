# Integration Patterns in the Psychology Domain

## Overview

Integration patterns define how bounded contexts communicate, share data, and manage dependencies. Domain-Driven Design identifies several integration patterns including shared kernel, published language, open host service, customer-supplier, conformist, and anti-corruption layer. In the psychology domain, these patterns govern how clinical assessment, therapeutic intervention, behavioral analysis, cognitive evaluation, research methods, and outcomes measurement work together while maintaining their distinct models and ubiquitous languages.

Choosing the right integration pattern for each context relationship is a strategic design decision. The choice depends on the power dynamics between teams, the degree of model overlap, the rate of change in each context, and the clinical requirements for data fidelity and timeliness.

## Shared Kernel

A shared kernel is a small, explicitly defined subset of the domain model that is shared between two or more bounded contexts. Changes to the shared kernel require coordination between all contexts that use it.

In the psychology domain, the Research Methods Context and the Outcomes Measurement Context share a kernel for statistical analysis. Both contexts use Effect Size, Confidence Interval, and Statistical Significance value objects. These value objects have identical definitions and behavior in both contexts because they represent the same mathematical concepts. The shared kernel ensures consistency in statistical calculations across research and clinical outcomes reporting.

The shared kernel should be kept as small as possible. Only the value objects that are truly identical across contexts belong in the kernel. The temptation to expand the shared kernel to include more concepts should be resisted, as each addition increases the coordination cost between contexts.

## Published Language

A published language is a well-documented, standardized format for exchanging information between contexts. Unlike a shared kernel, a published language does not require shared code -- it defines a data format that both sides agree to.

The Behavioral Analysis Context publishes behavioral data using a standardized format that the Outcomes Measurement Context can consume. This published language defines how behavior rates, intervention fidelity scores, and goal attainment levels are expressed. The format uses operational definitions, standardized units of measurement, and agreed-upon data structures.

Similarly, the Psychological Assessment Context publishes assessment summaries in a standardized clinical format that downstream contexts can consume. The published language defines the structure of diagnostic formulations, functional descriptions, and treatment recommendations.

## Open Host Service

An open host service provides a well-defined protocol for accessing a bounded context's capabilities. It serves as a stable interface that multiple consumers can use without requiring bilateral negotiation.

The Outcomes Measurement Context can expose an open host service that allows multiple contexts to submit outcome data and retrieve outcome reports. The Therapeutic Intervention Context submits session-level outcome data. The Behavioral Analysis Context submits behavior data. The Research Methods Context queries aggregate outcome data for research purposes. All interactions follow the same published protocol.

## Customer-Supplier

In a customer-supplier relationship, the downstream (customer) context depends on data or services from the upstream (supplier) context. The supplier context accommodates the customer's needs within reason.

The Psychological Assessment Context (supplier) and Therapeutic Intervention Context (customer) have a customer-supplier relationship. The assessment context produces diagnostic formulations and treatment recommendations that the therapeutic context consumes. The assessment context accommodates the therapeutic context's need for clinically actionable summaries rather than raw psychometric data.

The Research Methods Context (customer) consumes standardized instruments from the Psychological Assessment Context (supplier) when studies use psychometric measures. The assessment context provides normative data and reliability information that the research context needs for measurement validation.

## Conformist

In a conformist relationship, the downstream context adopts the upstream context's model without modification. This pattern is appropriate when the upstream model is authoritative and the downstream context has no leverage to request changes.

The Therapeutic Intervention Context conforms to the Outcomes Measurement Context's standardized measurement protocols. When the outcomes context adopts a new outcome measure or updates the measurement schedule, the therapeutic context adapts its session workflow to accommodate the new requirements. The outcomes context's measurement model is authoritative because it reflects external standards and payer requirements.

## Anti-Corruption Layer

The anti-corruption layer (ACL) is a translation boundary that protects a context from external models. The ACL is detailed in its own topic, but its role as an integration pattern warrants mention here.

ACLs are used between the Psychological Assessment Context and the Therapeutic Intervention Context to translate psychometric data into clinical treatment concepts. ACLs are used between clinical contexts and the Research Methods Context to translate identified clinical data into de-identified research variables. ACLs protect clinical contexts from external systems like EHRs and insurance platforms.

## Integration with External Systems

Psychology practice systems integrate with several external systems. Electronic Health Records (EHRs) use HL7 FHIR or similar healthcare interoperability standards. Insurance systems use X12 EDI transactions for claims and authorizations. Test publisher platforms provide scoring services and normative databases. Each external integration requires an ACL that translates between the psychology domain model and the external system's model.

## Pattern Selection Guidelines

When both contexts are under the same team's control and share genuine model overlap, use a shared kernel. When contexts need to exchange data but maintain independent models, use published language. When one context serves multiple consumers with the same interface, use open host service. When one context depends on another and can negotiate the interface, use customer-supplier. When the upstream model is authoritative and non-negotiable, use conformist. When an external model threatens to corrupt the internal model, use an anti-corruption layer.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley. Chapter 14 on context mapping patterns.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley. Chapter 3 on integration patterns.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media. Chapter 5 on integration between bounded contexts.
- Hohpe, G., & Woolf, B. (2003). Enterprise Integration Patterns. Addison-Wesley.
