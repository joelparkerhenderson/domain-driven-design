# Subdomains - Dental Domain

## Overview

Subdomain classification identifies which areas of the dental domain provide the greatest competitive advantage, which support core activities, and which can leverage existing generic solutions. This classification guides investment decisions, determining where to build custom solutions and where to adopt off-the-shelf capabilities. In the dental domain, the distinction between clinical decision-making and administrative operations provides a natural basis for subdomain classification.

## Core Subdomains

Core subdomains represent areas where the dental practice system creates its primary value and competitive differentiation. These areas require deep domain expertise and custom modeling.

### Clinical Examination

Clinical examination is a core subdomain because accurate diagnosis drives all downstream clinical and financial outcomes. The ability to systematically record, analyze, and present examination findings with precision directly impacts treatment quality. This subdomain requires sophisticated modeling of dental charting, radiographic interpretation workflows, and caries risk assessment algorithms that are specific to dental clinical practice.

### Treatment Planning

Treatment planning is a core subdomain because the ability to create well-sequenced, clinically sound, and financially transparent treatment plans determines patient acceptance and treatment outcomes. The sequencing logic that manages clinical dependencies (a tooth must be extracted before an adjacent bridge can be planned) and the cost estimation rules that account for insurance benefit limitations represent complex domain logic that cannot be adequately handled by generic solutions.

## Supporting Subdomains

Supporting subdomains are necessary for the dental practice to function but do not provide primary competitive differentiation. They implement established clinical protocols with moderate complexity.

### Restorative Services

Restorative services represent a supporting subdomain because restorative procedures follow well-established clinical protocols with standardized material selections and technique sequences. While the tracking of multi-visit procedures (crown preparation, temporary, and cementation) requires dental-specific modeling, the underlying patterns are well understood and relatively stable across practices.

### Orthodontic Management

Orthodontic management is a supporting subdomain because orthodontic treatment follows structured protocols with predictable milestones. The Angle classification system, cephalometric analysis parameters, and wire sequencing protocols are standardized across orthodontic practice. The complexity lies in tracking the long treatment timeline and measuring progress against predictions.

### Periodontal Care

Periodontal care is a supporting subdomain because periodontal disease management follows established clinical guidelines with standardized probing protocols, treatment thresholds, and maintenance intervals. The disease staging and grading systems provide a structured framework that supporting models can implement.

## Generic Subdomains

Generic subdomains address functions that are common across healthcare or business domains and can be served by existing solutions with minimal customization.

### Practice Management - Scheduling

Appointment scheduling is a generic subdomain because scheduling logic (resource allocation across providers, rooms, and time slots) is common across healthcare practices. While dental scheduling has some specific constraints (procedure duration varies by type, operatory equipment requirements), the core scheduling problem is generic.

### Practice Management - Insurance Processing

Insurance verification and claims processing is a generic subdomain because these functions follow standardized ANSI X12 transaction formats and insurance industry protocols. The CDT code mapping adds a dental-specific layer, but the underlying claims workflow is common across healthcare billing.

### Practice Management - Patient Recall

Patient recall management is a generic subdomain because appointment reminder and recall systems are common across healthcare practices. The dental-specific element (recall intervals based on clinical risk assessment) can be parameterized within a generic recall system.

## Subdomain Classification Summary

| Subdomain | Classification | Rationale |
|---|---|---|
| Clinical Examination | Core | Diagnostic accuracy drives clinical and business outcomes |
| Treatment Planning | Core | Sequencing logic and cost transparency create primary value |
| Restorative Services | Supporting | Established protocols with dental-specific tracking needs |
| Orthodontic Management | Supporting | Structured protocols with long timeline tracking |
| Periodontal Care | Supporting | Standardized disease management with longitudinal tracking |
| Scheduling | Generic | Common healthcare resource allocation problem |
| Insurance Processing | Generic | Standardized industry transaction formats |
| Patient Recall | Generic | Common healthcare communication function |

## Investment Implications

Core subdomains warrant the highest investment in domain modeling, expert collaboration, and custom development. Supporting subdomains benefit from solid modeling but can leverage established patterns. Generic subdomains should use existing solutions or frameworks wherever possible, with customization limited to dental-specific parameters.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 15 on distillation and core domain.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 2 on subdomains.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 1 on subdomain types and classification.
