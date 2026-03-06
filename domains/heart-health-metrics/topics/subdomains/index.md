# Subdomains

Subdomains in the heart health metrics domain classify cardiac health areas by their strategic importance to the organization. This classification guides investment decisions, build-versus-buy choices, and team allocation across the cardiac monitoring system.

## Overview

Eric Evans distinguishes three types of subdomains: core subdomains that provide competitive advantage, supporting subdomains that are necessary but not differentiating, and generic subdomains that can be sourced from existing solutions. In the heart health metrics domain, this classification determines where organizations invest their most talented domain experts and where they leverage off-the-shelf solutions.

Correct subdomain classification prevents organizations from over-investing in commodity infrastructure while under-investing in the clinical analysis algorithms that differentiate their cardiac monitoring platform.

## Key Concepts

**Core Subdomains.** Cardiac Analysis and Risk Assessment are core subdomains. The algorithms for rhythm classification, arrhythmia detection, HRV computation, and cardiovascular risk scoring embody the differentiating clinical intelligence of the system. These subdomains require deep collaboration between cardiologists and software engineers and justify the highest investment in domain modeling rigor.

**Supporting Subdomains.** Monitoring and Alerts, and Reporting and Visualization are supporting subdomains. They are essential for delivering value to clinicians and patients but do not themselves contain the differentiating clinical logic. Alert rule engines and dashboard frameworks can follow established patterns without requiring novel domain modeling breakthroughs.

**Generic Subdomains.** Data Collection infrastructure and Clinical Integration protocols are generic subdomains. Device communication protocols (Bluetooth LE, IEEE 11073), data ingestion pipelines, and HL7 FHIR resource mapping follow well-established standards. Organizations should leverage existing libraries, SDKs, and middleware rather than building these capabilities from scratch.

**Subdomain Boundaries and Bounded Contexts.** Each subdomain typically aligns with one bounded context, though the mapping is not always one-to-one. The core subdomains demand the most sophisticated bounded context models with carefully designed aggregates, rich value objects, and comprehensive domain events.

**Investment Strategy.** Core subdomains receive the best domain experts, the most rigorous testing, and the deepest domain modeling effort. Supporting subdomains receive adequate but not maximal investment. Generic subdomains are candidates for third-party integration, open-source adoption, or outsourcing.

## Domain Examples

A cardiac monitoring startup identifies its proprietary arrhythmia detection algorithm as a core subdomain. It assigns its top cardiologist and senior engineers to model the Cardiac Analysis Context, ensuring that rhythm classification logic captures the nuances of atrial fibrillation versus atrial flutter versus supraventricular tachycardia. Meanwhile, it uses an open-source FHIR client library for clinical integration (generic subdomain) and a commercial alerting framework for monitoring (supporting subdomain).

A hospital system building an internal cardiac dashboard classifies Reporting and Visualization as a supporting subdomain, using an existing business intelligence platform, while investing deeply in the Risk Assessment Context to implement institution-specific risk scoring models that incorporate their patient population's unique comorbidity profiles.

## Relationships to Other Topics

- [Strategic Design](../strategic-design/index.md) - Subdomain classification is a strategic design decision.
- [Bounded Contexts](../bounded-contexts/index.md) - Subdomains map to bounded contexts.
- [Cardiac Analysis Context](../cardiac-analysis-context/index.md) - A core subdomain.
- [Risk Assessment Context](../risk-assessment-context/index.md) - A core subdomain.
- [Data Collection Context](../data-collection-context/index.md) - A generic subdomain.
- [Clinical Integration Context](../clinical-integration-context/index.md) - A generic subdomain.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 15 on distillation.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 2 on subdomains.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly, 2021. Chapter 1 on business domains and subdomains.
- Porter, Michael E. *Competitive Advantage*. Free Press, 1985.
- Topol, Eric. *The Creative Destruction of Medicine*. Basic Books, 2012.
