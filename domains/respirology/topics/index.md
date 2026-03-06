# Respirology Domain - Topics

## Strategic Design

- [Strategic Design](strategic-design/) - Overview of strategic DDD patterns applied to the respirology domain
- [Bounded Contexts](bounded-contexts/) - The six bounded contexts decomposing the respirology domain
- [Context Map](context-map/) - Relationships and integration patterns between bounded contexts
- [Ubiquitous Language](ubiquitous-language/) - Shared terminology for respirology domain modeling
- [Subdomains](subdomains/) - Classification of domain areas by strategic importance
- [Anti-Corruption Layer](anti-corruption-layer/) - Translation boundaries protecting bounded contexts

## Tactical Design

- [Aggregates](aggregates/) - Consistency boundaries and aggregate roots in the respirology domain
- [Entities](entities/) - Objects with unique identity persisting over time
- [Value Objects](value-objects/) - Immutable objects defined by their attributes
- [Domain Events](domain-events/) - Significant occurrences triggering cross-context actions
- [Repositories](repositories/) - Abstractions for storing and retrieving aggregate roots
- [Domain Services](domain-services/) - Stateless operations spanning multiple aggregates

## Bounded Context Deep Dives

- [Clinical Assessment Context](clinical-assessment-context/) - Respiratory examination, history taking, symptom scoring, risk stratification
- [Respiratory Diagnostics Context](respiratory-diagnostics-context/) - Pulmonary function tests, imaging, bronchoscopy, sputum analysis
- [Airway Management Context](airway-management-context/) - Intubation, tracheostomy, airway clearance, bronchodilator therapy
- [Chronic Disease Management Context](chronic-disease-management-context/) - COPD action plans, ILD monitoring, bronchiectasis, sarcoidosis
- [Ventilatory Support Context](ventilatory-support-context/) - NIV, mechanical ventilation, high-flow therapy, home ventilation
- [Pulmonary Rehabilitation Context](pulmonary-rehabilitation-context/) - Exercise programs, breathing retraining, patient education, discharge planning

## Cross-Cutting Concerns

- [Event-Driven Architecture](event-driven-architecture/) - Communication between bounded contexts through domain events
- [Integration Patterns](integration-patterns/) - Shared kernel, published language, open host service patterns
- [Respirology Standards](respirology-standards/) - ATS/ERS, GOLD, BTS, and other clinical standards
- [Regulatory Compliance](regulatory-compliance/) - HIPAA, FDA, clinical documentation, and data governance requirements
