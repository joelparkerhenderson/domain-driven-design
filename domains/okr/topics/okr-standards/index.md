# OKR Standards

## Overview

The OKR domain draws on a rich ecosystem of management frameworks, quality methodologies, and strategic planning tools that complement and reinforce the Objectives and Key Results methodology. These standards inform how objectives are defined, how progress is measured, how quality is ensured, and how strategic decisions are made. Each framework contributes specific concepts that are modeled as value objects, domain services, or process patterns within the OKR bounded contexts.

This topic catalogs each framework, explains its core concepts, and describes how it integrates with the OKR domain model.

## Goal-Setting and Strategic Planning Frameworks

### SMART -- Specific, Measurable, Actionable, Relatable, Timely

SMART is the foundational quality standard for Key Result definition. Each Key Result must satisfy all five SMART dimensions before its parent Objective can be approved. In the domain model, SMART is represented as the SMARTCriteria value object, applied during objective setting and validated before lifecycle state transitions.

**OKR Integration**: The Objective Setting Context enforces SMART compliance as a gate for Draft-to-Approved transitions.

### OKR -- Objective & Key Result

The OKR methodology, popularized by Andy Grove at Intel and evangelized by John Doerr at Google, structures goal-setting into qualitative Objectives paired with quantitative Key Results. The entire domain model is built around this framework, with Objectives and Key Results as primary entities, scored on a 0.0-to-1.0 scale.

**OKR Integration**: The core domain model. All bounded contexts serve the OKR lifecycle.

### KPI -- Key Performance Indicator

KPIs are quantitative metrics that measure ongoing operational performance. Unlike Key Results (which are time-bounded targets), KPIs are persistent measures that track business health over time. KPIs often serve as data sources for Key Results.

**OKR Integration**: The Key Result Tracking Context integrates with KPI systems through Anti-Corruption Layers for automated progress tracking.

### KRI -- Key Risk Indicator

KRIs measure the level of risk exposure in specific areas. They complement KPIs by providing a risk-oriented lens. When KRI thresholds are breached, associated Key Results may be flagged as at-risk.

**OKR Integration**: The Key Result Tracking Context monitors KRI feeds to trigger ConfidenceLevel adjustments and AlertTriggered events.

### CSF -- Critical Success Factor

CSFs identify the essential conditions that must be met for an objective to succeed. They inform objective setting by ensuring that prerequisites and dependencies are recognized during planning.

**OKR Integration**: CSFs inform the Objective Setting Context by guiding which Key Results are defined and ensuring critical conditions are tracked.

### OGSM -- Objectives, Goals, Strategies, Measures

OGSM provides a four-level strategic planning hierarchy. Objectives state what to achieve, Goals quantify the objectives, Strategies describe how to achieve them, and Measures track execution. OGSM's hierarchical decomposition maps naturally to OKR alignment and cascading.

**OKR Integration**: The Alignment & Cascading Context uses OGSM principles to validate that cascaded objectives maintain strategic coherence across organizational levels.

### GIST -- Goals, Ideas, Steps, Tasks

GIST bridges strategic goals with tactical execution. Goals are high-level outcomes, Ideas are hypothesized approaches, Steps are time-bounded implementations, and Tasks are specific work items. GIST complements OKRs by connecting objectives to execution planning.

**OKR Integration**: GIST patterns inform the integration between OKR systems and project management tools described in [Integration Patterns](../integration-patterns/).

## Process Improvement Frameworks

### OODA -- Observe, Orient, Decide, Act

OODA is a decision-making loop originally developed for military strategy. In OKR practice, each check-in follows the OODA pattern: Observe current measurements, Orient by comparing to targets and trends, Decide on corrective actions, and Act before the next check-in.

**OKR Integration**: The check-in process in the Key Result Tracking Context and Review & Cadence Context embodies the OODA loop.

### PDCA -- Plan, Do, Check, Act

PDCA is the foundational continuous improvement cycle. It maps directly to OKR cadence: Plan (set objectives), Do (execute), Check (review progress), Act (adjust and learn). The Review & Cadence Context explicitly integrates PDCA into its cycle management.

**OKR Integration**: The Review & Cadence Context uses PDCA as its operational model for cycle transitions and retrospectives.

### DMAIC -- Define, Measure, Analyze, Improve, Control

DMAIC is the Six Sigma process improvement methodology. In OKR practice, DMAIC structures how underperforming Key Results are diagnosed and improved: Define the gap, Measure current performance, Analyze root causes, Improve the process, and Control to sustain gains.

**OKR Integration**: DMAIC informs retrospective analysis in the Review & Cadence Context and root-cause analysis in the Analytics & Reporting Context.

## Quality Frameworks

### CTP -- Critical To Process

CTP identifies process characteristics that must be maintained for operational effectiveness. CTP analysis ensures that Key Results measuring process performance target the right process variables.

**OKR Integration**: CTP analysis informs Key Result definition in the Objective Setting Context for process-oriented objectives.

### CTQ -- Critical To Quality

CTQ identifies quality characteristics that matter most to customers or stakeholders. CTQ analysis ensures Key Results are aligned with genuine quality outcomes rather than vanity metrics.

**OKR Integration**: CTQ analysis guides the definition of customer-facing Key Results, ensuring measurable quality standards.

## Decision and Analysis Frameworks

### QOC -- Questions, Options, Criteria

QOC is a decision rationale framework. During objective setting and mid-cycle reviews, QOC structures decision-making: articulate the Question, enumerate Options, and evaluate against Criteria.

**OKR Integration**: QOC structures decision records when objectives are modified, retired, or when scoring disputes arise.

### SWOT -- Strengths, Weaknesses, Opportunities, Threats

