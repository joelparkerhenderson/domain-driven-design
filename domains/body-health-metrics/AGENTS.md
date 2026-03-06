# Body Health Metrics Domain

This domain applies Domain-Driven Design to body health measurement systems,
including biometric data collection, body composition analysis, fitness
assessment, health scoring, progress tracking, and clinical integration.

## Bounded Contexts

1. Biometric Collection Context - weight, height, BMI, body fat %, waist circumference, vital signs
2. Body Composition Context - lean mass, fat mass, bone density, hydration levels, metabolic rate
3. Fitness Assessment Context - VO2 max, strength testing, flexibility, endurance benchmarks
4. Health Scoring Context - composite health scores, wellness indices, age-adjusted metrics
5. Progress Tracking Context - goal setting, trend analysis, milestone tracking, streak management
6. Clinical Integration Context - EHR integration, physician reporting, FHIR data exchange

## Guidelines

- Use ubiquitous language from body health, fitness, and clinical measurement domains.
- Model business logic as pure domain logic; no infrastructure concerns.
- Apply CQRS to separate measurement recording from metric querying.
- All value objects must carry units of measurement and measurement precision.
- Aggregates enforce physiological plausibility constraints.
- Domain events communicate measurement changes across bounded contexts.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. 2013.
- Khononov, Vlad. Learning Domain-Driven Design. 2021.
