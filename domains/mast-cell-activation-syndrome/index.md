# Mast Cell Activation Syndrome (MCAS) Domain

## Overview

This domain applies Domain-Driven Design (DDD) to Mast Cell Activation Syndrome management systems. MCAS is a complex multi-system condition characterized by episodic inappropriate mast cell mediator release, requiring coordinated management across diagnostic assessment, trigger identification, medication optimization, symptom monitoring, specialist coordination, and outcomes measurement.

The DDD approach structures the MCAS management domain into six bounded contexts, each representing a distinct area of clinical responsibility with its own model, rules, and ubiquitous language.

## Bounded Contexts

1. **Diagnostic Assessment Context** - Manages tryptase levels, histamine metabolites, prostaglandin D2 measurements, bone marrow biopsy results, and consensus criteria evaluation.

2. **Trigger Management Context** - Handles trigger identification, avoidance strategies, environmental controls, and dietary management protocols.

3. **Medication Protocol Context** - Governs H1/H2 antihistamines, mast cell stabilizers, leukotriene inhibitors, and compounded medication formulations.

4. **Symptom Tracking Context** - Captures multi-system symptom logging, flare documentation, severity scoring, and pattern analysis.

5. **Specialist Coordination Context** - Coordinates allergist, immunologist, gastroenterologist referrals, and shared care plans.

6. **Outcomes Measurement Context** - Tracks symptom burden scores, quality of life assessments, medication effectiveness, and functional status.

## Strategic Design

The domain is decomposed into core, supporting, and generic subdomains. The Diagnostic Assessment Context serves as the core subdomain, providing the diagnostic foundation upon which all other contexts depend. Trigger Management, Medication Protocol, and Symptom Tracking function as supporting subdomains. Specialist Coordination and Outcomes Measurement serve as additional supporting subdomains that integrate across the system.

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
- [Diagnostic Assessment Context](topics/diagnostic-assessment-context/index.md)
- [Trigger Management Context](topics/trigger-management-context/index.md)
- [Medication Protocol Context](topics/medication-protocol-context/index.md)
- [Symptom Tracking Context](topics/symptom-tracking-context/index.md)
- [Specialist Coordination Context](topics/specialist-coordination-context/index.md)
- [Outcomes Measurement Context](topics/outcomes-measurement-context/index.md)
- [Event-Driven Architecture](topics/event-driven-architecture/index.md)
- [Integration Patterns](topics/integration-patterns/index.md)
- [MCAS Standards](topics/mast-cell-activation-syndrome-standards/index.md)
- [Regulatory Compliance](topics/regulatory-compliance/index.md)

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- Valent, P. et al. "Proposed Diagnostic Algorithm for Patients with Suspected Mast Cell Activation Syndrome." Journal of Allergy and Clinical Immunology: In Practice, 2019.
- Afrin, L.B. Never Bet Against Occam: Mast Cell Activation Disease and the Modern Epidemics of Chronic Illness and Medical Complexity. Sisters Media, 2016.
