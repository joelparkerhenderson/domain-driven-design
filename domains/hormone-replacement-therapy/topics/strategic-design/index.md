# Strategic Design in the Hormone Replacement Therapy Domain

Strategic design addresses how to decompose the hormone replacement therapy (HRT) domain into manageable bounded contexts, each with clearly defined responsibilities, relationships, and integration points. In DDD, strategic design precedes tactical modeling because the way a system is partitioned has greater impact on long-term success than any individual class or object design.

## Purpose

The HRT domain encompasses clinical assessment, pharmacological protocol management, laboratory monitoring, adverse event tracking, treatment optimization, and outcomes measurement. Without strategic decomposition, these concerns become entangled, producing a monolithic model where changes to lab monitoring logic ripple unpredictably into treatment protocols or clinical assessments. Strategic design prevents this by establishing explicit boundaries.

## Bounded Contexts

The HRT domain is decomposed into six bounded contexts, each owning a distinct portion of the domain model.

The Clinical Assessment Context handles patient intake, symptom scoring, hormone panel interpretation, and candidacy determination. It produces assessment artifacts consumed downstream by protocol selection.

The Hormone Protocol Context manages treatment regimen definitions, delivery method catalogs, dosing algorithms, and prescription generation. It translates clinical assessment outputs into actionable treatment plans.

The Lab Monitoring Context governs test ordering, result capture, trend analysis, and threshold-based alerting. It defines monitoring schedules based on protocol parameters and evaluates results against therapeutic target ranges.

The Side Effect Management Context tracks adverse events, maintains contraindication databases, performs risk stratification, and coordinates mitigation strategies across the treatment lifecycle.

The Treatment Optimization Context performs dose titration calculations, formulation adjustments, and combination therapy planning by synthesizing data from monitoring, side effect tracking, and outcomes measurement.

The Outcomes Tracking Context measures symptom resolution, quality of life scores, and long-term health outcomes to evaluate treatment efficacy and inform evidence-based practice improvements.

## Context Map

The context map for the HRT domain reveals several key relationships. Clinical Assessment acts as an upstream context that supplies patient evaluations to the Hormone Protocol Context through a customer-supplier relationship. Lab Monitoring operates as a conformist to the Hormone Protocol Context, accepting protocol-defined monitoring schedules. Side Effect Management consumes events from both Lab Monitoring and Clinical Assessment. Treatment Optimization integrates data from Lab Monitoring, Side Effect Management, and Outcomes Tracking through an open host service pattern. Outcomes Tracking acts as a downstream consumer of data from all other contexts.

## Subdomain Classification

The Hormone Protocol Context and Treatment Optimization Context constitute the core subdomains because they embody the unique clinical decision-making that differentiates HRT systems. Clinical Assessment and Side Effect Management are supporting subdomains that provide essential but more standardized functionality. Lab Monitoring and Outcomes Tracking are closer to generic subdomains, as their patterns appear across many medical domains, though they require HRT-specific configuration.

## Anti-Corruption Layers

Anti-corruption layers are positioned between contexts that have different modeling philosophies. The Treatment Optimization Context maintains an ACL against Lab Monitoring to translate raw lab result formats into optimization-specific representations. The Side Effect Management Context maintains an ACL against external pharmacovigilance systems to prevent external adverse event taxonomies from contaminating the internal domain model.

## Design Principles

Strategic design in the HRT domain follows several guiding principles. Each bounded context maintains autonomy over its domain model, meaning that an entity like "Patient" may have different representations across contexts. Shared concepts are communicated through published language and domain events rather than shared databases. Integration occurs through well-defined contracts, and each context can evolve independently as clinical knowledge advances.

## Alignment with Clinical Practice

The bounded context boundaries align with how clinical teams naturally organize HRT care. Intake coordinators perform clinical assessments. Prescribing clinicians manage protocols. Laboratory departments handle monitoring. Pharmacovigilance teams track side effects. Clinical pharmacists optimize treatments. Quality teams measure outcomes. This alignment ensures that the domain model reflects real-world organizational boundaries, a key DDD principle.

## Benefits

Strategic design provides several concrete benefits in the HRT domain. It enables specialized teams to own individual contexts without requiring global coordination. It allows each context to adopt the modeling granularity appropriate to its complexity. It creates natural seams for testing, deployment, and scaling. It facilitates regulatory compliance by isolating auditable workflows within well-defined boundaries.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapters 14-17 on strategic design.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapters 2-3 on domains, subdomains, and bounded contexts.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Part II on strategic design.
- The Endocrine Society. Clinical Practice Guidelines for Hormone Therapy.
- North American Menopause Society. Position Statement on Hormone Therapy, 2022.
