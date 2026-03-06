# Human Resources Context in Enterprise Resource Planning

## Overview

The Human Resources Context manages the people side of the organization: employee records, payroll processing, benefits administration, recruitment, time and attendance, training, and workforce compliance.

## Core Responsibilities

- **Employee Management**: Maintaining employee records, organizational assignments, and employment history
- **Payroll Processing**: Calculating gross-to-net pay, tax withholding, deductions, and disbursements
- **Benefits Administration**: Enrolling employees in health, dental, vision, retirement, and other benefit plans
- **Recruitment**: Managing job postings, applicant tracking, interviewing, and onboarding
- **Time and Attendance**: Tracking work hours, overtime, PTO accrual, and absence management
- **Compliance**: Ensuring adherence to labor laws, tax regulations, and reporting requirements

## Aggregates

### Employee Aggregate

- **Root**: Employee (identified by EmployeeID)
- **Value objects**: Address, Compensation (salary/hourly rate, pay frequency), BenefitsEnrollment, TaxWithholding (federal, state, local), BankAccount (direct deposit)
- **States**: Onboarding → Active → Leave → Terminated / Retired
- **Invariants**: Only one active employment record at a time; compensation changes require effective date; terminated employees cannot accrue benefits

### PayrollRun Aggregate

- **Root**: PayrollRun (identified by RunID, pay period)
- **Internal entities**: PayStub (per employee)
- **Value objects**: Money, DateRange (pay period), TaxCalculation
- **Invariants**: All active employees must be included; gross pay minus deductions equals net pay; completed runs are immutable

### Position Aggregate

- **Root**: Position (identified by PositionID)
- **Value objects**: JobTitle, Department, PayGrade, ReportingRelationship
- **Invariants**: One active incumbent per position (unless position allows multiple); pay grade must match compensation range

## Domain Events

| Event | Trigger | Consumers |
|-------|---------|-----------|
| EmployeeHired | New hire processed | Payroll setup, IT (account creation), Facilities |
| CompensationChanged | Salary/rate adjusted | Payroll recalculation |
| BenefitsEnrolled | Benefits election made | Benefits provider, Payroll (deductions) |
| PayrollProcessed | Pay period completed | Finance (journal entries), Banking (disbursements) |
| EmployeeTerminated | Employment ended | Payroll (final pay), IT (access revocation), Benefits |
| TimeApproved | Timesheet approved | Payroll (hours input) |

## Integration with Other Contexts

- **HR → Finance**: PayrollProcessed creates salary expense journal entries with cost center allocation
- **HR → Manufacturing**: Employee skills and certifications inform work center assignments
- **HR ← External**: Tax authority tables, benefits provider APIs via anti-corruption layers

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) — One of six ERP bounded contexts
- [Context Map](../context-map/) — Upstream to Finance for payroll
- [Aggregates](../aggregates/) — Employee, PayrollRun, Position are aggregate roots
- [Regulatory Compliance](../regulatory-compliance/) — Labor laws, tax regulations, GDPR for employee data

## References

- Evans, Eric. "Domain-Driven Design." Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
- Dessler, Gary. "Human Resource Management." Pearson, 2019.
