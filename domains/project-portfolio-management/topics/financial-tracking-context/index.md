# Financial Tracking Context in Project Portfolio Management

## Overview

The Financial Tracking Context manages the financial aspects of project and portfolio management: budgets, cost tracking, actuals, forecasts, earned value analysis, ROI calculations, and financial reporting.

## Core Responsibilities

- **Budget Management**: Creating, approving, and maintaining project and portfolio budgets
- **Cost Tracking**: Recording actual costs against budget categories
- **Earned Value Management**: Calculating performance metrics (SPI, CPI, EAC, ETC, VAC)
- **Forecasting**: Projecting final costs based on current performance trends
- **ROI Analysis**: Calculating return on investment and net present value for project proposals
- **Financial Reporting**: Producing cost variance reports, budget-vs-actuals, and portfolio financial summaries

## Aggregates

### Budget Aggregate

- **Root**: Budget (identified by BudgetID, linked to ProjectID)
- **Internal entities**: BudgetLineItem (category, planned amount, period)
- **Value objects**: CurrencyAmount, FiscalPeriod, CostCategory (labor, materials, services, overhead)
- **States**: Draft → Approved → Active → Revised → Closed
- **Invariants**: Budget total = sum of line items; approved budgets require change request for revision; closed budgets are immutable

### CostRecord Aggregate

- **Root**: CostRecord (identified by RecordID)
- **Value objects**: CurrencyAmount, CostCategory, CostDate, CostSource (timesheet, invoice, allocation)
- **Invariants**: Must reference valid budget line item; amount must be positive; cost date within project period

### Forecast Aggregate

- **Root**: Forecast (identified by ForecastID, linked to BudgetID)
- **Value objects**: EarnedValueMetrics, CurrencyAmount, ForecastDate
- **Invariants**: EAC ≥ AC (estimate at completion cannot be less than already spent); forecast date must be in the future

## Domain Events

| Event | Consumers |
|-------|-----------|
| BudgetApproved | Execution, Resource |
| CostRecorded | EVM calculation, Variance analysis |
| BudgetExceeded | Governance (escalation), Project manager |
| ForecastUpdated | Governance (portfolio financials) |
| BudgetRevised | Execution (plan update) |
| ROICalculated | Governance (project evaluation) |

## Earned Value Management (EVM)

Key metrics calculated by the EarnedValueCalculator domain service:

| Metric | Formula | Meaning |
|--------|---------|---------|
| PV (Planned Value) | Baseline budget for work scheduled | What should have been spent |
| EV (Earned Value) | Baseline budget for work completed | Value of work done |
| AC (Actual Cost) | Actual cost of work completed | What was actually spent |
| SV (Schedule Variance) | EV - PV | Schedule ahead/behind |
| CV (Cost Variance) | EV - AC | Under/over budget |
| SPI (Schedule Performance Index) | EV / PV | Schedule efficiency |
| CPI (Cost Performance Index) | EV / AC | Cost efficiency |
| EAC (Estimate at Completion) | BAC / CPI | Projected total cost |
| ETC (Estimate to Complete) | EAC - AC | Remaining cost |
| VAC (Variance at Completion) | BAC - EAC | Projected final variance |

## Integration

- **Governance → Financial**: Budget approval and project prioritization
- **Execution → Financial**: Progress events drive earned value calculations
- **Resource → Financial**: Resource costs (rates × hours) are a primary cost input
- **Financial → Governance**: Budget warnings and forecasts feed portfolio decisions
- **ERP Finance → Financial**: Actual costs from accounting via ACL

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) — One of five PPM bounded contexts
- [Context Map](../context-map/) — Downstream from Execution, Resource; feeds back to Governance
- [Aggregates](../aggregates/) — Budget, CostRecord, Forecast are aggregate roots
- [Domain Services](../domain-services/) — EarnedValueCalculator
- [Value Objects](../value-objects/) — CurrencyAmount, EarnedValueMetrics
- [PPM Standards](../ppm-standards/) — EVM from PMI standards

## References

- PMI. "PMBOK Guide." 7th ed. PMI, 2021.
- PMI. "Practice Standard for Earned Value Management." 2nd ed. PMI, 2011.
- Fleming, Quentin W. & Koppelman, Joel M. "Earned Value Project Management." 4th ed. PMI, 2010.
- Evans, Eric. "Domain-Driven Design." Addison-Wesley, 2003.
