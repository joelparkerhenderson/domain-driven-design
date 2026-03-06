# PPM Standards in Project Portfolio Management

## Overview

PPM standards provide frameworks, methodologies, and best practices that inform the domain model design. These standards define the vocabulary, processes, and metrics used by project management professionals.

## Portfolio Management Standards

### PMI Standard for Portfolio Management

Published by the Project Management Institute. Defines portfolio management processes:

- **Defining**: Strategic alignment, portfolio charter, portfolio roadmap
- **Aligning**: Portfolio optimization, portfolio balancing, authorization
- **Monitoring**: Portfolio reporting, strategic change management

**DDD mapping**: Portfolio Governance Context models these processes. ScoringModel, StrategicObjective, and InvestmentCategory value objects reflect PMI portfolio concepts.

### MoP (Management of Portfolios)

AXELOS framework (formerly OGC). Defines principles, cycles, and practices for portfolio management.

**DDD mapping**: Complementary to PMI standard; informs governance gate review design.

## Project Management Standards

### PMBOK Guide (7th Edition)

PMI's body of knowledge for project management. Organized around principles and performance domains:

- **Performance Domains**: Stakeholders, Team, Development Approach, Planning, Project Work, Delivery, Measurement, Uncertainty
- **Key Concepts**: WBS, critical path, earned value, risk management

**DDD mapping**: Execution Management Context models PMBOK concepts. WBSElement, CriticalPath, and EarnedValueMetrics value objects. Project lifecycle states align with PMBOK process groups.

### PRINCE2

AXELOS structured project management method with defined roles, processes, and themes:

- **Themes**: Business Case, Organization, Quality, Plans, Risk, Change, Progress
- **Processes**: Starting Up, Initiating, Controlling, Managing Product Delivery, Managing Stage Boundaries, Closing

**DDD mapping**: GateReview aggregate models stage-gate decisions. Business Case is a value object in Governance.

### Agile Frameworks

#### Scrum

Iterative framework with sprints, backlogs, and ceremonies.

**DDD mapping**: Execution context supports sprint-based planning with Sprint, Backlog, and UserStory concepts.

#### SAFe (Scaled Agile Framework)

Scales agile to portfolio level with Agile Release Trains, Program Increments, and portfolio Kanban.

**DDD mapping**: Portfolio Governance Context can model SAFe portfolio Kanban; Execution Context supports PI planning.

## Maturity Models

### OPM3 (Organizational Project Management Maturity Model)

PMI's maturity model for assessing organizational PPM capability across standardization, measurement, control, and continuous improvement.

### P3M3 (Portfolio, Programme, and Project Management Maturity Model)

AXELOS maturity model with five levels per perspective (management control, benefits management, financial management, stakeholder management, risk management, organizational governance, resource management).

## Earned Value Management Standards

### ANSI/EIA-748

U.S. standard for Earned Value Management Systems (EVMS). Defines 32 criteria in five categories: organization, planning/budgeting/scheduling, accounting, analysis/management reports, revisions/data maintenance.

**DDD mapping**: EarnedValueCalculator domain service implements EVM calculations per this standard.

## Relationships to Other Topics

- [Value Objects](../value-objects/) — EarnedValueMetrics, ScoringModel, WBSElement from standards
- [Bounded Contexts](../bounded-contexts/) — Context design informed by standard process areas
- [Domain Services](../domain-services/) — EarnedValueCalculator, CriticalPathAnalyzer from standards
- [Financial Tracking Context](../financial-tracking-context/) — EVM implementation

## References

- PMI. "The Standard for Portfolio Management." 4th ed. Project Management Institute, 2017.
- PMI. "A Guide to the Project Management Body of Knowledge (PMBOK Guide)." 7th ed. PMI, 2021.
- PMI. "Organizational Project Management Maturity Model (OPM3)." 3rd ed. PMI, 2013.
- PMI. "Practice Standard for Earned Value Management." 2nd ed. PMI, 2011.
- AXELOS. "Management of Portfolios (MoP)." TSO, 2011.
- AXELOS. "Managing Successful Projects with PRINCE2." 6th ed. TSO, 2017.
- AXELOS. "Portfolio, Programme and Project Management Maturity Model (P3M3)." AXELOS, 2015.
- Scaled Agile. "SAFe 6.0 Framework." https://www.scaledagileframework.com/
- ANSI/EIA. "ANSI/EIA-748: Earned Value Management Systems." 2019.
