# Sales Pipeline Context

## Overview

The Sales Pipeline Context is responsible for tracking opportunity stage progression,
generating revenue forecasts, and delivering pipeline analytics. This bounded context
provides sales leadership with visibility into the health, velocity, and trajectory of the
sales process. While the Opportunity Management Context owns individual deal progression,
the Sales Pipeline Context aggregates this data across all opportunities to provide a
holistic view of the pipeline.

The Sales Pipeline Context is classified as a supporting subdomain. It follows
well-understood stage-gate methodologies and forecasting techniques, but the specific
configuration of stages, probability weightings, and forecasting models may be tailored
to the organization's sales process.

## Key Concepts

**Pipeline Stages**: A defined sequence of phases that opportunities progress through. Each
stage represents a milestone in the buyer's journey, from initial qualification through
discovery, evaluation, proposal, negotiation, and closure. Stages have associated
probability percentages that represent the likelihood of closing from that stage.

**Stage Conversion Rates**: The percentage of opportunities that successfully advance from
one stage to the next. Conversion rates are tracked over time and segmented by
representative, territory, product line, and deal size to identify patterns and areas
for improvement.

**Pipeline Forecasting**: The process of predicting future revenue based on current pipeline
composition. Forecasting methods include:

- Weighted pipeline: sum of deal values multiplied by stage probabilities.
- Historical conversion analysis: applying historical win rates to current pipeline.
- Representative commit and best-case estimates: subjective assessments from the field.
- Trend-based forecasting: extrapolating from historical revenue patterns.

**Sales Velocity**: A composite metric calculated as (Number of Opportunities x Average Deal
Value x Win Rate) / Average Sales Cycle Length. Sales velocity measures how quickly the
pipeline generates revenue and is used to assess pipeline health.

**Pipeline Coverage**: The ratio of total pipeline value to the revenue target (quota) for
a given period. A coverage ratio of 3x means the pipeline contains three times the quota
target, providing a buffer against expected losses. Industry benchmarks suggest 3x to 4x
coverage for healthy pipelines.

**Pipeline Review**: A regular cadence of reviewing pipeline data with sales teams to assess
deal quality, identify stuck opportunities, and validate forecasts. Pipeline reviews use
analytics from this context to drive coaching conversations and deal strategy.

**Pipeline Hygiene**: The practice of maintaining accurate and up-to-date pipeline data by
removing stale opportunities, updating close dates, and ensuring stage assignments reflect
reality. Poor pipeline hygiene degrades forecast accuracy.

## Aggregates and Entities

- **PipelineView** (Aggregate Root): A snapshot of the pipeline at a point in time,
  segmented by stage, representative, territory, or time period.

- **Forecast** (Aggregate Root): A revenue prediction for a specific period with weighted,
  commit, and best-case projections.

- **StageDefinition** (Value Object): The name, sequence position, probability, and criteria
  for a pipeline stage.

- **SalesVelocity** (Value Object): The calculated velocity metric for a given segment and
  time period.

- **PipelineCoverage** (Value Object): The ratio of pipeline value to quota for a segment.

## Domain Events

- PipelineStageUpdated: The pipeline view has been recalculated after opportunity changes.
- ForecastGenerated: A new forecast has been produced for a specific period.
- PipelineReviewCompleted: A scheduled pipeline review has been conducted.
- VelocityAlertTriggered: Sales velocity has dropped below a defined threshold.
- CoverageAlertTriggered: Pipeline coverage has fallen below acceptable levels.

## Context Relationships

- **Upstream from Opportunity Management**: Consumes OpportunityStageChanged, DealClosedWon,
  and DealClosedLost events to update pipeline views and forecasts.

- **Downstream to Sales Analytics**: Provides pipeline metrics and forecasts to the Sales
  Analytics Context for inclusion in comprehensive dashboards.

- **Shared Kernel with Opportunity Management**: Stage definitions and probability weightings
  are shared between the two contexts.

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software."
  Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." O'Reilly Media, 2021.
- Jordan, Jason and Vazzana, Michelle. "Cracking the Sales Management Code."
  McGraw-Hill, 2011.
- Miller, Robert and Heiman, Stephen. "The New Strategic Selling."
  Grand Central Publishing, 2005.
