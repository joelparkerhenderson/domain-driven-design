# Ubiquitous Language - Asthma Domain

## Overview

Ubiquitous language is the shared vocabulary that domain experts, developers, and stakeholders use consistently in all communications -- conversations, documentation, code, and models. In the asthma management domain, the ubiquitous language bridges clinical terminology used by pulmonologists, allergists, and respiratory therapists with software modeling concepts used by developers and architects.

Establishing a rigorous ubiquitous language prevents the common failure mode where clinical terms are misinterpreted in software models, or where software abstractions diverge from clinical reality. Every term in the ubiquitous language must be precisely defined, mutually agreed upon, and used consistently across all bounded contexts.

## Language Development Process

The asthma domain ubiquitous language is developed through collaboration between:

- **Pulmonologists and allergists** who provide clinical assessment and treatment terminology.
- **Respiratory therapists** who contribute pulmonary function testing and inhaler technique vocabulary.
- **Pharmacists** who define medication management and adherence terminology.
- **Environmental health specialists** who supply trigger monitoring and exposure terminology.
- **Patients and patient advocates** who validate action planning and self-management language clarity.
- **Software developers and architects** who ensure terms map cleanly to domain model constructs.

The language development follows an iterative process: domain experts propose terms, developers test them against model constraints, and discrepancies are resolved through collaborative modeling sessions. This process is repeated as the domain model evolves.

## Language Boundaries

While the ubiquitous language is shared across the domain, certain terms have context-specific meanings:

**FEV1** in the Clinical Assessment Context is a measured value with associated test metadata (date, device calibration, patient effort grade). In the Outcomes Tracking Context, FEV1 is a data point in a longitudinal trend with statistical properties (mean, variance, trajectory slope).

**Medication** in the Treatment Management Context refers to a therapeutic agent with pharmacological properties and step assignment. In the Medication Management Context, medication refers to a prescribed item with dosing schedule, refill status, and adherence metrics.

**Zone** in the Action Planning Context is a patient-facing classification (green, yellow, red) with associated self-management instructions. In the Clinical Assessment Context, the equivalent concept is the GINA control level (well-controlled, partly controlled, uncontrolled), which uses different criteria and terminology.

These differences are intentional. Each bounded context maintains its own precise usage of shared terms, and translation occurs at context boundaries through anti-corruption layers or published languages.

## Key Term Categories

### Diagnostic Terms

Spirometry, peak expiratory flow (PEF), forced expiratory volume in one second (FEV1), forced vital capacity (FVC), FEV1/FVC ratio, fractional exhaled nitric oxide (FeNO), bronchial hyperresponsiveness, methacholine challenge, exercise challenge, reversibility testing.

### Classification Terms

GINA severity classification (intermittent, mild persistent, moderate persistent, severe persistent), asthma control level (well-controlled, partly controlled, uncontrolled), phenotype (allergic, exercise-induced, aspirin-exacerbated, late-onset), endotype (T2-high, T2-low, eosinophilic, neutrophilic).

### Treatment Terms

Stepwise therapy (steps 1-5), controller medication, rescue inhaler, inhaled corticosteroid (ICS), long-acting beta-agonist (LABA), short-acting beta-agonist (SABA), leukotriene receptor antagonist (LTRA), biologic agent (omalizumab, mepolizumab, benralizumab, dupilumab, tezepelumab), allergen immunotherapy.

### Monitoring Terms

Asthma Control Test (ACT), peak flow diary, trigger, allergen, air quality index (AQI), pollen count, occupational exposure, exacerbation, medication adherence, rescue inhaler usage frequency.

### Action Planning Terms

Asthma action plan, zone system, green zone, yellow zone, red zone, personal best PEF, zone threshold, emergency protocol, emergency contacts, shared decision-making.

### Outcome Terms

Exacerbation rate, lung function trend, quality of life score (AQLQ), healthcare utilization, emergency department visit, hospitalization, oral corticosteroid burst, symptom-free days.

## Language Governance

The ubiquitous language is maintained through:

1. **Glossary Document.** The ubiquitous-language.md file at the domain root serves as the authoritative reference.
2. **Model Alignment.** Code artifacts (class names, method names, variable names) must use ubiquitous language terms.
3. **Review Process.** Changes to the ubiquitous language require agreement between domain experts and development team.
4. **Context Annotations.** When a term has different meanings across bounded contexts, each usage is annotated with its context.

## Anti-Patterns to Avoid

- **Synonym Proliferation.** Using "attack," "episode," "flare-up," and "exacerbation" interchangeably. The ubiquitous language selects one term (exacerbation) and uses it consistently.
- **Jargon Without Definition.** Introducing clinical abbreviations (BHR, AHR) without explicit glossary entries.
- **Developer-Invented Terms.** Creating software terms (AsthmaEvent, LungReading) that have no clinical equivalent.
- **Overloaded Terms.** Using "assessment" to mean both a spirometry test session and a control level evaluation without distinguishing them.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 2 on ubiquitous language.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 1 on developing ubiquitous language.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 2 on discovering domain knowledge.
- Global Initiative for Asthma (GINA). Global Strategy for Asthma Management and Prevention, 2023. Glossary of terms.
