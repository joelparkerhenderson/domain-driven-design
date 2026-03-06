# Asthma Domain - Domain-Driven Design

## Introduction

This domain applies Domain-Driven Design (DDD) to asthma management systems. Asthma is a chronic respiratory condition affecting over 300 million people worldwide, characterized by airway inflammation, bronchial hyperresponsiveness, and variable airflow obstruction. Effective asthma management requires coordination across clinical assessment, treatment planning, trigger identification, medication adherence, and outcomes measurement.

DDD provides a structured approach to modeling this complexity by establishing a ubiquitous language shared between clinicians, respiratory therapists, patients, pharmacists, and software developers. The domain is decomposed into six bounded contexts that reflect real-world organizational and clinical boundaries.

## Bounded Contexts

1. **Clinical Assessment Context** - Manages spirometry testing, peak flow monitoring, FeNO measurement, and GINA-based severity classification. This context owns the diagnostic and assessment models that determine asthma control status.

2. **Treatment Management Context** - Handles stepwise therapy planning, biologic agent prescriptions, allergen immunotherapy protocols, and inhaler technique evaluation. Follows the GINA stepwise approach for treatment escalation and de-escalation.

3. **Trigger Monitoring Context** - Tracks allergen exposures, air quality indices, environmental factors, and occupational exposures. Provides data for correlating environmental conditions with symptom exacerbations.

4. **Action Planning Context** - Defines personalized asthma action plans using the zone system (green, yellow, red). Establishes self-management protocols and emergency response procedures.

5. **Medication Management Context** - Manages controller medication schedules, rescue inhaler usage, medication adherence monitoring, and prescription workflows. Integrates with pharmacy and EHR systems.

6. **Outcomes Tracking Context** - Measures clinical outcomes including ACT scores, exacerbation frequency, lung function trends, and quality of life assessments. Provides longitudinal analysis for treatment effectiveness evaluation.

## Topics

- [Strategic Design](topics/strategic-design/index.md)
- [Bounded Contexts](topics/bounded-contexts/index.md)
- [Context Map](topics/context-map/index.md)
- [Ubiquitous Language](topics/ubiquitous-language/index.md)
- [Subdomains](topics/subdomains/index.md)
- [Anti-Corruption Layer](topics/anti-corruption-layer/index.md)
- [Aggregates](topics/aggregates/index.md)
- [Entities](topics/entities/index.md)
- [Value Objects](topics/value-objects/index.md)
- [Domain Events](topics/domain-events/index.md)
- [Repositories](topics/repositories/index.md)
- [Domain Services](topics/domain-services/index.md)
- [Clinical Assessment Context](topics/clinical-assessment-context/index.md)
- [Treatment Management Context](topics/treatment-management-context/index.md)
- [Trigger Monitoring Context](topics/trigger-monitoring-context/index.md)
- [Action Planning Context](topics/action-planning-context/index.md)
- [Medication Management Context](topics/medication-management-context/index.md)
- [Outcomes Tracking Context](topics/outcomes-tracking-context/index.md)
- [Event-Driven Architecture](topics/event-driven-architecture/index.md)
- [Integration Patterns](topics/integration-patterns/index.md)
- [Asthma Standards](topics/asthma-standards/index.md)
- [Regulatory Compliance](topics/regulatory-compliance/index.md)

## Key Resources

- [Ubiquitous Language Glossary](ubiquitous-language.md)
- [Project Plan](plan.md)
- [Tasks](tasks.md)

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- Global Initiative for Asthma (GINA). Global Strategy for Asthma Management and Prevention, 2023.
- National Asthma Education and Prevention Program (NAEPP). Expert Panel Report 4, 2020.