SWOT analysis evaluates internal capabilities (Strengths, Weaknesses) and external factors (Opportunities, Threats). The Analytics & Reporting Context uses SWOT dimensions to categorize OKR performance insights.

**OKR Integration**: The Analytics & Reporting Context maps health metrics to SWOT categories for strategic reporting.

### PEST -- Political, Economic, Social, Technological

PEST analysis examines external environmental factors that affect organizational performance. When explaining score variances, PEST provides context for factors outside the team's control.

**OKR Integration**: The Analytics & Reporting Context incorporates PEST factors when generating performance reports and explaining external influences on OKR outcomes.

### RAID -- Risks, Assumptions, Issues, Dependencies

RAID is a project management tracking framework. In OKR practice, RAID captures the risks to Key Result achievement, assumptions underlying targets, issues encountered during execution, and dependencies on other teams or systems.

**OKR Integration**: RAID elements are tracked in check-in narratives and blockers within the Key Result Tracking Context.

### RICE -- Reach, Impact, Confidence, Effort

RICE is a prioritization framework that scores initiatives by Reach, Impact, Confidence, and Effort. RICE can be used during objective setting to prioritize which objectives to pursue when capacity is limited.

**OKR Integration**: RICE scoring informs Priority assignment in the Objective Setting Context.

## Change Management and Learning Frameworks

### ADDIE -- Analyze, Design, Develop, Implement, Evaluate

ADDIE is an instructional design framework. In the OKR domain, ADDIE structures the development of OKR training programs and the rollout of new OKR practices within an organization.

**OKR Integration**: ADDIE guides OKR adoption programs and coaching practices.

### ADKAR -- Awareness, Desire, Knowledge, Ability, Reinforcement

ADKAR is a change management framework. Implementing OKR practice in an organization is a change initiative. ADKAR provides a structured approach: build Awareness of why OKRs matter, create Desire to adopt them, provide Knowledge of how to write and track OKRs, build Ability through practice, and Reinforce through consistent cadence and recognition.

**OKR Integration**: ADKAR structures OKR rollout and adoption strategies.

## Operational and Knowledge Frameworks

### ITIL -- Information Technology Infrastructure Library

ITIL provides IT service management best practices. For technology organizations, ITIL processes (incident management, change management, service level management) generate KPIs and KRIs that can serve as Key Result data sources.

**OKR Integration**: ITIL service metrics integrate with the Key Result Tracking Context through Anti-Corruption Layers.

### PERT -- Program Evaluation Review Technique

PERT is a project scheduling technique that estimates task durations using optimistic, pessimistic, and most-likely estimates. PERT analysis can inform DateRange estimation for objectives and forecast Key Result achievement timelines.

**OKR Integration**: PERT estimates inform DateRange value objects and progress velocity calculations in the Analytics & Reporting Context.

### SOP -- Standard Operating Procedure

SOPs document standardized processes. In OKR practice, SOPs define the standard processes for objective setting ceremonies, check-in procedures, grading protocols, and retrospective facilitation.

**OKR Integration**: SOPs codify the operational processes of the Review & Cadence Context.

### BOK -- Book of Knowledge

A BOK is a comprehensive reference for a professional discipline. The OKR domain's BOK encompasses the collected knowledge of OKR methodology, scoring guidelines, alignment best practices, and organizational adoption playbooks.

**OKR Integration**: The BOK serves as the reference foundation for all OKR domain practices and ubiquitous language definitions.

### DR -- Decision Record

Decision Records document the rationale behind significant decisions. In OKR practice, DRs capture why objectives were chosen, why targets were set at specific levels, why objectives were retired, and how scoring disputes were resolved.

**OKR Integration**: Decision Records are attached to Objectives and ReviewSessions as narrative artifacts, supporting audit trails and organizational learning.

## Relationships to Other Topics

- **Objective Setting Context**: SMART, CSF, CTP, CTQ, RICE, and GIST inform objective definition. See [Objective Setting Context](../objective-setting-context/).
- **Key Result Tracking Context**: KPI, KRI, OODA, and RAID inform progress tracking. See [Key Result Tracking Context](../key-result-tracking-context/).
- **Review & Cadence Context**: PDCA, DMAIC, and SOP structure review processes. See [Review & Cadence Context](../review-cadence-context/).
- **Analytics & Reporting Context**: SWOT, PEST, and PERT inform reporting. See [Analytics & Reporting Context](../analytics-reporting-context/).
- **Alignment & Cascading Context**: OGSM informs cascading logic. See [Alignment & Cascading Context](../alignment-cascading-context/).
- **Value Objects**: SMARTCriteria, Score, and ConfidenceLevel embody these standards. See [Value Objects](../value-objects/).
- **Ubiquitous Language**: Framework terminology is part of the ubiquitous language. See [Ubiquitous Language](../ubiquitous-language/).

## References

- Doerr, John. "Measure What Matters: How Google, Bono, and the Gates Foundation Rock the World with OKRs." Portfolio/Penguin, 2018.
- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
- Doran, George T. "There's a S.M.A.R.T. Way to Write Management's Goals and Objectives." Management Review, 1981.
- Deming, W. Edwards. "Out of the Crisis." MIT Press, 1986. Foundation of PDCA.
- Pyzdek, Thomas and Keller, Paul. "The Six Sigma Handbook." McGraw-Hill, 2014. Foundation of DMAIC.
- Boyd, John. "A Discourse on Winning and Losing." Air University Press, 2018. Foundation of OODA.
- Prosci. "ADKAR: A Model for Change in Business, Government, and Our Community." Prosci, 2006.
- Axelos. "ITIL Foundation: ITIL 4 Edition." TSO, 2019.
