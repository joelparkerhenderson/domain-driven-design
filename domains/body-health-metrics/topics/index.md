# Body Health Metrics Domain - Topics

## Strategic Design

- [Strategic Design](strategic-design/index.md) - high-level architectural approach to decomposing body health measurement systems into bounded contexts
- [Bounded Contexts](bounded-contexts/index.md) - definitions and boundaries for each measurement domain area
- [Context Map](context-map/index.md) - relationships and integration patterns between body health contexts
- [Ubiquitous Language](ubiquitous-language/index.md) - shared terminology across domain experts, clinicians, and developers
- [Subdomains](subdomains/index.md) - classification of body health areas as core, supporting, or generic
- [Anti-Corruption Layer](anti-corruption-layer/index.md) - protective boundaries for external clinical system integration

## Tactical Design

- [Aggregates](aggregates/index.md) - consistency boundaries for measurement sessions, composition profiles, and assessments
- [Entities](entities/index.md) - identity-bearing objects such as individuals, measurement devices, and assessment protocols
- [Value Objects](value-objects/index.md) - immutable types for measurements, units, ranges, and scores
- [Domain Events](domain-events/index.md) - significant occurrences such as measurements recorded, thresholds crossed, and goals achieved
- [Repositories](repositories/index.md) - persistence abstractions for measurement aggregates
- [Domain Services](domain-services/index.md) - cross-aggregate operations such as BMI calculation and composite scoring

## Bounded Contexts

- [Biometric Collection Context](biometric-collection-context/index.md) - weight, height, BMI, body fat, waist circumference, vital signs
- [Body Composition Context](body-composition-context/index.md) - lean mass, fat mass, bone density, hydration, metabolic rate
- [Fitness Assessment Context](fitness-assessment-context/index.md) - VO2 max, strength testing, flexibility, endurance benchmarks
- [Health Scoring Context](health-scoring-context/index.md) - composite scores, wellness indices, age-adjusted metrics
- [Progress Tracking Context](progress-tracking-context/index.md) - goal setting, trend analysis, milestones, streaks
- [Clinical Integration Context](clinical-integration-context/index.md) - EHR integration, physician reporting, FHIR data exchange

## Cross-Cutting Concerns

- [Event-Driven Architecture](event-driven-architecture/index.md) - asynchronous communication between body health contexts
- [Integration Patterns](integration-patterns/index.md) - shared kernel, published language, and open host service patterns
- [Body Health Metrics Standards](body-health-metrics-standards/index.md) - LOINC, SNOMED CT, FHIR, and clinical measurement standards
- [Regulatory Compliance](regulatory-compliance/index.md) - HIPAA, GDPR, and clinical data governance requirements
