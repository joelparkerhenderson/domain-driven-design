# Subdomains in the Psychology Domain

## Overview

Subdomains classify areas of a domain by their strategic importance to the organization. Domain-Driven Design identifies three types of subdomains: core, supporting, and generic. In the psychology domain, this classification guides where to invest the most modeling effort, where to leverage existing solutions, and where to build supporting capabilities that enable core functions.

The classification of subdomains is not universal. A university research hospital may classify Research Methods as a core subdomain, while a private therapy practice would classify Therapeutic Intervention as core and Research Methods as generic. The classification depends on the organization's mission, competitive strategy, and value proposition.

## Core Subdomains

Core subdomains are the areas where the organization creates its primary competitive advantage. They require the deepest domain modeling, the most sophisticated business logic, and the greatest investment in custom development.

### Psychological Assessment (Core)

For organizations that provide psychological evaluations, the assessment process is a core differentiator. The ability to select appropriate test batteries, administer tests according to standardized protocols, apply complex scoring algorithms, interpret results in clinical context, and generate comprehensive reports represents the primary value delivered to clients and referral sources. The domain model must capture the full complexity of psychometric theory, normative comparisons, and clinical interpretation.

### Therapeutic Intervention (Core)

For clinical practices, therapeutic intervention is the primary revenue-generating activity and the source of clinical reputation. The domain model must accommodate multiple evidence-based modalities, track treatment fidelity, manage therapeutic relationships, and support clinical decision-making. Treatment protocol modeling, session management, and progress tracking require deep domain expertise and custom solutions.

## Supporting Subdomains

Supporting subdomains enable core functions without being the primary value proposition. They require some custom modeling but may leverage established frameworks and standards.

### Behavioral Analysis (Supporting)

Behavioral analysis supports core therapeutic activities by providing systematic assessment of behavior patterns and evidence-based intervention strategies. While behavior analysts may use specialized ABA techniques, the behavioral data ultimately feeds into treatment planning and outcomes measurement. The domain model must be accurate but can follow established ABA frameworks without extensive custom innovation.

### Cognitive Assessment (Supporting)

Cognitive assessment supports the broader assessment and treatment contexts by providing specialized evaluation of cognitive functioning. Cognitive screening and neuropsychological testing follow well-established protocols with standardized scoring. The domain model supports core assessment activities and informs treatment recommendations.

### Outcomes Measurement (Supporting)

Outcomes measurement supports both clinical practice and organizational quality improvement. It tracks treatment effectiveness using standardized instruments and provides benchmarking data. While critically important for evidence-based practice, outcomes measurement typically follows established measurement frameworks and statistical methods.

## Generic Subdomains

Generic subdomains address common business needs that are not unique to psychology practice. These areas should leverage existing solutions rather than consuming custom development resources.

### Scheduling and Appointment Management

Appointment scheduling, calendar management, and resource allocation are generic business functions. Off-the-shelf scheduling solutions can be integrated through anti-corruption layers that translate scheduling concepts into domain-relevant terms like session times and assessment blocks.

### Billing and Insurance Processing

Insurance claims, billing codes (CPT codes for psychological services), and payment processing are complex but generic. Existing healthcare billing solutions can handle these functions when integrated with domain-specific procedure code mappings.

### Document Management

Storage, retrieval, and versioning of clinical documents, assessment reports, and research manuscripts are generic functions addressed by standard document management systems.

## Strategic Implications

The subdomain classification drives resource allocation. Core subdomains receive the most experienced domain experts and developers. Supporting subdomains receive adequate but not maximum investment. Generic subdomains are addressed with existing solutions or minimal custom development.

When organizational priorities shift, subdomain classifications may change. A practice that begins offering specialized neuropsychological services may reclassify Cognitive Assessment from supporting to core. A research institution that establishes an outcomes research center may reclassify Outcomes Measurement as core.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley. Chapter 15 on distillation and core domain.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley. Chapter 2 on subdomains.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media. Chapter 1 on analyzing business domains and subdomains.
- Porter, M. E. (1985). Competitive Advantage. Free Press. On strategic positioning and value chain analysis.
