# Key Result Tracking Context

## Overview

The Key Result Tracking Context is the bounded context responsible for measuring, tracking, and scoring key results throughout an OKR cycle. While the Objective Setting Context defines what will be measured, this context manages the ongoing process of recording progress, assessing confidence, calculating scores, and integrating with external performance data sources such as KPIs and KRIs.

This context operates during the Active and Scoring phases of an OKR cycle, handling the rhythmic check-ins that keep teams accountable and informed.

## Responsibilities

### Progress Tracking

Progress tracking captures the evolving measurement of each Key Result over time. The context maintains a chronological series of ProgressUpdate value objects for each Key Result, forming a progress history that enables trend analysis and forecasting.

Progress can be captured in two ways:

- **Manual reporting**: Owners submit progress updates during check-in sessions, recording the current value along with a narrative explanation.
- **Automated ingestion**: The context integrates with external data sources (analytics platforms, CRM systems, financial tools) to automatically pull current values for Key Results with automated data sources.

Each ProgressUpdate is immutable once recorded, preserving the historical record of how progress evolved throughout the cycle.

### Check-Ins

Check-ins are the primary mechanism for regular progress assessment. During each check-in, the Key Result owner provides:

- **Current value**: The latest measurement against the Key Result's metric.
- **Confidence level**: A qualitative assessment (High, Medium, Low) of the likelihood of achieving the target by cycle end.
- **Narrative**: Contextual explanation of progress, blockers, and planned next steps.
- **Blockers**: Specific impediments that may prevent target achievement.

Check-in frequency is governed by the Cadence value object defined in the Review & Cadence Context (typically weekly or biweekly). The check-in process follows the OODA (Observe, Orient, Decide, Act) loop: observe current measurements, orient by comparing to targets, decide on corrective actions, and act on those decisions before the next check-in.

### Confidence Levels

Confidence levels serve as a leading indicator of OKR health. Unlike scores (which are lagging indicators based on actual measurements), confidence reflects the owner's forward-looking assessment:

- **High**: Current trajectory leads to target achievement. No intervention needed.
- **Medium**: Achievement is possible but at risk. The team should discuss mitigation strategies.
- **Low**: Target achievement is unlikely without significant intervention or scope change.

Confidence trends are tracked over time. A pattern of declining confidence is a strong signal for management attention, even if the current score appears adequate.

### Scoring

The scoring process translates raw Key Result measurements into the standardized 0.0-to-1.0 scale:

- **Percentage-based**: `Score = (currentValue - baseline) / (target - baseline)`, clamped to 0.0-1.0.
- **Absolute-based**: Same formula applied to absolute numbers.
- **Binary**: Score is 0.0 (not achieved) or 1.0 (achieved).
- **Currency-based**: Same formula with currency values.

Objective-level scores are computed as the weighted average of their Key Result scores, using the weights defined in the Objective Setting Context.

Score finalization occurs during end-of-cycle grading and is irreversible. The ScoringService (a domain service) handles calculation and finalization logic.

### KPI and KRI Integration

Key Performance Indicators (KPIs) and Key Risk Indicators (KRIs) from external systems can serve as data sources for Key Results:

- **KPIs** measure operational performance (e.g., customer satisfaction score, revenue growth rate). When a Key Result is linked to a KPI, progress updates can be automatically ingested from the KPI data source.
- **KRIs** measure risk exposure (e.g., system downtime percentage, compliance violation count). KRI integration enables risk-aware OKR tracking where Key Results that depend on risk factors are automatically flagged when KRI thresholds are breached.

Integration with KPI and KRI systems is mediated through an Anti-Corruption Layer that translates external data formats and semantics into the OKR domain model.

## Domain Events Produced

- **CheckInRecorded**: When a progress check-in is recorded for a Key Result.
- **ProgressUpdated**: When a Key Result's current value changes (manual or automated).
- **ConfidenceChanged**: When a confidence level assessment changes from the previous check-in.
- **ScoreCalculated**: When an interim score is computed during the cycle.
- **ScoreFinalized**: When a Key Result's end-of-cycle score is locked.

## Domain Events Consumed

- **KeyResultDefined**: From the Objective Setting Context, to initialize tracking for a new Key Result.
- **KeyResultModified**: From the Objective Setting Context, to update tracking parameters (target, baseline).
- **CycleClosed**: From the Review & Cadence Context, to trigger score finalization.

## Relationships to Other Topics

- **Objective Setting Context**: Key Results are defined in the [Objective Setting Context](../objective-setting-context/) and tracked here.
- **Review & Cadence Context**: Check-in schedules and cycle transitions are managed by the [Review & Cadence Context](../review-cadence-context/).
- **Analytics & Reporting Context**: Progress data feeds into the [Analytics & Reporting Context](../analytics-reporting-context/) for dashboards and trend analysis.
- **Domain Services**: The ScoringService resides in this context. See [Domain Services](../domain-services/).
- **Value Objects**: Score, ConfidenceLevel, and ProgressUpdate are defined in [Value Objects](../value-objects/).
- **Domain Events**: Events produced and consumed are cataloged in [Domain Events](../domain-events/).
- **Anti-Corruption Layer**: KPI/KRI integration uses ACLs. See [Anti-Corruption Layer](../anti-corruption-layer/).
- **OKR Standards**: KPI and KRI frameworks are explained in [OKR Standards](../okr-standards/).

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003. Chapter 14: Maintaining Model Integrity.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013. Chapter 2: Domains, Subdomains, and Bounded Contexts.
- Doerr, John. "Measure What Matters: How Google, Bono, and the Gates Foundation Rock the World with OKRs." Portfolio/Penguin, 2018. Chapters on tracking and scoring.
- Parmenter, David. "Key Performance Indicators: Developing, Implementing, and Using Winning KPIs." Wiley, 2015.
- Wodtke, Christina. "Radical Focus: Achieving Your Most Important Goals with Objectives and Key Results." Cucina Media, 2016.
