# Anti-Corruption Layer in Project Portfolio Management

## Overview

ACLs protect PPM bounded contexts from external system concepts. PPM systems integrate with project management tools, HR systems, ERP finance, and time tracking systems.

## PPM ACL Examples

### Project Management Tools → Execution Management

Tools like MS Project, Jira, and Azure DevOps have their own data models. The ACL:

- Translates Jira epics/stories into domain Tasks and Milestones
- Maps MS Project task hierarchies to internal WBS structures
- Converts tool-specific status values to domain RAGStatus
- Normalizes date formats and schedule representations

### HR Systems → Resource Management

HR systems provide employee data with different structures. The ACL:

- Translates HR employee records into domain Resource entities
- Maps HR skill classifications to internal SkillCategory value objects
- Converts HR department structures to internal resource pools
- Handles differences in availability tracking (HR tracks PTO; PPM tracks project availability)

### ERP Finance → Financial Tracking

Accounting systems provide actual cost data. The ACL:

- Translates GL entries into domain CostRecord value objects
- Maps account codes to internal project cost categories
- Converts ERP fiscal periods to PPM reporting periods
- Reconciles ERP actuals with PPM budget structures

### Time Tracking Systems → Resource Management / Financial Tracking

- Translates timesheet entries into domain TimeEntry value objects
- Maps time codes to internal task references
- Converts hourly rates to domain CurrencyAmount value objects

## Relationships to Other Topics

- [Context Map](../context-map/) — ACLs appear on the context map
- [Integration Patterns](../integration-patterns/) — ACL is one integration pattern
- [Bounded Contexts](../bounded-contexts/) — ACLs protect context boundaries

## References

- Evans, Eric. "Domain-Driven Design." Chapter 14. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 3. Addison-Wesley, 2013.
- Hohpe, Gregor & Woolf, Bobby. "Enterprise Integration Patterns." Addison-Wesley, 2003.
