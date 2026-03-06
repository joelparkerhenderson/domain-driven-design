# Risk Management Context in Project Portfolio Management

## Overview

The Risk Management Context handles the identification, assessment, mitigation, and monitoring of risks at both the project and portfolio level.

## Core Responsibilities

- **Risk Identification**: Capturing potential risks from project activities, dependencies, and external factors
- **Risk Assessment**: Evaluating probability and impact using qualitative and quantitative methods
- **Mitigation Planning**: Defining strategies to reduce risk probability or impact (avoid, transfer, mitigate, accept)
- **Risk Monitoring**: Tracking risk status, triggers, and effectiveness of mitigations
- **Risk Reporting**: Generating risk registers, heat maps, and trend analysis
- **Portfolio Risk Aggregation**: Assessing overall portfolio risk exposure and correlation

## Aggregates

### Risk Aggregate

- **Root**: Risk (identified by RiskID)
- **Value objects**: RiskScore (probability × impact), MitigationStrategy, RiskCategory (technical, schedule, cost, resource, external), RiskOwner, TriggerCondition
- **States**: Identified → Assessed → Mitigated → Monitoring → Closed / Materialized
- **Invariants**: Must have owner; mitigation required above risk threshold; materialized risks become issues; risk score must be recalculated when probability or impact changes

### RiskRegister Aggregate (optional, for portfolio-level)

- **Root**: RiskRegister (identified by PortfolioID)
- **Internal**: Aggregated risk summaries
- **Value objects**: PortfolioRiskProfile, RiskAppetite, RiskTolerance

## Domain Events

| Event | Consumers |
|-------|-----------|
| RiskIdentified | Risk register, Project manager |
| RiskAssessed | Risk dashboard |
| RiskMitigated | Risk monitoring |
| RiskEscalated | Governance, Project manager |
| RiskMaterialized | Execution (issue), Financial (cost) |
| RiskClosed | Risk register update |
| PortfolioRiskUpdated | Governance dashboard |

## Risk Assessment Methods

- **Qualitative**: Probability-impact matrix, risk categorization, expert judgment
- **Quantitative**: Monte Carlo simulation, sensitivity analysis, expected monetary value (EMV), decision tree analysis
- **Risk scoring**: Probability (0.0–1.0) × Impact (1–5) = Risk Score → classified as High/Medium/Low

## Mitigation Strategies

- **Avoid**: Change plans to eliminate the risk entirely
- **Transfer**: Shift risk to a third party (insurance, outsourcing)
- **Mitigate**: Reduce probability or impact through proactive actions
- **Accept**: Acknowledge the risk with contingency reserves (active) or no action (passive)

## Integration

- **Execution → Risk**: Project activities are the primary source of risk identification
- **Risk → Execution**: RiskEscalated events trigger corrective actions
- **Risk → Financial**: RiskMaterialized events trigger cost impact assessment
- **Risk → Governance**: Portfolio-level risk aggregation informs governance decisions

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) — One of five PPM bounded contexts
- [Aggregates](../aggregates/) — Risk is an aggregate root
- [Value Objects](../value-objects/) — RiskScore, MitigationStrategy
- [Domain Events](../domain-events/) — Risk events drive Execution and Governance

## References

- PMI. "PMBOK Guide." 7th ed. Chapter on Risk. PMI, 2021.
- PMI. "The Standard for Risk Management in Portfolios, Programs, and Projects." PMI, 2019.
- Hillson, David. "Practical Project Risk Management: The ATOM Methodology." Management Concepts, 2012.
- Evans, Eric. "Domain-Driven Design." Addison-Wesley, 2003.
