# Context Map - Kinesiology Domain

## Overview

A context map provides a visual and textual representation of the relationships and interactions between bounded contexts in a domain. For the kinesiology domain, the context map documents how the six bounded contexts exchange information, which contexts are upstream or downstream, and what integration patterns govern their interactions.

The context map is not a static artifact. It evolves as the domain model matures and as organizational structures change. It serves as a communication tool for domain experts, developers, and stakeholders to understand the system's integration architecture at a glance.

## Context Relationships

### Movement Assessment to Exercise Prescription

Relationship type: Customer-Supplier. Movement Assessment is the upstream supplier, and Exercise Prescription is the downstream customer. When an assessment is completed, the Movement Assessment Context publishes an AssessmentCompleted event containing a summary of findings. The Exercise Prescription Context consumes this event to inform program design decisions.

The downstream context has influence over what data the upstream context provides, consistent with the customer-supplier pattern. Exercise Prescription may request that Movement Assessment include specific test results, such as one-repetition maximum estimates or flexibility scores, in its published events.

### Movement Assessment to Rehabilitation Planning

Relationship type: Customer-Supplier. Movement Assessment supplies evaluation data to Rehabilitation Planning. However, Rehabilitation Planning has additional clinical requirements, such as pain assessments and tissue healing stage classifications, that may not be part of standard movement assessment. The rehabilitation context extends assessment data with its own clinical observations.

### Movement Assessment to Injury Prevention

Relationship type: Published Language. Movement Assessment publishes screening results using a standardized data contract that Injury Prevention consumes. The published language defines the exact format of movement quality scores, asymmetry indices, and functional test results. This ensures Injury Prevention can reliably interpret screening data regardless of which specific assessment battery was used.

### Performance Analysis to Movement Assessment

Relationship type: Conformist. Performance Analysis provides instrumented measurement data that enriches Movement Assessment findings. The Movement Assessment Context adopts the data formats and conventions of the Performance Analysis Context without modification. This conformist relationship is appropriate because performance analysis data (force plate readings, motion capture coordinates) has inherent precision requirements that should not be altered during translation.

### Performance Analysis to Injury Prevention

Relationship type: Published Language. Performance Analysis publishes workload metrics, force asymmetry data, and neuromuscular fatigue indicators using a shared data contract. Injury Prevention consumes these metrics to update risk profiles. The published language ensures consistent interpretation of quantitative thresholds.

### Exercise Prescription to Injury Prevention

Relationship type: Customer-Supplier. Injury Prevention is the downstream customer, consuming training load data published by Exercise Prescription. This relationship enables Injury Prevention to monitor acute-to-chronic workload ratios and flag programs that may elevate injury risk.

### Rehabilitation Planning to Exercise Prescription

Relationship type: Shared Kernel. These two contexts share a small set of core concepts, including exercise specifications, dosage parameters, and progression rules. The shared kernel is carefully controlled and versioned to prevent unintended coupling. Changes to shared concepts require agreement from both context teams.

### Research and Education to All Contexts

Relationship type: Open Host Service. The Research and Education Context exposes its evidence base through a well-defined service interface that any context can query. Clinical practice guidelines, evidence summaries, and recommended protocols are available to all contexts through this open host service. Each consuming context applies its own anti-corruption layer to translate research findings into context-specific recommendations.

## Integration Topology

The overall integration topology follows a hub-and-spoke pattern with Movement Assessment at the center. Assessment data flows outward to Exercise Prescription, Rehabilitation Planning, and Injury Prevention. Performance Analysis feeds enrichment data into Movement Assessment and directly into Injury Prevention. Research and Education serves as a knowledge utility accessible by all contexts.

Event flow is predominantly unidirectional, moving from assessment through prescription or rehabilitation and into prevention. Feedback loops exist where Injury Prevention sends risk alerts back to Exercise Prescription to trigger program modifications, and where Rehabilitation Planning sends milestone events that prompt reassessment in the Movement Assessment Context.

## Conflict Resolution

When bounded contexts have conflicting interpretations of shared concepts, the context map documents the translation strategy. For example, "exercise intensity" means percentage of one-repetition maximum in Exercise Prescription but may mean rating of perceived exertion in Rehabilitation Planning. The context map explicitly documents these translation rules to prevent misinterpretation.

## Maintaining the Context Map

The context map should be reviewed whenever bounded context boundaries change, when new integration patterns are introduced, or when stakeholder feedback indicates conceptual confusion. Regular review sessions with representatives from each bounded context ensure the map remains accurate and useful.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 14 on context maps.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 3 on context mapping patterns.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 5 on context mapping strategies.
- Brandolini, Alberto. "Context Mapping." DDD Community, 2009.
