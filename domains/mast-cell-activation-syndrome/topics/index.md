# Mast Cell Activation Syndrome (MCAS) Domain - Topics

## Strategic Design Patterns

- [Strategic Design](strategic-design/index.md) - How to decompose the MCAS management system into manageable bounded contexts.
- [Bounded Contexts](bounded-contexts/index.md) - Logical boundaries where specific MCAS models and terminologies are consistent.
- [Context Map](context-map/index.md) - Relationships and interactions between the six MCAS bounded contexts.
- [Ubiquitous Language](ubiquitous-language/index.md) - Shared terminology bridging clinical MCAS expertise and software development.
- [Subdomains](subdomains/index.md) - Classification of MCAS domain areas by strategic importance.
- [Anti-Corruption Layer](anti-corruption-layer/index.md) - Translation boundaries protecting MCAS contexts from external system concepts.

## Tactical Design Patterns

- [Aggregates](aggregates/index.md) - Clusters of related MCAS objects treated as single consistency units.
- [Entities](entities/index.md) - MCAS objects with unique identities persisting over time.
- [Value Objects](value-objects/index.md) - Immutable MCAS objects defined by their attributes rather than identity.
- [Domain Events](domain-events/index.md) - Significant occurrences within the MCAS domain triggering cross-context actions.
- [Repositories](repositories/index.md) - Abstractions for storing and retrieving MCAS aggregate roots.
- [Domain Services](domain-services/index.md) - Stateless operations spanning multiple MCAS aggregates.

## Bounded Context Documentation

- [Diagnostic Assessment Context](diagnostic-assessment-context/index.md) - Tryptase levels, histamine metabolites, prostaglandin D2, bone marrow biopsy, consensus criteria.
- [Trigger Management Context](trigger-management-context/index.md) - Trigger identification, avoidance strategies, environmental controls, dietary management.
- [Medication Protocol Context](medication-protocol-context/index.md) - H1/H2 antihistamines, mast cell stabilizers, leukotriene inhibitors, compounding.
- [Symptom Tracking Context](symptom-tracking-context/index.md) - Multi-system symptom logging, flare documentation, severity scoring, pattern analysis.
- [Specialist Coordination Context](specialist-coordination-context/index.md) - Allergist, immunologist, gastroenterologist referrals, shared care plans.
- [Outcomes Measurement Context](outcomes-measurement-context/index.md) - Symptom burden scores, quality of life, medication effectiveness, functional status.

## Cross-Cutting Concerns

- [Event-Driven Architecture](event-driven-architecture/index.md) - Communication between MCAS bounded contexts through domain events.
- [Integration Patterns](integration-patterns/index.md) - Shared kernel, published language, open host service, customer-supplier patterns.
- [MCAS Standards](mast-cell-activation-syndrome-standards/index.md) - Clinical standards and guidelines informing domain model design.
- [Regulatory Compliance](regulatory-compliance/index.md) - HIPAA, HL7 FHIR, and healthcare governance requirements.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
