# Sleep Health Metrics - Domain Driven Design

This domain applies Domain-Driven Design to sleep health monitoring,
covering sleep data collection, sleep stage analysis, sleep quality
assessment, circadian rhythm tracking, intervention tracking, and
clinical integration.

## Bounded Contexts

1. Sleep Data Collection Context
2. Sleep Stage Analysis Context
3. Sleep Quality Assessment Context
4. Circadian Rhythm Context
5. Intervention Tracking Context
6. Clinical Integration Context

## Guidelines

- Use ubiquitous language from sleep medicine throughout all documentation.
- Model business logic independently of infrastructure (no database, UI, or API details).
- Follow strategic and tactical DDD patterns from Evans (2003) and Vernon (2013).
- Maintain bounded context boundaries; use anti-corruption layers for external integrations.
- Apply CQRS where read and write models diverge (e.g., sleep study reporting vs. data ingestion).
- Align with clinical standards: AASM scoring manual, ICSD-3, FHIR R4.
- Reference regulatory requirements: HIPAA, FDA device classification, GDPR for health data.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software.
- Vernon, V. (2013). Implementing Domain-Driven Design.
- Khononov, V. (2021). Learning Domain-Driven Design.
