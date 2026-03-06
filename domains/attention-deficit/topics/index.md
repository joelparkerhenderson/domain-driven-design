# Attention-Deficit Domain: Topics

## Strategic Design

- [Strategic Design](strategic-design/) - High-level architectural decomposition of the ADHD management domain into bounded contexts, subdomains, and integration patterns.
- [Bounded Contexts](bounded-contexts/) - Six logical boundaries defining clinical assessment, treatment management, behavioral monitoring, educational accommodation, medication management, and outcomes tracking.
- [Context Map](context-map/) - Relationships and communication patterns between bounded contexts in the ADHD management ecosystem.
- [Ubiquitous Language](ubiquitous-language/) - Shared terminology drawn from clinical, educational, and therapeutic ADHD practice.
- [Subdomains](subdomains/) - Classification of ADHD management areas into core, supporting, and generic subdomains.
- [Anti-Corruption Layer](anti-corruption-layer/) - Translation boundaries protecting bounded contexts from external clinical and educational system concepts.

## Tactical Design

- [Aggregates](aggregates/) - Consistency boundaries grouping related ADHD management entities and value objects into transactional units.
- [Entities](entities/) - Domain objects with unique identity that persist across assessment sessions, treatment episodes, and monitoring periods.
- [Value Objects](value-objects/) - Immutable descriptors such as diagnostic codes, rating scale scores, dosage amounts, and accommodation types.
- [Domain Events](domain-events/) - Significant occurrences including assessment completions, treatment plan changes, symptom changes, and medication adjustments.
- [Repositories](repositories/) - Persistence abstractions for storing and retrieving ADHD management aggregate roots.
- [Domain Services](domain-services/) - Stateless operations spanning multiple aggregates such as treatment coordination and cross-context data analysis.

## Bounded Contexts

- [Clinical Assessment Context](clinical-assessment-context/) - Diagnostic evaluation using DSM-5 criteria, rating scales, and neuropsychological testing.
- [Treatment Management Context](treatment-management-context/) - Multimodal treatment planning including behavioral therapy, coaching, and parent training.
- [Behavioral Monitoring Context](behavioral-monitoring-context/) - Longitudinal symptom tracking, behavior charting, and executive function monitoring.
- [Educational Accommodation Context](educational-accommodation-context/) - IEP and 504 plan management, classroom strategies, and academic support coordination.
- [Medication Management Context](medication-management-context/) - Stimulant and non-stimulant protocols, titration, side effect monitoring, and adherence tracking.
- [Outcomes Tracking Context](outcomes-tracking-context/) - Treatment effectiveness measurement, functional improvement assessment, and quality of life evaluation.

## Cross-Cutting Concerns

- [Event-Driven Architecture](event-driven-architecture/) - Asynchronous communication between bounded contexts through domain events.
- [Integration Patterns](integration-patterns/) - Shared kernel, published language, and open host service patterns for ADHD system integration.
- [Attention-Deficit Standards](attention-deficit-standards/) - DSM-5, ICD-11, AAP clinical practice guidelines, and NICE guidelines informing domain model design.
- [Regulatory Compliance](regulatory-compliance/) - HIPAA, FERPA, IDEA, and ADA requirements shaping data handling and access control.
