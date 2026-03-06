# Ubiquitous Language

## Overview

Ubiquitous Language is a foundational concept in Domain-Driven Design where all team members -- domain experts, developers, product managers, and stakeholders -- use the same precise terminology when discussing the domain. In the OKR domain, this means that terms like "Objective," "Key Result," "Score," and "Check-In" have exact, agreed-upon definitions that are consistent within each Bounded Context and used in code, documentation, conversations, and user interfaces.

A well-maintained ubiquitous language eliminates ambiguity, reduces translation errors between business and technical teams, and ensures that the software model accurately reflects the domain model.

## Core OKR Terminology

### Across All Contexts

These terms have consistent meaning throughout the OKR domain:

- **Objective**: A qualitative, inspirational goal that describes what an organization, team, or individual wants to achieve within a defined time period. Objectives should be ambitious, actionable, and time-bound.

- **Key Result**: A quantitative, measurable outcome that indicates progress toward an objective. Key results define how success is measured and must be specific enough to be unambiguously evaluated.

- **OKR**: The combination of an Objective and its associated Key Results, forming the fundamental unit of goal management.

- **Owner**: The individual or team accountable for driving progress on a specific objective or key result.

- **Contributor**: A person or team that supports progress on a key result without being the primary owner.

- **Initiative**: A project, task, or workstream that contributes to achieving one or more key results.

### Objective Setting Context

- **Draft Objective**: An objective that has been created but not yet approved for inclusion in an active cycle.

- **Active Objective**: An approved objective currently being pursued within an active cycle.

- **Retired Objective**: An objective that has been intentionally withdrawn before cycle completion.

- **Committed OKR**: An OKR with a high expectation of full achievement (100% target), often tied to business-critical deliverables.

- **Aspirational OKR**: An OKR with intentional stretch, where achieving 60-70% is considered successful. Also called a "moonshot" or "stretch" OKR.

- **SMART Criteria**: The validation framework requiring key results to be Specific, Measurable, Actionable, Relatable, and Timely.

- **OKR Type**: The classification of an OKR as either committed or aspirational, which affects how its score is interpreted.

- **Approval**: The process by which a draft objective is reviewed and authorized for inclusion in an active cycle.

### Key Result Tracking Context

- **Score**: A numerical evaluation of key result achievement on a scale of 0.0 (no progress) to 1.0 (full achievement). The scoring scale provides a common metric across all key results regardless of their individual measurement units.

- **Confidence Level**: A subjective assessment (typically High, Medium, or Low, or expressed as a probability) of the likelihood of achieving a key result by cycle end.

- **Progress Update**: A periodic record of advancement toward a key result, including quantitative data and qualitative narrative.

- **Check-In**: A structured progress review event where key result owners report current status, update confidence levels, and identify blockers.

- **Baseline**: The starting measurement for a key result at the beginning of a cycle.

- **Target**: The desired end-state measurement for a key result.

- **Current Value**: The most recently recorded measurement for a key result.

- **Blocker**: An obstacle preventing progress on a key result that requires escalation or intervention.

- **Automated Tracking**: Key result progress that is updated automatically from integrated data sources (KPI feeds, system metrics) rather than manual entry.

### Alignment & Cascading Context

- **Alignment**: The relationship between objectives at different organizational levels that ensures lower-level goals support higher-level strategy.

- **Cascading**: The practice of deriving lower-level OKRs from higher-level objectives to maintain strategic coherence.

- **Parent Objective**: A higher-level objective from which child objectives are derived through cascading.

- **Child Objective**: A lower-level objective that supports and contributes to a parent objective.

- **Alignment Tree**: The hierarchical structure showing how objectives at all levels connect, from company-wide to team to individual.

- **Alignment Link**: A directed relationship between two objectives indicating that one supports the other.

- **Cross-Team Alignment**: A lateral alignment relationship where objectives from different teams contribute to shared outcomes without a hierarchical parent-child structure.

- **Alignment Gap**: A situation where a strategic objective has no supporting child objectives.

- **Alignment Conflict**: A situation where child objectives contradict or undermine each other or their parent.

- **Dependency**: A relationship where progress on one key result requires completion of work owned by another team.

### Review & Cadence Context

- **OKR Cycle**: A time-bounded period (typically quarterly, sometimes annual) during which objectives and key results are pursued. Also called a "cadence period" or "OKR quarter."

