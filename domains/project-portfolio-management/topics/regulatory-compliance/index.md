# Regulatory Compliance in Project Portfolio Management

## Overview

PPM systems must support organizational governance requirements, audit standards, data protection regulations, and industry-specific compliance mandates. Compliance requirements shape how decisions are documented, how data is retained, and how access is controlled.

## Governance and Audit Requirements

### Corporate Governance

Organizations must demonstrate that project investments align with strategic objectives and are managed responsibly.

**DDD implications**:

- **Decision audit trail**: Every ProjectApproved, ProjectDeferred, and ProjectCancelled event includes who made the decision, criteria used, and rationale
- **Gate review records**: GateReview aggregate preserves complete records of review participants, criteria evaluations, and decisions
- **Portfolio rebalancing history**: PortfolioRebalanced events document what changed and why
- **Segregation of duties**: Project proposer ≠ project approver; budget requestor ≠ budget approver

### SOX Compliance (for publicly traded companies)

When PPM financial data feeds into corporate financial statements, SOX requirements apply.

**DDD implications**:

- **Budget approval controls**: Budget aggregate enforces approval workflows
- **Cost recording accuracy**: CostRecord aggregates validated against budget categories
- **Change management**: ChangeRequest aggregate documents all scope/budget changes with approvals
- **Immutable financial events**: CostRecorded and BudgetApproved events are immutable once posted

### Internal Audit

Internal auditors review PPM processes for compliance with organizational policies.

**DDD implications**:

- Domain events provide complete, tamper-proof audit trails
- All state changes to portfolios, projects, and budgets are traceable
- Access logs track who viewed or modified PPM data

## Data Protection

### GDPR / Privacy Regulations

PPM systems store personal data about resources (employees) and stakeholders.

**DDD implications**:

- **Data minimization**: Resource aggregate stores only necessary personal data
- **Right to erasure**: Support anonymization of resource data when requested
- **Consent tracking**: Stakeholder records include communication consent status
- **Access controls**: Role-based access at bounded context boundaries

## Industry-Specific Requirements

### Government/Public Sector

- **Earned Value Management**: U.S. federal agencies require EVMS per ANSI/EIA-748 for contracts over $20M
- **Federal Acquisition Regulation (FAR)**: Compliance requirements for government contracts
- **IT Dashboard**: OMB requires reporting on federal IT investments

### Regulated Industries (Healthcare, Financial Services)

- **Validation requirements**: Project deliverables may require documented validation
- **Change control**: Stricter change management processes for regulated environments
- **Traceability**: Full traceability from requirements to deliverables

## Compliance in Domain Design

### Audit Events

Key events that form the compliance audit trail:

- `ProjectProposed` (proposer, business case, date)
- `ProjectApproved` (approver, criteria scores, rationale, date)
- `BudgetApproved` (approver, amount, categories, date)
- `ChangeRequestApproved` (approver, impact assessment, date)
- `CostRecorded` (recorder, amount, category, source, date)
- `GateReviewCompleted` (reviewers, decision, criteria, date)

### Access Control

Each bounded context enforces access policies:

- **Governance**: Portfolio managers, PMO directors, executives
- **Execution**: Project managers, team leads, contributors
- **Financial**: Financial controllers, PMO analysts
- **Resource**: Resource managers, HR liaisons
- **Risk**: Risk managers, project managers

### Retention Policies

- Financial records: Retained per organizational policy (typically 7+ years)
- Project records: Retained for audit and lessons-learned purposes
- Resource data: Subject to privacy regulations (GDPR retention limits)

## Relationships to Other Topics

- [Domain Events](../domain-events/) — Events form audit trails
- [Aggregates](../aggregates/) — Compliance shapes invariants
- [Portfolio Governance Context](../portfolio-governance-context/) — Decision audit trails
- [Financial Tracking Context](../financial-tracking-context/) — Financial compliance
- [PPM Standards](../ppm-standards/) — EVM and governance standards

## References

- PMI. "The Standard for Portfolio Management." PMI, 2017.
- PMI. "Governance of Portfolios, Programs, and Projects: A Practice Guide." PMI, 2016.
- U.S. Congress. "Sarbanes-Oxley Act of 2002."
- European Parliament. "General Data Protection Regulation (GDPR)."
- ANSI/EIA. "ANSI/EIA-748: Earned Value Management Systems."
- OMB. "Federal IT Dashboard and Investment Management." Circular A-11.
