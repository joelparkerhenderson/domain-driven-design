# Nutrition Domain - Topics

## Strategic Design

- [Strategic Design](strategic-design/) - Overview of strategic DDD patterns applied to the nutrition domain
- [Bounded Contexts](bounded-contexts/) - The six bounded contexts and their responsibilities
- [Context Map](context-map/) - Relationships and integration points between contexts
- [Ubiquitous Language](ubiquitous-language/) - Shared terminology across domain experts and developers
- [Subdomains](subdomains/) - Classification of domain areas by strategic importance
- [Anti-Corruption Layer](anti-corruption-layer/) - Translation boundaries protecting contexts from external systems

## Tactical Design

- [Aggregates](aggregates/) - Consistency boundaries and transactional units within each context
- [Entities](entities/) - Objects with unique identity and lifecycle in the nutrition domain
- [Value Objects](value-objects/) - Immutable objects defined by their attributes
- [Domain Events](domain-events/) - Significant occurrences that trigger cross-context actions
- [Repositories](repositories/) - Abstractions for storing and retrieving aggregate roots
- [Domain Services](domain-services/) - Stateless operations spanning multiple aggregates

## Bounded Context Details

- [Nutritional Assessment Context](nutritional-assessment-context/) - Dietary recall, anthropometry, biomarkers, nutrient analysis
- [Meal Planning Context](meal-planning-context/) - Meal design, recipes, grocery lists, nutrient targets
- [Clinical Nutrition Context](clinical-nutrition-context/) - Medical nutrition therapy, enteral/parenteral nutrition, disease-specific diets
- [Sports Nutrition Context](sports-nutrition-context/) - Performance nutrition, hydration, supplementation, periodized fueling
- [Dietary Compliance Context](dietary-compliance-context/) - Food logging, adherence tracking, behavioral coaching
- [Outcomes Tracking Context](outcomes-tracking-context/) - Weight management, biomarker trends, body composition, patient-reported outcomes

## Cross-Cutting Concerns

- [Event-Driven Architecture](event-driven-architecture/) - Inter-context communication through domain events
- [Integration Patterns](integration-patterns/) - Shared kernel, published language, open host service patterns
- [Nutrition Standards](nutrition-standards/) - DRIs, food composition databases, clinical terminology standards
- [Regulatory Compliance](regulatory-compliance/) - HIPAA, FDA, USDA, and other regulatory requirements
