# Domain Services in the Sales Domain

## Overview

Domain services are stateless operations that encapsulate domain logic which does not
naturally belong to any single entity or value object. In the Sales domain, domain services
handle operations that span multiple aggregates, require coordination between different parts
of the model, or implement business rules that involve concepts from across the bounded
context. Unlike application services that orchestrate use cases, domain services contain pure
business logic expressed in the ubiquitous language.

The Sales domain has several natural candidates for domain services. Lead scoring involves
evaluating multiple data points across a lead's profile and engagement history. Pipeline
forecasting aggregates data from many opportunities. Territory assignment requires rules that
span accounts and representatives. These operations are not the responsibility of any single
entity and are best modeled as domain services.

## Key Concepts

**Statelessness**: Domain services do not maintain state between invocations. They receive
inputs, apply business logic, and return results or emit domain events. A LeadScoringService,
for example, takes a lead's attributes and engagement data as inputs and returns a calculated
LeadScore value object.

**Domain Logic, Not Infrastructure**: Domain services contain business rules, not technical
infrastructure. A LeadScoringService implements the scoring algorithm defined by the business,
not the messaging infrastructure that delivers lead data. Infrastructure concerns belong in
application services or infrastructure layers.

**Named in Ubiquitous Language**: Domain service names use the ubiquitous language of their
bounded context. LeadQualificationService, PipelineForecastingService, and
TerritoryAssignmentService clearly communicate their business purpose.

**When to Use Domain Services**: Use a domain service when the operation involves multiple
aggregates, when the logic does not belong to any single entity, or when the operation
represents a domain concept in its own right (such as scoring, forecasting, or matching).

**Avoiding Anemic Models**: Domain services should complement, not replace, entity behavior.
If business logic naturally belongs to an entity (such as an Opportunity enforcing its own
stage transition rules), it should stay in the entity. Domain services are for operations
that genuinely span multiple objects.

## Sales Domain Services

**LeadScoringService** (Lead Generation Context): Calculates lead scores based on
demographic fit, firmographic data, and behavioral engagement signals. Applies configurable
scoring rules defined by the marketing and sales teams. Returns a LeadScore value object.

**LeadQualificationService** (Lead Generation Context): Evaluates a lead against
qualification criteria (such as BANT: Budget, Authority, Need, Timeline) to determine if
the lead is ready for sales engagement. Emits a LeadQualified or LeadDisqualified event.

**PipelineForecastingService** (Sales Pipeline Context): Generates revenue forecasts based
on pipeline data, historical conversion rates, and probability-weighted deal values.
Produces forecast reports for specified time periods.

**TerritoryAssignmentService** (Account Management Context): Assigns accounts and leads to
sales representatives based on territory rules including geography, industry, account size,
and workload balancing.

**CommissionCalculationService** (Opportunity Management Context): Calculates sales
representative commissions based on closed deal values, commission structures, quota
attainment, and accelerator tiers.

**RenewalRiskAssessmentService** (Account Management Context): Evaluates the likelihood of
contract renewal based on engagement metrics, support ticket history, usage patterns, and
relationship health indicators.

**DealScoringService** (Opportunity Management Context): Assigns a probability score to an
opportunity based on historical patterns, deal characteristics, and engagement signals.
Complements the static stage-based probability with a dynamic, data-driven assessment.

## Relationship to Other Topics

- [Aggregates](../aggregates/) are operated on by domain services spanning multiple aggregates.
- [Entities](../entities/) may delegate cross-entity logic to domain services.
- [Value Objects](../value-objects/) are frequently returned by domain services as results.
- [Repositories](../repositories/) are used by domain services to access aggregates.
- [Domain Events](../domain-events/) are emitted by domain services upon completion.
- [Bounded Contexts](../bounded-contexts/) contain the domain services for their logic.

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software."
  Addison-Wesley, 2003. Chapter 5: A Model Expressed in Software.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
  Chapter 7: Services.
- Khononov, Vlad. "Learning Domain-Driven Design." O'Reilly Media, 2021.
  Chapter 6: Tactical Design.
- Millett, Scott and Tune, Nick. "Patterns, Principles, and Practices of
  Domain-Driven Design." Wiley, 2015.
