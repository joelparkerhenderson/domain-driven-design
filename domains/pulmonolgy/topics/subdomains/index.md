# Subdomains in the Pulmonolgy Domain

## Overview

Subdomains classify areas of a domain by their strategic importance to the organization. In Domain-Driven Design, every business domain consists of multiple subdomains, each contributing differently to the organization's competitive advantage and operational capability. The classification guides where to invest the most modeling effort, where to adopt standardized solutions, and where to minimize complexity.

In the pulmonolgy domain, subdomain classification reflects the clinical and organizational value of each area of respiratory medicine. Not all areas of pulmonology demand equal modeling sophistication. Some represent core differentiators that warrant deep domain modeling, while others follow standardized protocols where simpler approaches suffice.

## Core Subdomains

Core subdomains represent the highest strategic value and competitive differentiation. They contain complex business logic that is unique to the organization and difficult for competitors to replicate. In the pulmonolgy domain, two bounded contexts qualify as core subdomains.

### Respiratory Diagnostics

The Respiratory Diagnostics Context is a core subdomain because diagnostic accuracy and procedural excellence define the quality of a pulmonology practice. The modeling of spirometry interpretation algorithms, bronchoscopy workflow management, specimen tracking chains, and diagnostic classification logic requires deep domain expertise and represents significant intellectual investment.

Advanced diagnostic capabilities, including pattern recognition for complex obstructive-restrictive overlap, multi-test correlation analysis, and longitudinal trend detection, provide clinical differentiation. The domain model must capture nuanced diagnostic reasoning that reflects expert clinical judgment.

### Chronic Disease Management

The Chronic Disease Management Context is a core subdomain because long-term patient outcomes depend on sophisticated treatment plan modeling, exacerbation prediction, and outcome tracking. The complexity of managing multiple chronic respiratory conditions, each with disease-specific classification systems, treatment protocols, and monitoring requirements, demands the most rigorous domain modeling.

This subdomain captures competitive value through superior care coordination, earlier exacerbation detection, more effective treatment plan optimization, and better long-term outcome measurement. The domain model must represent the full complexity of multi-disease management with overlapping treatment considerations.

## Supporting Subdomains

Supporting subdomains are essential to clinical operations but are more standardized and less likely to provide competitive differentiation. They still require custom domain modeling but at a lower complexity level than core subdomains.

### Pulmonary Assessment

The Pulmonary Assessment Context is a supporting subdomain. While clinical assessment is essential to all downstream workflows, the assessment process follows well-established clinical protocols. The domain model must accurately represent pulmonary function test ordering, result interpretation, and symptom evaluation, but these workflows are relatively standardized across pulmonology practices.

### Critical Care

The Critical Care Context is a supporting subdomain. Mechanical ventilation management and ARDS protocols are critically important but follow evidence-based guidelines that are broadly standardized (such as ARDSNet protocols). The domain model captures ventilator settings, weaning readiness criteria, and ICU protocols, but these are well-defined clinical algorithms rather than areas of significant clinical differentiation.

### Sleep Medicine

The Sleep Medicine Context is a supporting subdomain. Sleep study interpretation follows AASM scoring rules, OSA severity classification uses standardized AHI thresholds, and CPAP therapy management follows established titration and compliance protocols. While sleep medicine requires specialized knowledge, the workflows are relatively standardized.

## Generic Subdomains

Generic subdomains provide necessary functionality but follow widely standardized approaches. They offer minimal competitive differentiation and can often leverage existing frameworks or off-the-shelf solutions with modest customization.

### Pulmonary Rehabilitation

The Pulmonary Rehabilitation Context is a generic subdomain. Rehabilitation programs follow well-established ATS/ERS guidelines for exercise training, breathing technique instruction, patient education, and outcome measurement. The six-minute walk test, Borg dyspnea scale, and standardized exercise protocols are universally applied. While rehabilitation is clinically important, the domain model can follow standard patterns without extensive custom modeling.

## Strategic Investment Implications

The subdomain classification directly guides development strategy:

- **Core subdomains** receive the deepest domain modeling, the most experienced domain experts, and the highest development investment. Custom, in-house development is preferred.

- **Supporting subdomains** receive solid domain modeling with attention to integration points. Development may mix custom work with adaptation of existing patterns.

- **Generic subdomains** receive minimal custom modeling. Existing frameworks, standardized protocols, and proven patterns are leveraged to reduce development effort.

## Subdomain Boundaries and Overlap

Subdomain boundaries align with but are not identical to bounded context boundaries. In the pulmonolgy domain, the one-to-one mapping between subdomains and bounded contexts simplifies governance. However, it is important to recognize that subdomain classification may shift over time. A supporting subdomain may become a core subdomain if the organization decides to invest in clinical differentiation in that area.

For example, if a pulmonology practice decides to develop advanced predictive analytics for critical care ventilation management, the Critical Care Context might be reclassified from supporting to core. The subdomain classification should be reviewed periodically as organizational strategy evolves.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 15 on distilling core domain and identifying supporting and generic subdomains.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 2 on subdomain identification and strategic classification.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 1 on subdomain types and their impact on design decisions.
