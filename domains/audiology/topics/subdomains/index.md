# Subdomains in the Audiology Domain

## Overview

Subdomain classification is a strategic design activity that categorizes areas of the audiology domain by their strategic importance and competitive differentiation potential. Domain-Driven Design recognizes three types of subdomains: core, supporting, and generic. This classification drives investment decisions, modeling depth, and build-versus-buy choices for each area of the audiology system.

## Core Subdomain: Hearing Assessment

The Hearing Assessment Context is the core subdomain of the audiology domain. Diagnostic accuracy is the foundation upon which all clinical decisions rest. An incorrect audiogram leads to inappropriate device selection, ineffective rehabilitation, and poor patient outcomes. The core subdomain is where the audiology practice differentiates itself through clinical expertise, advanced testing protocols, and diagnostic precision.

The core subdomain warrants the deepest domain modeling, the most rigorous business rule enforcement, and the closest collaboration between audiologists and developers. The hearing assessment model must capture the nuances of threshold determination, masking rules, cross-hearing physics, and test reliability indicators. Compromising on model fidelity in this subdomain directly compromises patient care.

Investment in the core subdomain should be highest. Custom-built solutions that faithfully represent audiometric concepts are preferred over generic tools that approximate clinical reality. Domain experts from clinical audiology should be continuously involved in refining the assessment model.

## Supporting Subdomains

### Device Management

The Device Management Context is a supporting subdomain. While device fitting and programming are clinically important, the processes follow well-established protocols (NAL-NL2, DSL v5) and manufacturer specifications. Device management supports the clinical decisions made in the core subdomain by implementing prescriptive targets and verifying device output.

The device management model requires solid engineering but can leverage established fitting algorithms and manufacturer APIs. The key modeling challenge is translating diagnostic data from the core subdomain into device-specific programming parameters while maintaining clinical accuracy.

### Rehabilitation

The Rehabilitation Context is a supporting subdomain. Aural rehabilitation programs implement clinical recommendations from the assessment context and complement device fitting. Rehabilitation follows evidence-based protocols and outcome measurement frameworks that, while important, do not represent the primary point of clinical differentiation.

The rehabilitation model focuses on tracking intervention plans, progress metrics, and functional outcomes. It supports the core subdomain by demonstrating the long-term effectiveness of diagnostic and treatment decisions.

### Vestibular Services

The Vestibular Services Context is a supporting subdomain. Balance assessment and vestibular rehabilitation serve a subset of audiology patients and follow specialized but standardized protocols. While vestibular services require domain expertise, they support rather than define the audiology practice's core value proposition.

### Pediatric Audiology

The Pediatric Audiology Context is a supporting subdomain that adapts core assessment concepts for pediatric populations. Pediatric audiology requires specialized testing methods and developmental monitoring but builds upon the same fundamental audiometric principles as the core subdomain. The shared kernel between Pediatric Audiology and Hearing Assessment reflects this relationship.

## Generic Subdomain: Clinical Documentation

The Clinical Documentation Context is a generic subdomain. Documentation, reporting, and referral management are essential operational functions but do not differentiate the audiology practice. Every healthcare practice needs documentation capabilities, and the fundamental requirements (report generation, storage, distribution, compliance) are common across medical specialties.

Generic subdomains are candidates for off-the-shelf solutions or standardized implementations. The Clinical Documentation Context may integrate with existing electronic health record (EHR) systems rather than requiring custom development. The key requirement is that the documentation context accurately captures and formats the clinical data produced by other contexts without introducing its own clinical interpretations.

## Classification Criteria

The subdomain classification for audiology follows three criteria. First, competitive advantage: does excellence in this area differentiate the practice? Hearing assessment accuracy directly differentiates clinical quality. Second, complexity: does this area contain complex, domain-specific business rules? Assessment masking rules, cross-hearing physics, and threshold determination algorithms represent deep domain complexity. Third, volatility: how frequently do practices and standards change? Assessment protocols evolve as new research emerges, requiring a model that can adapt without structural upheaval.

## Strategic Implications

The subdomain classification informs several strategic decisions. For the core subdomain, the organization invests in deep domain expertise, custom modeling, and continuous refinement. For supporting subdomains, the organization builds solid implementations that faithfully execute established protocols. For the generic subdomain, the organization evaluates commercial solutions and focuses custom development only on integration requirements.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley. Chapter 15 on distillation and core domain.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley. Chapter 2 on subdomains.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media. Chapter 3 on subdomain types and classification.
