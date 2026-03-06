# Subdomains for Body Health Metrics

## Overview

Subdomain classification organizes the body health metrics domain into areas of differing strategic importance. Each subdomain is categorized as core, supporting, or generic based on its contribution to competitive differentiation, the complexity of its domain logic, and the degree of custom modeling it requires. This classification guides investment decisions, team allocation, and modeling rigor across the system.

## Core Subdomains

Core subdomains provide competitive differentiation and represent the primary value proposition of the body health metrics system. These areas demand the highest investment in domain modeling expertise and design quality.

### Health Scoring

The health scoring subdomain transforms raw measurements into meaningful, actionable health assessments. The scoring algorithms, normalization methods, and composite index formulations are proprietary domain logic that differentiates one body health platform from another. Developing accurate, clinically validated, and intuitively understandable health scores requires deep collaboration between domain experts in exercise physiology, clinical medicine, and data science.

Key complexity drivers:
- Multi-variable scoring algorithms incorporating measurements from diverse modalities
- Age-sex-ethnicity normalization requiring extensive reference population data
- Longitudinal scoring that accounts for measurement trajectory, not just current values
- Clinical validity requirements demanding evidence-based algorithm design

### Progress Tracking

The progress tracking subdomain converts measurement histories into personalized health improvement narratives. The intelligence embedded in goal recommendation, trend detection, milestone calibration, and streak management directly influences user engagement and health outcomes. This subdomain differentiates the system through its ability to motivate sustained behavioral change.

Key complexity drivers:
- Adaptive goal recommendation based on individual measurement history and population trends
- Statistical trend detection that distinguishes meaningful change from measurement noise
- Milestone calibration that balances attainability with meaningful health improvement
- Streak algorithms that account for measurement gaps, holidays, and irregular schedules

## Supporting Subdomains

Supporting subdomains provide essential capabilities that enable core subdomains to function but do not themselves represent primary competitive differentiation. They require solid domain modeling but follow established scientific and clinical practices.

### Biometric Collection

The biometric collection subdomain captures and validates fundamental body measurements. While essential, the science of body measurement is well-established. The domain logic centers on measurement validation, unit conversion, and device calibration status tracking. Custom modeling is needed but follows measurement science conventions.

### Body Composition

The body composition subdomain analyzes body mass distribution using established analytical methods. While the composition models (BIA equations, DEXA interpretation) are scientifically complex, they follow published research rather than requiring novel proprietary algorithms. The domain modeling effort focuses on accurately representing established composition analysis methodologies.

### Fitness Assessment

The fitness assessment subdomain evaluates physical performance using standardized protocols published by organizations such as the American College of Sports Medicine. The assessment methodologies are well-documented in exercise physiology literature. Domain modeling focuses on faithful representation of these protocols and their normative databases.

## Generic Subdomains

Generic subdomains implement widely standardized capabilities that do not require custom domain modeling. Off-the-shelf or standards-based solutions are appropriate for these areas.

### Clinical Integration

The clinical integration subdomain implements data exchange using industry standards including FHIR, LOINC, and SNOMED CT. These standards define data formats, coding systems, and exchange protocols that are common across all health information systems. While integration implementation requires technical expertise, the domain logic follows published specifications rather than custom business rules.

### Supporting Generic Capabilities

Several cross-cutting capabilities fall into the generic category:
- **Unit conversion**: Standard mathematical transformations between SI and imperial units
- **Identity management**: Person identification and authentication
- **Audit logging**: Recording data access and modification events for compliance

## Investment Allocation

The subdomain classification directly informs resource allocation:

- **Core subdomains**: Dedicated domain experts, experienced developers, extensive testing, iterative refinement through continuous domain expert collaboration
- **Supporting subdomains**: Competent development teams with access to domain expertise, thorough but not exhaustive modeling
- **Generic subdomains**: Standards-based implementation, leveraging existing libraries and frameworks where available

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 15 on distillation and core domain identification.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 2 on subdomains.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 1 on subdomain classification strategies.