- **Cycle State**: The current phase of a cycle: Planning (objectives being set), Active (work in progress), Scoring (end-of-cycle evaluation), Closed (cycle complete).

- **Cadence**: The recurring rhythm of OKR-related activities, including setting, check-ins, scoring, and retrospectives.

- **Grading Ceremony**: A formal end-of-cycle event where final scores are assigned to all key results and objectives are evaluated.

- **Retrospective**: A review session at the end of an OKR cycle to assess what worked, what did not, and what to improve in the next cycle.

- **Check-In Cadence**: The scheduled frequency of progress reviews, typically weekly or biweekly.

- **Planning Period**: The time at the beginning of a cycle when objectives and key results are drafted, refined, and approved.

- **Scoring Window**: The designated time at the end of a cycle when key results are given final scores.

### Analytics & Reporting Context

- **Health Metric**: An operational measure that must remain within acceptable bounds, distinct from aspirational OKR targets. Health metrics monitor ongoing business health rather than goal progress.

- **Completion Rate**: The percentage of key results that achieved a score above a defined threshold (typically 0.7) within a cycle.

- **Alignment Coverage**: The percentage of strategic objectives that have supporting child objectives.

- **Score Trend**: The trajectory of average scores across multiple cycles, indicating improving or declining goal achievement.

- **OKR Maturity**: A measure of how effectively an organization practices OKR, considering factors like adoption rate, scoring consistency, and alignment depth.

- **Dashboard**: A real-time visual display of OKR performance metrics.

- **Performance Report**: A periodic summary document of OKR outcomes, typically generated at cycle close.

## Framework Terminology

The OKR domain incorporates terminology from related frameworks:

- **KPI (Key Performance Indicator)**: An ongoing metric measuring operational health or business performance, distinct from OKR key results but often used as data sources for tracking.
- **KRI (Key Risk Indicator)**: A metric that signals increasing risk exposure, used alongside KPIs to provide a balanced view.
- **CSF (Critical Success Factor)**: A condition that must be met for a strategy to succeed, often mapped to objectives.
- **PDCA (Plan-Do-Check-Act)**: The continuous improvement cycle applied to OKR reviews.
- **DMAIC (Define-Measure-Analyze-Improve-Control)**: A Six Sigma methodology applicable to OKR process improvement.

## Language Governance

Maintaining the ubiquitous language requires:

1. **Glossary Maintenance**: A living glossary (such as this document) that is updated as the domain evolves.
2. **Code Alignment**: Class names, method names, and variable names in the codebase must use the ubiquitous language. An Objective class should be called `Objective`, not `Goal` or `Target`.
3. **Context Boundaries**: Terms may have different meanings across contexts. "Score" in the KRTC is a specific 0.0-1.0 value; in the ARC, it may be an aggregated statistic. Context qualifiers prevent confusion.
4. **Onboarding**: New team members learn the language as part of domain onboarding.
5. **Continuous Refinement**: Language evolves through domain conversations, Event Storming sessions, and retrospectives.

## Relationships to Other Topics

- **Strategic Design**: The ubiquitous language emerges from and supports the Strategic Design process described in [Strategic Design](../strategic-design/).
- **Bounded Contexts**: Each context defines its own language dialect, as outlined in [Bounded Contexts](../bounded-contexts/).
- **Entities**: Entity names are drawn directly from the ubiquitous language, detailed in [Entities](../entities/).
- **Value Objects**: Value object names reflect domain concepts from the language, detailed in [Value Objects](../value-objects/).
- **Domain Events**: Event names follow the language conventions, documented in [Domain Events](../domain-events/).
- **OKR Standards**: Framework terminology (SMART, KPI, PDCA, etc.) is detailed in [OKR Standards](../okr-standards/).

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003. Chapter 2: Communication and the Use of Language.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013. Chapter 1: Getting Started with DDD (Ubiquitous Language).
- Doerr, John. "Measure What Matters: How Google, Bono, and the Gates Foundation Rock the World with OKRs." Portfolio/Penguin, 2018.
- Wodtke, Christina. "Radical Focus: Achieving Your Most Important Goals with Objectives and Key Results." Cucina Media, 2016.
- Niven, Paul R. and Lamorte, Ben. "Objectives and Key Results: Driving Focus, Alignment, and Engagement with OKRs." Wiley, 2016.
