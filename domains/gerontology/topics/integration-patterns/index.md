# Integration Patterns in the Gerontology Domain

## Overview

Integration patterns define how bounded contexts interact with each other and with external systems. In the gerontology domain, integration is complex because geriatric care systems must connect with hospital EHRs, pharmacy systems, insurance platforms, community service directories, legal document repositories, and regulatory reporting systems. Evans (2003) and Vernon (2013) describe several integration patterns that apply to these relationships, including shared kernel, published language, open host service, customer-supplier, conformist, and anti-corruption layer.

The choice of integration pattern depends on the organizational relationship between the teams owning the systems, the degree of model alignment, and the tolerance for coupling. In gerontology, where multiple independent organizations participate in a patient's care, integration patterns must accommodate organizational autonomy while ensuring clinical data flows reliably.

## Shared Kernel

A shared kernel is a small, explicitly shared subset of the domain model maintained jointly by two or more bounded context teams. In the gerontology domain, the Geriatric Assessment Context and Functional Independence Context share a kernel containing the PatientDemographics value object and the FunctionalStatusScore value object. Both teams agree on the structure and semantics of these shared objects, and changes require bilateral approval.

The shared kernel is kept deliberately small to minimize coupling. Only concepts that are genuinely shared and stable are included. The kernel does not include context-specific entities or aggregates, which remain within their respective bounded contexts.

## Published Language

Published language is a well-documented, shared data format that bounded contexts use to communicate. In the gerontology domain, several published languages facilitate integration.

Cognitive test scores are published using standardized formats. The MMSE is published as a score from 0 to 30 with interpretation bands (normal: 24-30, mild impairment: 18-23, moderate impairment: 10-17, severe impairment: 0-9). The MoCA is published with its own score range and interpretation. Any context consuming cognitive data can interpret these published formats without bilateral negotiation.

ADL scores are published using the Katz Index format (0-6) and IADL scores using the Lawton Scale format. These standardized clinical instruments serve as natural published languages because their scoring conventions are universally understood in geriatric care.

HL7 FHIR resources serve as the published language for exchanging clinical data with external healthcare systems. Patient demographics, observations, care plans, and medication lists are exchanged using FHIR resource types, providing interoperability with EHR systems and health information exchanges.

## Open Host Service

An open host service exposes a bounded context's capabilities through a well-defined protocol that multiple consumers can use without custom integration. In the gerontology domain, the Care Coordination Context exposes an open host service for querying community resources. The service accepts search criteria (location, service type, eligibility requirements) and returns matching community resources in a standardized format. Multiple contexts and external systems can query this service without requiring context-specific integration.

The Geriatric Assessment Context exposes an open host service for retrieving a patient's current assessment status, enabling authorized external systems to check whether a patient has completed a CGA and what the summary findings are.

## Customer-Supplier

The customer-supplier pattern describes a relationship where one context (supplier) provides data that another context (customer) depends on, and the customer can negotiate the format and content of that data. In the gerontology domain, the Geriatric Assessment Context is the supplier and the Cognitive Health Context is the customer. The Cognitive Health team can request that the assessment context include specific cognitive screening data in its published events, and the assessment team accommodates these requests because they serve a legitimate downstream need.

The Care Coordination Context acts as a customer of the Medication Management Context, requesting that medication change events include the clinical rationale for changes so that care plan documentation can be maintained accurately.

## Conformist

The conformist pattern applies when a downstream context must accept an upstream model without negotiation, typically when the upstream system is outside the organization's control. In the gerontology domain, the Medication Management Context conforms to external pharmacy formulary systems. The formulary provider dictates medication classifications, coverage rules, and prior authorization requirements, and the gerontology domain adapts its model to work with these external classifications.

Similarly, the Care Coordination Context conforms to insurance payer models for service authorization. The payer defines authorization codes, coverage criteria, and approval workflows, and the coordination context works within these constraints.

## Anti-Corruption Layer

The anti-corruption layer pattern is used extensively in the gerontology domain to protect bounded contexts from the models of external systems. ACLs are documented in detail in the anti-corruption layer topic. Key ACL deployments include translation between hospital EHR encounter models and the domain's care episode model, translation between pharmacy NDC codes and the domain's medication appropriateness model, and translation between legal document formats and the domain's advance directive model.

## Synchronous vs. Asynchronous Integration

Within the gerontology domain, bounded contexts primarily communicate asynchronously through domain events. This approach supports temporal decoupling and resilience. Between the domain and external systems, both synchronous and asynchronous integration patterns are used. Real-time queries against formulary systems use synchronous integration, while assessment result sharing with EHR systems may use asynchronous message exchange.

## Data Consistency Across Integration Points

Integration in the gerontology domain must handle the reality that data may be inconsistent across systems. A patient's medication list in the pharmacy system may not match the list in the gerontology domain. Medication reconciliation is both a clinical process and a data integration challenge. The domain models this explicitly rather than assuming consistency.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media.
- HL7 International. (2019). FHIR (Fast Healthcare Interoperability Resources) Specification.
