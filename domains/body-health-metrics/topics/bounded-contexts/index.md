# Bounded Contexts for Body Health Metrics

## Overview

Bounded contexts define logical boundaries within the body health metrics domain where specific models and terminology remain internally consistent. Each bounded context encapsulates a coherent area of body health measurement with its own domain model, ubiquitous language subset, and consistency rules. The boundaries are drawn along natural divisions in health measurement science, ensuring that each context can evolve independently while maintaining clear integration interfaces.

## Context Definitions

### Biometric Collection Context

This context owns the fundamental act of recording body measurements. It manages measurement sessions, validates data against physiological plausibility constraints, and standardizes units. Key concepts include measurement session, measurement device, raw reading, and calibration status. A "weight" in this context is a raw recorded value with timestamp, device reference, and measurement conditions.

### Body Composition Context

This context interprets raw biometric data to produce body composition profiles. It applies analytical models to derive lean mass, fat mass, bone mineral density, hydration levels, and metabolic rate estimates. A "weight" in this context decomposes into fat mass and lean mass components. The same term carries different semantics across the context boundary.

### Fitness Assessment Context

This context evaluates physical performance capacity through standardized assessment protocols. It manages test administration, normative databases, and performance classification. Concepts include assessment protocol, performance result, normative percentile, and fitness classification. A VO2 max measurement in this context includes the test protocol used, environmental conditions, and effort verification criteria.

### Health Scoring Context

This context synthesizes measurements from multiple upstream contexts into composite health metrics. It applies scoring algorithms, age-sex normalization, and population-based comparisons. Concepts include composite score, wellness index, scoring algorithm, and normative reference. This context transforms raw measurements into actionable health assessments.

### Progress Tracking Context

This context manages the longitudinal dimension of health data. It owns goal definitions, trend calculations, milestone recognition, and streak maintenance. Concepts include health goal, progress trend, milestone achievement, and measurement streak. A "weight" in this context is a trend line, not a single value.

### Clinical Integration Context

This context bridges internal domain models with external clinical systems. It manages data format translation, clinical coding, and regulatory compliance. Concepts include clinical observation, FHIR resource, LOINC code, and clinical report. This context speaks the language of clinical informatics rather than body health measurement.

## Boundary Characteristics

Each bounded context boundary exhibits specific characteristics:

- **Linguistic boundary**: The same term may have different meanings across contexts. "Measurement" in the Biometric Collection Context refers to a raw data capture event; in the Health Scoring Context it refers to a normalized input to a scoring algorithm.

- **Model boundary**: Each context maintains its own domain model. The Body Composition Context models a CompositionProfile aggregate with fat mass and lean mass components that do not exist in the Biometric Collection Context.

- **Consistency boundary**: Each context enforces its own invariants. The Biometric Collection Context validates physiological plausibility; the Health Scoring Context validates score range constraints.

- **Lifecycle boundary**: Contexts evolve independently. Adding a new fitness assessment protocol does not require changes to the body composition model.

## Context Size and Scope

The contexts are sized to balance cohesion with manageability. Each context is large enough to contain a meaningful subdomain of body health measurement but small enough for a single team to own and maintain. The Biometric Collection Context is the most constrained, focusing narrowly on data capture. The Health Scoring Context is the most integrative, consuming data from multiple upstream contexts.

## Team Alignment

Each bounded context aligns with a team that includes domain expertise relevant to that area. The Biometric Collection team includes measurement science specialists. The Body Composition team includes exercise physiologists. The Clinical Integration team includes clinical informatics specialists. This alignment ensures that the ubiquitous language within each context reflects genuine domain expertise.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 14 on maintaining model integrity.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 2 on bounded contexts.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 6 on discovering bounded context boundaries.
