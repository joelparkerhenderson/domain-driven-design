# Subdomains - Kinesiology Domain

## Overview

In Domain-Driven Design, subdomains represent distinct areas of business activity within a domain. Classifying subdomains by strategic importance guides where to invest the most modeling effort, where to seek simpler solutions, and where off-the-shelf approaches may suffice. The kinesiology domain contains core, supporting, and generic subdomains that reflect the varying strategic value of different areas of movement science practice.

Subdomain classification is not about technical complexity. A technically complex subsystem may be generic if it does not provide competitive differentiation. Conversely, a conceptually straightforward subsystem may be core if it embodies the unique expertise that distinguishes the kinesiology system.

## Core Subdomains

Core subdomains represent the areas where the kinesiology system must excel. These are the differentiators, the capabilities that justify building a specialized system rather than using generic tools.

### Movement Assessment

Movement assessment is a core subdomain because the quality and sophistication of clinical evaluation directly determines the value of all downstream activities. A kinesiology system that cannot capture, interpret, and communicate assessment findings with clinical precision fails to serve its fundamental purpose. The assessment model must support diverse evaluation methodologies, from simple functional screens to comprehensive biomechanical evaluations, and must represent clinical findings with sufficient granularity to inform exercise prescription and rehabilitation planning.

Investment in the Movement Assessment model should be substantial. The domain model here warrants rich entities, carefully designed value objects for measurements and scores, and sophisticated invariants that enforce clinical validity.

### Exercise Prescription

Exercise prescription is a core subdomain because it represents the primary value-creating activity in most kinesiology systems. Translating assessment findings into effective, individualized training programs requires deep domain expertise in exercise science, training theory, and program design. The prescription model must capture the nuances of periodization, progressive overload, exercise selection criteria, and contraindication logic that distinguish expert practice from generic programming.

The domain model for Exercise Prescription should be the most richly elaborated in the system, with complex aggregate structures, detailed value objects for training parameters, and domain services that encode exercise science decision rules.

## Supporting Subdomains

Supporting subdomains are necessary for the system to function effectively but do not represent the primary competitive advantage.

### Rehabilitation Planning

Rehabilitation planning extends exercise prescription into clinical recovery contexts. While it requires specialized knowledge of tissue healing, clinical milestones, and return-to-play protocols, these concepts follow well-established clinical frameworks. The domain model must be clinically sound but can leverage established rehabilitation science patterns rather than inventing novel approaches.

The model complexity is moderate. Rehabilitation protocols follow phase-based progressions with defined criteria for advancement, which can be effectively captured through aggregate-based patterns with clear state transitions.

### Performance Analysis

Performance analysis provides advanced measurement capabilities that enhance the system's assessment and monitoring functions. While the technical infrastructure for force plate analysis and motion capture is sophisticated, the domain logic for interpreting these measurements follows established biomechanical analysis conventions.

The model emphasis is on accurate representation of measurement data and derived metrics. Value objects for forces, moments, and kinematic variables must be precisely defined, but the analysis workflows themselves are well-documented in biomechanics literature.

### Injury Prevention

Injury prevention synthesizes data from multiple sources to predict and mitigate injury risk. The epidemiological models and risk calculation approaches are grounded in sports medicine research. While injury prevention is clinically important, the modeling patterns draw heavily on established risk assessment frameworks.

The model focuses on aggregating risk factors from multiple contexts and applying evidence-based thresholds to generate risk alerts. The domain logic, while clinically important, follows documented epidemiological patterns.

## Generic Subdomains

Generic subdomains can leverage existing solutions with minimal customization.

### Research and Education

Research and education management, including literature curation, curriculum design, and continuing education tracking, is a well-understood problem domain. Knowledge management systems, learning management systems, and professional development platforms are mature product categories.

The domain model can be relatively simple, focusing on organizing evidence by topic and strength, tracking educational progress, and managing certification requirements. This subdomain benefits from adopting established patterns rather than building custom solutions.

## Investment Allocation

The subdomain classification directly informs where the team invests its domain modeling effort. Core subdomains receive the most attention: multiple modeling iterations, extensive domain expert collaboration, and the richest tactical patterns. Supporting subdomains receive competent modeling with established patterns. Generic subdomains may use simplified models or integrate with existing solutions.

This allocation prevents the common mistake of investing equal effort across all areas, which dilutes attention from the aspects of the system that matter most.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 15 on distillation and core domain.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 2 on subdomain classification.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 1 on subdomain types and strategic importance.
