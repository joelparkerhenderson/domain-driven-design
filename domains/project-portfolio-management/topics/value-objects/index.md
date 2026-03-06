# Value Objects in Project Portfolio Management

## Overview

Value objects in PPM represent the quantities, scores, statuses, and descriptive data that characterize portfolio decisions and project execution.

## PPM Value Objects

### CurrencyAmount

- **Attributes**: Amount (decimal), currency (ISO 4217)
- **Usage**: Budgets, costs, ROI calculations, resource rates
- **Example**: `CurrencyAmount(250000.00, "USD")`

### DateRange

- **Attributes**: Start date, end date
- **Validation**: Start before end
- **Usage**: Project timelines, allocation periods, fiscal periods, contract terms

### RAGStatus

- **Attributes**: Schedule status (R/A/G), budget status (R/A/G), scope status (R/A/G), overall status
- **Usage**: Project health reporting, portfolio dashboards
- **Example**: `RAGStatus(schedule: "Green", budget: "Amber", scope: "Green", overall: "Amber")`

### Priority

- **Attributes**: Rank (integer), category (must-have, should-have, nice-to-have), score (from scoring model)
- **Usage**: Portfolio prioritization, resource allocation decisions

### RiskScore

- **Attributes**: Probability (0.0–1.0), impact (1–5 scale), score (probability × impact)
- **Behavior**: classify() returns High/Medium/Low based on score thresholds
- **Usage**: Risk assessment, risk heat maps

### ScoringModel

- **Attributes**: List of criteria with weights (strategic alignment, financial return, risk, urgency)
- **Behavior**: score(project) returns weighted total score
- **Usage**: Portfolio governance for project selection and ranking

### EarnedValueMetrics

- **Attributes**: PV (planned value), EV (earned value), AC (actual cost), SPI, CPI, EAC, ETC
- **Usage**: Financial tracking, project performance measurement
- **Example**: `EarnedValueMetrics(PV: 100000, EV: 90000, AC: 95000)` → SPI=0.9, CPI=0.95

### Dependency

- **Attributes**: Source task/milestone, target task/milestone, type (finish-to-start, start-to-start, etc.), lag
- **Usage**: Project scheduling, critical path analysis

### Skill

- **Attributes**: Name, category, proficiency level
- **Usage**: Resource matching, capacity planning

### WBSElement

- **Attributes**: Code (hierarchical: 1.2.3), name, level
- **Usage**: Work breakdown structure in project planning

## Relationships to Other Topics

- [Entities](../entities/) — Entities contain value objects
- [Aggregates](../aggregates/) — Value objects within aggregates
- [PPM Standards](../ppm-standards/) — EVM, WBS from PMI standards

## References

- Evans, Eric. "Domain-Driven Design." Chapter 5. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 6. Addison-Wesley, 2013.
- PMI. "PMBOK Guide." 7th ed. PMI, 2021.
