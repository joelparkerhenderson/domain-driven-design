# Gastroenterology Domain - Domain-Driven Design

## Overview

This domain applies Domain-Driven Design (DDD) to gastroenterology clinical systems.
Gastroenterology encompasses the diagnosis and management of disorders affecting the
gastrointestinal tract, liver, biliary system, and pancreas. The domain model captures
clinical workflows for assessment, endoscopic procedures, inflammatory bowel disease,
hepatology, motility disorders, and nutrition management.

DDD provides the architectural framework to manage this clinical complexity by establishing
bounded contexts that mirror how gastroenterologists organize their practice, using
ubiquitous language drawn directly from clinical gastroenterology terminology, and
applying tactical patterns that enforce consistency within each clinical subdomain.

## Bounded Contexts

1. **Clinical Assessment Context** - Symptom evaluation, physical examination, laboratory
   studies, and imaging referrals. This context manages the initial and ongoing evaluation
   of patients presenting with gastrointestinal complaints.

2. **Endoscopic Procedures Context** - EGD, colonoscopy, ERCP, EUS, capsule endoscopy,
   and pathology tracking. This context handles the full lifecycle of endoscopic procedures
   from scheduling through pathology result integration.

3. **Inflammatory Disease Context** - IBD (Crohn's disease, ulcerative colitis) management,
   biologic therapy, and flare management. This context supports longitudinal management
   of chronic inflammatory conditions.

4. **Hepatology Context** - Liver disease management, hepatitis, cirrhosis staging, and
   transplant evaluation. This context tracks liver disease progression and manages the
   transplant candidacy workflow.

5. **Motility Disorders Context** - Manometry, pH monitoring, GERD management, and
   functional GI disorders. This context captures objective motility measurements and
   applies diagnostic criteria such as Rome IV.

6. **Nutrition Management Context** - Enteral and parenteral nutrition, celiac disease,
   and food intolerance. This context manages nutritional assessment, intervention
   planning, and dietary therapy adherence.

## Domain Files

- [CLAUDE.md](CLAUDE.md) - Domain conventions and instructions
- [plan.md](plan.md) - Vision, goals, and phasing
- [tasks.md](tasks.md) - Task tracking
- [ubiquitous-language.md](ubiquitous-language.md) - Glossary of domain terms
- [topics/](topics/) - Detailed topic documentation

## Topics

### Strategic Design

- [Strategic Design](topics/strategic-design/) - Overview of strategic DDD patterns
- [Bounded Contexts](topics/bounded-contexts/) - Context boundaries and responsibilities
- [Context Map](topics/context-map/) - Relationships between bounded contexts
- [Ubiquitous Language](topics/ubiquitous-language/) - Shared clinical terminology
- [Subdomains](topics/subdomains/) - Core, supporting, and generic classification
- [Anti-Corruption Layer](topics/anti-corruption-layer/) - External system translation

### Tactical Design

- [Aggregates](topics/aggregates/) - Consistency boundaries and invariants
- [Entities](topics/entities/) - Objects with unique identity
- [Value Objects](topics/value-objects/) - Immutable clinical measurements and codes
- [Domain Events](topics/domain-events/) - Clinically significant occurrences
- [Repositories](topics/repositories/) - Aggregate root persistence abstractions
- [Domain Services](topics/domain-services/) - Cross-aggregate operations

### Bounded Context Details

- [Clinical Assessment Context](topics/clinical-assessment-context/) - Patient evaluation
- [Endoscopic Procedures Context](topics/endoscopic-procedures-context/) - Procedural workflows
- [Inflammatory Disease Context](topics/inflammatory-disease-context/) - IBD management
- [Hepatology Context](topics/hepatology-context/) - Liver disease management
- [Motility Disorders Context](topics/motility-disorders-context/) - Motility and functional disorders
- [Nutrition Management Context](topics/nutrition-management-context/) - Nutrition and diet

### Cross-Cutting Concerns

- [Event-Driven Architecture](topics/event-driven-architecture/) - Async inter-context communication
- [Integration Patterns](topics/integration-patterns/) - External system connectivity
- [Gastroenterology Standards](topics/gastroenterology-standards/) - Clinical scoring and criteria
- [Regulatory Compliance](topics/regulatory-compliance/) - HIPAA, FDA, CMS requirements

## References

- Evans, E. (2003). *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media.
