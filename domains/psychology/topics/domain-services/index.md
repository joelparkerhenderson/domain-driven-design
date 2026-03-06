# Domain Services in the Psychology Domain

## Overview

A domain service is a stateless operation that represents a significant domain concept but does not naturally belong to any single entity or value object. Domain services encapsulate business logic that spans multiple aggregates or requires coordination across domain objects. In the psychology domain, domain services handle operations like test scoring algorithms, treatment matching, statistical calculations, and cross-context clinical workflows.

Domain services are distinct from application services and infrastructure services. A domain service contains pure domain logic expressed in ubiquitous language. It does not manage transactions, handle persistence, or coordinate user interface interactions. It operates on domain objects and produces domain results.

## Scoring Service

The Scoring Service in the Psychological Assessment Context implements the algorithms that transform raw test responses into standardized scores. This service does not belong to any single entity because scoring requires knowledge of the test instrument's scoring rules, the applicable normative data, and the examinee's demographic characteristics for norm group selection.

The Scoring Service accepts raw responses and an assessment instrument identifier, applies the correct scoring algorithm, selects the appropriate normative table based on the examinee's age, education, and other demographic factors, and produces a set of Test Score value objects with associated normative comparisons. The scoring logic is complex, instrument-specific, and updated when new test editions or normative studies are published.

## Test Selection Service

The Test Selection Service recommends appropriate test batteries based on the referral question, the client's characteristics, and the clinical context. This service spans the Assessment Instrument entity and the Assessment aggregate, drawing on domain knowledge about which tests are appropriate for specific referral questions, age ranges, language capabilities, and clinical presentations.

The service considers factors such as test validity for the client's demographic group, the availability of relevant normative data, the time required for administration, and the incremental validity of adding tests to the battery. This clinical decision support logic is too complex and cross-cutting to reside within any single aggregate.

## Treatment Matching Service

The Treatment Matching Service in the Therapeutic Intervention Context matches clients to evidence-based treatments based on diagnostic formulation, symptom presentation, client preferences, and available evidence. This service consumes assessment results (through the anti-corruption layer) and applies treatment selection algorithms informed by clinical practice guidelines.

The service implements logic such as: clients with PTSD and no substance use comorbidity are matched to EMDR or Prolonged Exposure; clients with depression and anxiety comorbidity are matched to transdiagnostic CBT protocols; and treatment selection accounts for client readiness for change, therapeutic alliance considerations, and prior treatment response.

## Statistical Analysis Service

The Statistical Analysis Service in the Research Methods Context performs statistical calculations that span multiple domain objects. It computes effect sizes from pre-post data, determines statistical significance, conducts power analyses for study design, and performs meta-analytic calculations when combining results across studies.

This service operates on collections of data points from Participant entities and produces Effect Size, Statistical Test Result, and Power Analysis value objects. The statistical algorithms are domain logic because they implement the specific analytical methods approved in the research protocol, not generic mathematical operations.

## Reliable Change Calculation Service

The Reliable Change Calculation Service in the Outcomes Measurement Context determines whether observed change in outcome scores represents reliable improvement, reliable deterioration, or no reliable change. This service requires the test's reliability coefficient (from the assessment context, via ACL), the standard deviation of the normative group, and the client's pre and post scores.

The calculation follows the Jacobson and Truax formula: RCI = (post - pre) / (SD * sqrt(2) * sqrt(1 - reliability)). The service produces Reliable Change Index value objects that inform clinical decision-making about treatment continuation, modification, or termination.

## Benchmark Comparison Service

The Benchmark Comparison Service compares individual or program-level outcomes against established benchmarks. It accepts outcome data and a benchmark dataset, performs the comparison calculations, and produces a Benchmark Report value object that indicates whether performance meets, exceeds, or falls below expected standards.

This service spans the Outcome Record aggregate and external benchmark data, making it a natural domain service rather than a method on any single entity.

## Functional Analysis Service

The Functional Analysis Service in the Behavioral Analysis Context analyzes patterns in behavioral observation data to identify functional relationships between antecedents, behaviors, and consequences. This service processes collections of Antecedent-Behavior-Consequence records and produces hypothesized behavior functions (attention, escape, access to tangibles, automatic reinforcement).

The analysis requires pattern recognition across multiple data points and multiple observation sessions, spanning the Behavior Plan aggregate's internal data collection records. The resulting functional hypothesis informs the selection of intervention strategies.

## Cross-Context Referral Service

The Referral Service coordinates the handoff of clients between bounded contexts. When a therapeutic intervention reveals the need for cognitive assessment, the Referral Service creates the appropriate referral, translates relevant clinical information through the anti-corruption layer, and raises domain events that the receiving context can act upon.

## Service Design Principles

Domain services in the psychology domain should be named using verbs or verb phrases from the ubiquitous language: ScoreAssessment, MatchTreatment, CalculateReliableChange. They should accept and return domain objects, not primitive types. They should contain no state between invocations. And they should implement logic that domain experts recognize as a coherent clinical or research operation.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley. Chapter 5 on domain services.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley. Chapter 7 on domain service design.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media. Chapter 7 on domain services.
- Jacobson, N. S., & Truax, P. (1991). Clinical significance: A statistical approach. Journal of Consulting and Clinical Psychology, 59(1), 12-19.
