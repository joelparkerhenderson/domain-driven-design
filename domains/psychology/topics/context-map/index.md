# Context Map in the Psychology Domain

## Overview

A context map is a visual and conceptual representation of the relationships and interactions between bounded contexts within a domain. In the psychology domain, the context map captures how the six bounded contexts -- Psychological Assessment, Therapeutic Intervention, Behavioral Analysis, Cognitive Assessment, Research Methods, and Outcomes Measurement -- communicate, share data, and depend on one another.

The context map makes explicit the power dynamics, data flow directions, and translation requirements between contexts. It prevents hidden dependencies and ensures that integration points are deliberately designed rather than accidentally coupled.

## Context Relationships

### Psychological Assessment to Therapeutic Intervention

Relationship type: Customer-Supplier. The Therapeutic Intervention Context is the customer that consumes assessment results to inform treatment planning. The Psychological Assessment Context is the supplier that produces diagnostic formulations and test interpretations. An anti-corruption layer translates detailed psychometric data into clinically actionable treatment recommendations. The assessment context publishes an AssessmentCompleted event that the therapeutic context consumes.

### Psychological Assessment to Cognitive Assessment

Relationship type: Partnership. These two contexts collaborate closely because comprehensive psychological evaluations often include cognitive components. They share a published language for referral information and test results. Neither context dominates; both teams coordinate their models to ensure compatibility. Referrals flow from psychological assessment to cognitive assessment, and cognitive profiles flow back to inform the overall assessment report.

### Therapeutic Intervention to Behavioral Analysis

Relationship type: Customer-Supplier. When a therapist identifies behavior patterns requiring systematic analysis, the Behavioral Analysis Context provides functional assessment services. The therapeutic context is the customer requesting behavioral insights, and the behavioral analysis context is the supplier delivering behavior plans. An anti-corruption layer ensures that behavioral terminology (antecedent, consequence, reinforcement) is translated into therapeutically relevant concepts.

### Therapeutic Intervention to Outcomes Measurement

Relationship type: Conformist. The Therapeutic Intervention Context conforms to the Outcomes Measurement Context's standardized measures and reporting formats. When outcome measures are updated or new measures are adopted, the therapeutic context adapts its progress tracking to match. The outcomes context publishes measurement protocols that the therapeutic context implements during session management.

### Research Methods to Outcomes Measurement

Relationship type: Shared Kernel. These two contexts share a common model for statistical analysis and effect size calculation. The shared kernel includes value objects for Effect Size, Confidence Interval, and Statistical Significance. Both contexts depend on this shared model, and changes require coordination between research and outcomes teams.

### Research Methods to Psychological Assessment

Relationship type: Customer-Supplier. The Research Methods Context consumes psychometric instruments from the Psychological Assessment Context when studies use standardized measures. The assessment context supplies normative data and reliability information that the research context uses to validate its measurement procedures.

### Behavioral Analysis to Outcomes Measurement

Relationship type: Published Language. The Behavioral Analysis Context publishes behavior data using a standardized format that the Outcomes Measurement Context can consume for treatment effectiveness tracking. The published language defines how behavior rates, intervention fidelity scores, and goal attainment levels are expressed.

### Cognitive Assessment to Research Methods

Relationship type: Customer-Supplier. Research studies frequently use cognitive measures as dependent variables. The Research Methods Context is the customer that consumes cognitive assessment instruments and normative data from the Cognitive Assessment Context.

## Data Flow Summary

Assessment results flow downstream to treatment planning and research. Behavior analysis data flows to both therapeutic intervention and outcomes measurement. Outcomes data flows to research methods for aggregate analysis and benchmarking. Cognitive assessment results flow to both psychological assessment and research methods.

## Upstream and Downstream Patterns

Upstream contexts (Psychological Assessment, Cognitive Assessment) produce foundational data that downstream contexts consume. Downstream contexts (Therapeutic Intervention, Outcomes Measurement) depend on upstream data quality and availability. The context map ensures that upstream changes do not break downstream consumers by requiring anti-corruption layers or conformist patterns at integration points.

## Maintaining the Context Map

The context map is a living document that evolves as the psychology domain model matures. When new bounded contexts emerge, such as a Telehealth Context or Digital Therapeutics Context, the map must be updated to reflect new relationships. Regular review of the context map by domain experts, clinicians, and developers ensures that it accurately reflects actual system interactions.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley. Chapter 14 on context maps.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley. Chapter 3 on context mapping.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media. Chapter 5 on context mapping patterns.
- Brandolini, A. (2021). Event Storming. Leanpub. On collaborative domain discovery.
