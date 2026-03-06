# Context Map for Body Health Metrics

## Overview

The context map provides a visual and conceptual representation of how the six bounded contexts in the body health metrics domain relate to one another. It documents the integration patterns, data flow directions, and organizational relationships that govern inter-context communication. The context map serves as the primary strategic artifact for understanding system-wide architecture.

## Context Relationships

### Biometric Collection to Body Composition

- **Pattern**: Customer-Supplier
- **Direction**: Biometric Collection (upstream) supplies measurement data to Body Composition (downstream)
- **Integration**: Body Composition subscribes to BiometricMeasurementRecorded events and translates raw measurements into composition analysis inputs
- **Conformist aspects**: Body Composition conforms to the measurement data format published by Biometric Collection

### Biometric Collection to Fitness Assessment

- **Pattern**: Customer-Supplier
- **Direction**: Biometric Collection (upstream) supplies vital signs and anthropometric data to Fitness Assessment (downstream)
- **Integration**: Fitness Assessment queries biometric data when conducting assessments that require baseline measurements such as resting heart rate for VO2 max estimation

### Biometric Collection to Health Scoring

- **Pattern**: Published Language
- **Direction**: Biometric Collection (upstream) publishes standardized measurement events consumed by Health Scoring (downstream)
- **Integration**: Health Scoring receives measurement events through a published language of domain events with standardized payload schemas

### Body Composition to Health Scoring

- **Pattern**: Customer-Supplier
- **Direction**: Body Composition (upstream) provides composition profiles to Health Scoring (downstream)
- **Integration**: Health Scoring consumes CompositionProfileUpdated events to incorporate body composition data into composite health scores

### Fitness Assessment to Health Scoring

- **Pattern**: Customer-Supplier
- **Direction**: Fitness Assessment (upstream) provides assessment results to Health Scoring (downstream)
- **Integration**: Health Scoring consumes FitnessAssessmentCompleted events to incorporate physical performance data into wellness indices

### Health Scoring to Progress Tracking

- **Pattern**: Customer-Supplier
- **Direction**: Health Scoring (upstream) provides composite scores to Progress Tracking (downstream)
- **Integration**: Progress Tracking subscribes to HealthScoreCalculated events to update goal progress and detect milestone achievements

### Biometric Collection to Progress Tracking

- **Pattern**: Published Language
- **Direction**: Biometric Collection (upstream) publishes measurement events to Progress Tracking (downstream)
- **Integration**: Progress Tracking monitors measurement frequency for streak calculation and tracks individual metric trends

### Clinical Integration to External Systems

- **Pattern**: Anti-Corruption Layer
- **Direction**: Clinical Integration mediates between internal contexts and external EHR systems
- **Integration**: Clinical Integration translates internal domain events into FHIR resources and external clinical observations into internal domain concepts

### Clinical Integration to Biometric Collection

- **Pattern**: Open Host Service
- **Direction**: Biometric Collection exposes an open host service that Clinical Integration uses to submit externally sourced measurements
- **Integration**: Clinical measurements received through FHIR interfaces are translated and submitted to Biometric Collection through a standardized service interface

## Data Flow Summary

The overall data flow follows a pipeline pattern:

1. Biometric Collection captures raw measurements and publishes events
2. Body Composition and Fitness Assessment consume measurement data and produce analytical results
3. Health Scoring aggregates analytical results into composite metrics
4. Progress Tracking monitors all upstream events for goal and trend analysis
5. Clinical Integration translates between internal events and external clinical systems at all levels

## Shared Kernel

The contexts share a minimal kernel consisting of:

- **PersonId**: Universal identifier for the individual being measured
- **MeasurementSessionId**: Identifier linking related measurements captured together
- **Timestamp**: Temporal reference with timezone awareness
- **UnitOfMeasure**: Enumeration of supported measurement units

This shared kernel is deliberately minimal to reduce coupling between contexts.

## Organizational Patterns

Teams owning upstream contexts have a responsibility to maintain stable published interfaces. Downstream teams provide feedback on data quality and completeness requirements. The Health Scoring team, as the primary downstream consumer, participates in design reviews for upstream event schemas.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 14 on context maps.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 3 on context maps.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 8 on context mapping patterns.
